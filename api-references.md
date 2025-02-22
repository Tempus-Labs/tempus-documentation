---
icon: webhook
---

# API References

## QuantAIAgent

The main class for interacting with the Tempus framework.

### Constructor

```python
QuantAIAgent(llm_provider: str = "openai", model_name: Optional[str] = None)
```

**Parameters:**

* `llm_provider`: LLM provider ("openai" or "deepseek")
* `model_name`: Model name (optional, uses default if not specified)

**Supported LLMs:**

* OpenAI (default: gpt-4)
* Deepseek (default: deepseek-chat)

### Methods

**chat**

```python
def chat(self, message: str) -> str
```

Send a message and get a complete response.

**Parameters:**

* `message`: The query or command to analyze

**Returns:**

* String containing the analysis response

**chat\_stream**

```python
def chat_stream(self, message: str)
```

Send a message and get a streaming response.

**Parameters:**

* `message`: The query or command to analyze

**Returns:**

* Generator yielding response chunks

**set\_model**

```python
def set_model(self, llm_provider: str, model_name: Optional[str] = None) -> None
```

Change the LLM provider and model.

**Parameters:**

* `llm_provider`: New LLM provider
* `model_name`: New model name (optional)

**available\_tools**

```python
@property
def available_tools(self) -> List[str]
```

Get list of available analysis tools.

**Returns:**

* List of tool names

## Market Analysis Tools

### analyze\_contract

```python
def analyze_contract(contract_address: str) -> Dict[str, Any]
```

Analyze a Solana token contract.

**Parameters:**

* `contract_address`: Solana contract address

**Returns:**

* Analysis report dictionary

### analyze\_ticker

```python
def analyze_ticker(ticker: str) -> Dict[str, Any]
```

Analyze a token by its ticker symbol.

**Parameters:**

* `ticker`: Token ticker (e.g., "BTC", "$SOL")

**Returns:**

* Analysis report dictionary

### analyze\_market\_trends

```python
def analyze_market_trends(top_n_tokens: int = 10) -> Dict[str, Any]
```

Analyze trending tokens.

**Parameters:**

* `top_n_tokens`: Number of top tokens to analyze

**Returns:**

* Market trend analysis dictionary

### analyze\_meta\_market

```python
def analyze_meta_market(top_n_tokens: int = 10, meta: str = "ai") -> Dict[str, Any]
```

Analyze tokens in a specific category.

**Parameters:**

* `top_n_tokens`: Number of tokens to analyze
* `meta`: Category ("ai", "gaming", "meme")

**Returns:**

* Meta market analysis dictionary

## Data Clients

### PumpFunClient

Client for real-time token data from Pump.fun.

```python
class PumpFunClient:
    def __init__(self, uri: str = "wss://pumpportal.fun/api/data", max_coin: int = 5)
```

**Methods**

**get\_trending\_tokens**

```python
def get_trending_tokens(limit: int = 5) -> List[Dict]
```

Get trending tokens via WebSocket connection.

**Parameters:**

* `limit`: Number of tokens to retrieve

**Returns:**

* List of token data dictionaries

**get\_meta\_tokens**

```python
def get_meta_tokens(meta: str, limit: int = 10) -> List[Dict]
```

Get tokens by category.

**Parameters:**

* `meta`: Category name
* `limit`: Number of tokens to retrieve

**Returns:**

* List of token data dictionaries

### DexClient

Client for DexScreener API interactions.

```python
class DexClient:
    def get_token_pairs(chain_id: str, token_address: str) -> List[Any]
    def search_pairs(pair_name: str) -> List[Any]
```

**Methods**

**get\_token\_pairs**

Get token pairs for a specific token.

**Parameters:**

* `chain_id`: Chain identifier
* `token_address`: Token contract address

**Returns:**

* List of token pair data

**search\_pairs**

Search for token pairs by name.

**Parameters:**

* `pair_name`: Name of the pair to search for

**Returns:**

* List of matching pairs
