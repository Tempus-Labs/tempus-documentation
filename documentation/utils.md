---
icon: screwdriver-wrench
---

# Utility

## Overview

The `Utility` module provides essential helper functions for market analysis and data retrieval within the Tempus framework.

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
