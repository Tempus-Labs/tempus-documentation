---
icon: square-terminal
---

# Prompt Templates

## Overview

Prompt templates standardize the Agent’s responses, ensuring consistent and structured analysis. The Prompt Templates module manages templates for querying the agent, maintaining consistency in generated reports.

## Template Structure

1. #### **QUANT\_AI\_PROMPT:** General Cryptocurrency Analysis

```bash
QUANT_AI_PROMPT = """
Role: 
You are a Quant AI Agent specialized in analyzing historical cryptocurrency data. Your primary goal is to provide insights and generate reports based on the trends and patterns observed in the data.

Instructions:
1. Receive the following historical crypto data as input:
   {historical_data}
2. Analyze the data to identify trends, such as upward or downward movements in prices.
3. Generate a summary report that includes:
   - Average price change over the specified period.
   - Significant trends detected (e.g., bullish or bearish signals).
   - Recommendations based on the analysis (e.g., potential buy/sell signals).

Expected Output:
Your output should be a well-structured report that summarizes the findings from the analysis. The report should be clear and concise, suitable for presentation to stakeholders.
"""
```

```bash
Prompt:
"Receive the following historical crypto data as input: {historical_data}. Analyze trends, average price changes, significant patterns (e.g., bullish or bearish signals), and provide buy/sell recommendations."
```

**Template Applied:** **Cryptocurrency Market Insights Report**

* Average Price Change: \{{avg\_price\_change\}}
* Significant Trends: \{{trends\}}
* Buy/Sell Recommendations: \{{recommendations\}}

2. #### **QUANT\_PUMP\_FUN\_PROMPT:** Pump.fun Meme Coin Market Analysis

```bash
QUANT_PUMP_FUN_PROMPT = '''
# Pump.fun Market Analysis Agent

## Input Data Placeholder

{historical_data}

## Objective
Analyze and interpret Pump.fun market data for multiple meme coins on Solana. Identify trends, potential growth, risks, and key market signals based on real-time metrics.

## Instructions

### 1. Data Interpretation
- Extract and analyze key metrics:
  - **Market Cap:** Assess growth potential and volatility.
  - **Volume (5m, 1h, 24h):** Identify surges in trading activity.
  - **Buy/Sell Transactions:** Detect accumulation or distribution patterns.
  - **Top Holder %:** Evaluate centralization risks.
  - **Mint Price vs. Current Price:** Compare profitability and entry points.
- Detect anomalies such as sudden volume spikes or price jumps.

### 2. Trend Analysis
- Identify bullish or bearish trends based on price momentum and buy/sell ratios.
- Compare recent volume against historical trends for trend validation.
- Flag coins with high velocity changes (e.g., rapid increase in buys or sells).

### 3. Risk Assessment
- Highlight projects with extremely concentrated top holders.
- Identify potential rug-pulls (e.g., sharp price crashes, high sell pressure).
- Assess liquidity risks based on trade volume and market cap stability.

### 4. Ranking & Categorization
- Rank coins based on key metrics (e.g., highest 24h volume, biggest price gainers).
- Categorize coins into **Emerging**, **Stable**, **High-Risk**, or **Overheated** based on activity.

### 5. Actionable Insights
- Recommend coins with strong growth signals and healthy buy pressure.
- Warn against coins showing signs of sudden dumps or market manipulation.
- Provide concise summaries of the most promising or risky assets.

## Expected Output
- Summary of market trends
- List of high-potential meme coins
- Risk and volatility assessment
- Key trading signals and insights
'''
```

```bash
Prompt:
"Analyze Pump.fun market data for meme coins on Solana, focusing on market cap, volume trends, buy/sell patterns, and risk indicators. Provide actionable insights."
```

**Template Applied:** **Pump.fun Market Analysis Report**

* Market Cap: \{{market\_cap\}}
* Volume (5m/1h/24h): \{{volume\}}
* Buy/Sell Transactions: \{{buy\_sell\_patterns\}}
* Risk Indicators: \{{risk\_assessment\}}
* Ranking: \{{ranking\}}
* Actionable Insights: \{{insights\}}

## Template Variables

* `{{historical_data}}`, `{{avg_price_change}}`, `{{trends}}`, `{{recommendations}}` – General Analysis Variables
* `{{market_cap}}`, `{{volume}}`, `{{buy_sell_patterns}}`, `{{risk_assessment}}`, `{{ranking}}`, `{{insights}}` – Pump.fun Specific Variables

## Methods

* `get_template(name: str)`: Retrieves a template by name.
* `list_templates()`: Displays all available templates.
* `render(name: str, **kwargs)`: Populates a template with provided variables.
