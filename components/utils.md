---
icon: screwdriver-wrench
---

# Utility

## Overview

The `Utility` component provides specialized market analysis tools that are used by the QuantAIAgent. These tools enable detailed analysis of cryptocurrency markets, tokens, and trends. Each tool is designed to handle specific aspects of market analysis and can be used independently or through the agent.

## Example Utility Function:

```python
def analyze_contract(contract_address):
    """
    Analyzes a Solana token contract using DexScreener data.
    """
    dex = DexClient()
    dex_data = dex.get_token_pairs("solana", contract_address)
    if not dex_data:
        return {"error": f"Data for {contract_address} not available on DexScreener"}
    return {"analysis": dex_data}
```

## Component

* **Utility Name:** Function identifier.&#x20;
* **Utility Description:** Brief explanation of its role

## Methods

* `analyze_contract(contract_address)` – Analyzes a Solana token contract using DexScreener data.
* `analyze_ticker(ticker)` – Extracts and analyzes token data based on its ticker symbol.
* `analyze_market_trends(top_n_tokens)` – Retrieves and analyzes trending tokens from Pump.fun along with DexScreener data.
* `analyze_meta_market(top_n_tokens, meta)` – Retrieves and analyzes trending tokens from Pump.fun for a given meta category.

### Basic Concepts

#### Tool Types

1. **Contract Analysis**
   * Token contract evaluation
   * Code analysis
   * Security assessment
2. **Market Analysis**
   * Price analysis
   * Volume analysis
   * Trend detection
3. **Meta Analysis**
   * Category analysis
   * Token classification
   * Market segment evaluation

### Architecture

```
tools/
├── market_analysis.py    # Market analysis tools
└── __init__.py          # Package initialization
```

### Basic Usage

#### Direct Tool Usage

```python
from tempus.tools.market_analysis import (
    analyze_contract,
    analyze_ticker,
    analyze_market_trends,
    analyze_meta_market
)

# Analyze a contract
result = analyze_contract("contract_address")

# Analyze a ticker
analysis = analyze_ticker("BTC")

# Get market trends
trends = analyze_market_trends(top_n=10)

# Analyze meta market
ai_tokens = analyze_meta_market(meta="ai", top_n=5)
```

#### Tool Configuration

```python
from langchain_openai import ChatOpenAI

# Set LLM for tools
llm = ChatOpenAI(model="gpt-4")
set_llm(llm)
```

### Tool Details

#### Contract Analysis Tool

```python
@tool
def analyze_contract(contract_address: str) -> Dict[str, Any]:
    """
    Analyzes a Solana token contract using DexScreener data.

    Example:
        analyze_contract("GQ82HnTvrWoe1q8AjSJ1dr5vf5GFjDUqcztb9Uyrpump")

    Args:
        contract_address (str): The contract address of the token.

    Returns:
        Dict[str, Any]: Analysis report or an error message if data is unavailable.
    """
    dex = DexClient()
    dex_data = dex.get_token_pairs("solana", contract_address)
    
    if not dex_data:
        return {"error": f"Data for {contract_address} not available on DexScreener"}

    system_prompt = QUANT_AI_PROMPT.format(historical_data=json.dumps(dex_data[0]))

    chat_messages = [
        SystemMessage(content=system_prompt),
        HumanMessage(content="Analyze the provided data and generate a report.")
    ]

    response = llm.invoke(chat_messages)
    return {"analysis": response}
```

#### Ticker Analysis Tool

```python
@tool
def analyze_ticker(ticker: str) -> Dict[str, Any]:
    """
    Analyzes a token based on its ticker symbol using DexScreener data.

    Example:
        analyze_ticker("$BTC")  → Extracts "BTC" and searches for its data.

    Args:
        ticker (str): The token ticker symbol (e.g., "$BTC", "$SOL").

    Returns:
        Dict[str, Any]: Analysis report or an error message if data is unavailable.
    """
    extracted_ticker = re.sub(r'^\$', '', ticker)

    dex = DexClient()
    search_results = dex.search_pairs(extracted_ticker)
    
    dex_data = search_results.get("pairs", [])
    if not dex_data:
        return {"error": f"Data for {extracted_ticker} not available on DexScreener"}
    
    if dex_data[0].get("chainId") != "solana":
        return {"error": f"Data for {extracted_ticker} is not on the Solana chain"}

    system_prompt = QUANT_AI_PROMPT.format(historical_data=json.dumps(dex_data[0]))

    chat_messages = [
        SystemMessage(content=system_prompt),
        HumanMessage(content="Analyze the provided data and generate a report.")
    ]

    response = llm.invoke(chat_messages)
    return {"analysis": response}
```

#### Market Trends Tool

```python
@tool
def analyze_market_trends(top_n_tokens: int) -> Dict[str, Any]:
    """
    Retrieves and analyzes trending tokens from Pump.fun along with DexScreener data.

    Example:
        analyze_market_trends(5) → Analyzes the top 5 trending tokens.

    Args:
        top_n_tokens (int): Number of top trending tokens to analyze.

    Returns:
        Dict[str, Any]: Market trend analysis report.
    """
    pump_fun = PumpFunClient()
    pump_fun_data = pump_fun.get_trending_tokens(top_n_tokens)
    
    if not pump_fun_data:
        return {"error": "No trending tokens found on Pump.fun"}

    final_data = []
    dex = DexClient()

    for token in pump_fun_data[1:]:  # Skip first element as per original code
        data = {
            'pumpfunData': token,
            'dexscreenerData': dex.get_token_pairs("solana", token['mint'])
        }
        final_data.append(data)

    system_prompt = QUANT_PUMP_FUN_PROMPT.format(historical_data=json.dumps(final_data))

    chat_messages = [
        SystemMessage(content=system_prompt),
        HumanMessage(content="Analyze the latest market trends based on provided data.")
    ]

    response = llm.invoke(chat_messages)
    return {"market_analysis": response}
```

#### Meta Market Tool

```python
@tool
def analyze_meta_market(top_n_tokens: int=10, meta: str="ai") -> Dict[str, Any]:
    """
    Retrieves and analyzes trending tokens from Pump.fun for a given meta category.

    Example:
        analyze_meta_market(5, "ai") → Analyzes the top 5 trending tokens in AI meta.

    Args:
        top_n_tokens (int): Number of top trending tokens to analyze.
        meta (str): Meta category from Pump.fun (e.g., "ai", "gaming", "meme").

    Returns:
        Dict[str, Any]: Market trend analysis report.
    """
    try:
        pump_fun = PumpFunClient()
        tokens = pump_fun.get_meta_tokens(meta, top_n_tokens)

        if not tokens:
            return {"error": f"No trending tokens found in {meta} meta on Pump.fun"}

        final_data = []
        dex = DexClient()
        
        for token in tokens:
            data = dex.get_token_pairs("solana", token['id'])
            final_data.append(data)

        system_prompt = QUANT_PUMP_FUN_PROMPT.format(historical_data=json.dumps(final_data))

        chat_messages = [
            SystemMessage(content=system_prompt),
            HumanMessage(content="Analyze the latest market trends based on provided data.")
        ]

        response = llm.invoke(chat_messages)
        return {"market_analysis": response}
    except Exception as e:
        return {"error": f"An error occurred: {str(e)}"}
```
