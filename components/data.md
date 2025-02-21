---
icon: database
---

# Data

## Overview

The `Data` module manages data collection and preprocessing from integrated APIs, enabling the agent to perform quantitative analysis.

## Data Resources

* **Dexscreener API:** Provides token metrics (price, volume, liquidity, etc.).
* **Solana RPC API:** Retrieves on-chain transaction data.
* **Pump.fun API & Web Scraper**: Fetches trending tokens via WebSocket and scrapes metadata from the Pump.fun website.

## Data Models

* `TokenData`: Stores token metadata (name, price, market cap, liquidity).
* `TransactionData`: Records transaction counts and volume.
* `PriceChangeData`: Tracks price fluctuations over defined time intervals.
* `TrendingTokenData`: Stores trending token information retrieved from Pump.fun.
* `MetaTokenData`: Stores categorized tokens based on metadata from Pump.fun.

## Data Flow

1. API queries retrieve real-time data from Dexscreener and Solana RPC.
2. WebSocket connection streams trending tokens from Pump.fun.
3. Web scraping extracts categorized token metadata from Pump.fun.
4. Data is parsed and stored in structured models.
5. Models feed into the Agent’s analysis process.

## Components

* **Data Source:** Defines sources such as Dexscreener, Solana RPC, and Pump.fun.&#x20;
* **Data Type:** Indicates the type (e.g., Transactions, Liquidity, Prices, Trending Tokens, Meta Tokens).
* **Data Schema:** Outlines fields and structures for processing.

## Methods

* `fetch_data()` – Collects data from integrated APIs.
  * `DexClient.get_token_pairs(chain_id, token_address)`: Retrieves token pairs from Dexscreener.
  * `DexClient.search_pairs(pair_name)`: Searches for token pairs by name.
  * `PumpFunClient.get_trending_tokens(limit)`: Fetches trending tokens via WebSocket.
  * `PumpFunClient.get_meta_tokens(meta, limit)`: Scrapes categorized tokens based on metadata.
* `preprocess()` – Cleans and structures raw data for analysis.
* `store()` – Saves processed data for reuse.
