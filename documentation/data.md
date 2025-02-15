# Data

## Overview

The `Data` module manages data collection and preprocessing from integrated APIs, enabling the agent to perform quantitative analysis.

## Data Resources

* **Dexscreener API:** Provides token metrics (price, volume, liquidity, etc.).
* **Solana RPC API:** Retrieves on-chain transaction data.

## Data Models

* `TokenData`: Stores token metadata (name, price, market cap, liquidity).
* `TransactionData`: Records transaction counts and volume.
* `PriceChangeData`: Tracks price fluctuations over defined time intervals.

## Data Flow

1. API queries retrieve real-time data.
2. Data is parsed and stored in structured models.
3. Models feed into the Agent’s analysis process.

## Component

**Data Source:** Defines sources such as Dexscreener and Solana RPC. **Data Type:** Indicates the type (e.g., Transactions, Liquidity, Prices). **Data Schema:** Outlines fields and structures for processing.

## Methods

* `fetch_data()` – Collects data from integrated APIs.
* `preprocess()` – Cleans and structures raw data for analysis.
* `store()` – Saves processed data for reuse.
