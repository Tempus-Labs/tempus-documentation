---
icon: database
---

# Data

### Overview

The `Data` component provides interfaces to various cryptocurrency data sources. It handles real-time market data, historical data, and meta-information about tokens and trading pairs. The component is designed to be extensible and supports multiple data providers.

### Data Resources

* **Dexscreener API:** Provides token metrics (price, volume, liquidity, etc.).
* **Solana RPC API:** Retrieves on-chain transaction data.
* **Pump.fun API & Web Scraper**: Fetches trending tokens via WebSocket and scrapes metadata from the Pump.fun website.

### Data Models

* `TokenData`: Stores token metadata (name, price, market cap, liquidity).
* `TransactionData`: Records transaction counts and volume.
* `PriceChangeData`: Tracks price fluctuations over defined time intervals.
* `TrendingTokenData`: Stores trending token information retrieved from Pump.fun.
* `MetaTokenData`: Stores categorized tokens based on metadata from Pump.fun.

### Data Flow

1. API queries retrieve real-time data from Dexscreener and Solana RPC.
2. WebSocket connection streams trending tokens from Pump.fun.
3. Web scraping extracts categorized token metadata from Pump.fun.
4. Data is parsed and stored in structured models.
5. Models feed into the Agent’s analysis process.

### Components

* **Data Source:** Defines sources such as Dexscreener, Solana RPC, and Pump.fun.&#x20;
* **Data Type:** Indicates the type (e.g., Transactions, Liquidity, Prices, Trending Tokens, Meta Tokens).
* **Data Schema:** Outlines fields and structures for processing.

### Methods

* `fetch_data()` – Collects data from integrated APIs.
  * `DexClient.get_token_pairs(chain_id, token_address)`: Retrieves token pairs from Dexscreener.
  * `DexClient.search_pairs(pair_name)`: Searches for token pairs by name.
  * `PumpFunClient.get_trending_tokens(limit)`: Fetches trending tokens via WebSocket.
  * `PumpFunClient.get_meta_tokens(meta, limit)`: Scrapes categorized tokens based on metadata.
* `preprocess()` – Cleans and structures raw data for analysis.
* `store()` – Saves processed data for reuse.

### Basic Concepts

#### Data Clients

1. **PumpFunClient**
   * Real-time token data
   * WebSocket streaming
   * Meta-market information
2. **DexClient**
   * Trading pair data
   * Token information
   * Market statistics

#### Data Types

1. **Token Data**
   * Price information
   * Volume metrics
   * Market indicators
2. **Trading Pairs**
   * Pair information
   * Liquidity data
   * Trading metrics

### Architecture

```
data/
├── pump_fun_client.py    # Pump.fun data client
├── dex_client.py         # DexScreener client
└── __init__.py          # Package initialization
```

### Basic Usage

#### PumpFunClient

```python
from tempus.data.pump_fun_client import PumpFunClient

# Initialize client
client = PumpFunClient(
    uri="wss://pumpportal.fun/api/data",
    max_coin=5
)

# Get trending tokens
trending = client.get_trending_tokens(limit=5)

# Get tokens by category
ai_tokens = client.get_meta_tokens(
    meta="ai",
    limit=10
)
```

#### DexClient

```python
from tempus.data.dex_client import DexClient

# Initialize client
client = DexClient()

# Get token pairs
pairs = client.get_token_pairs(
    chain_id="solana",
    token_address="token_address_here"
)

# Search pairs
results = client.search_pairs("ETH/USDT")
```

### Advanced Features

#### WebSocket Streaming

```python
async def stream_data():
    client = PumpFunClient()
    
    async for data in client.stream_trending():
        process_data(data)
```

#### Batch Data Retrieval

```python
def batch_get_pairs(addresses: List[str]):
    client = DexClient()
    results = []
    
    for address in addresses:
        pairs = client.get_token_pairs("solana", address)
        results.extend(pairs)
        
    return results
```

### Data Structures

#### Token Data

```python
TokenData = {
    "symbol": str,          # Token symbol
    "address": str,         # Contract address
    "price": float,         # Current price
    "volume_24h": float,    # 24h volume
    "price_change": float,  # Price change %
    "market_cap": float,    # Market cap
    "created_at": str       # Creation timestamp
}
```

#### Pair Data

```python
PairData = {
    "pair_address": str,    # Pair contract
    "token0": TokenData,    # Base token
    "token1": TokenData,    # Quote token
    "liquidity": float,     # Total liquidity
    "volume_24h": float,    # 24h volume
    "price": float         # Current price
}
```

### Error Handling

#### Network Errors

```python
from requests.exceptions import RequestException

def safe_get_pairs(address: str):
    client = DexClient()
    try:
        return client.get_token_pairs("solana", address)
    except RequestException as e:
        logger.error(f"Network error: {e}")
        return []
    except Exception as e:
        logger.error(f"Unknown error: {e}")
        return []
```

#### WebSocket Errors

```python
import websocket

def handle_websocket_error():
    try:
        client = PumpFunClient()
        data = client.get_trending_tokens()
        return data
    except websocket.WebSocketException as e:
        logger.error(f"WebSocket error: {e}")
        return []
```

### Best Practices

1.  **Rate Limiting**

    ```python
    from time import sleep

    def rate_limited_request(func):
        def wrapper(*args, **kwargs):
            try:
                return func(*args, **kwargs)
            except Exception as e:
                if "rate limit" in str(e).lower():
                    sleep(1)
                    return func(*args, **kwargs)
                raise
        return wrapper
    ```
2.  **Data Validation**

    ```python
    def validate_token_data(data: Dict) -> bool:
        required = {"symbol", "address", "price"}
        return all(key in data for key in required)
    ```
3.  **Error Recovery**

    ```python
    def get_with_retry(func, max_retries=3):
        for attempt in range(max_retries):
            try:
                return func()
            except Exception:
                if attempt == max_retries - 1:
                    raise
                sleep(2 ** attempt)
    ```

### Performance Optimization

#### Connection Pooling

```python
import requests
from requests.adapters import HTTPAdapter
from requests.packages.urllib3.util.retry import Retry

class OptimizedDexClient(DexClient):
    def __init__(self):
        self.session = requests.Session()
        retries = Retry(total=5, backoff_factor=0.1)
        self.session.mount('https://', HTTPAdapter(max_retries=retries))
```

#### Caching

```python
from functools import lru_cache
from typing import Dict

class CachedDexClient(DexClient):
    @lru_cache(maxsize=100)
    def get_token_pairs(self, chain_id: str, address: str) -> Dict:
        return super().get_token_pairs(chain_id, address)
```

### Extension Points

#### Custom Data Source

```python
from abc import ABC, abstractmethod

class DataSource(ABC):
    @abstractmethod
    def get_token_data(self, symbol: str) -> Dict:
        pass
        
class CustomDataSource(DataSource):
    def get_token_data(self, symbol: str) -> Dict:
        # Implementation
        pass
```

#### Data Transformers

```python
class DataTransformer:
    def transform_token_data(self, data: Dict) -> Dict:
        return {
            "symbol": data["symbol"].upper(),
            "price": float(data["price"]),
            "volume": float(data["volume24h"])
        }
```

### Security Considerations

1.  **Input Sanitization**

    ```python
    def sanitize_address(address: str) -> str:
        return "".join(c for c in address if c.isalnum())
    ```
2.  **API Key Protection**

    ```python
    import os

    class SecureDexClient(DexClient):
        def __init__(self):
            self.api_key = os.getenv("DEX_API_KEY")
            if not self.api_key:
                raise ValueError("API key not found")
    ```
3.  **Request Validation**

    ```python
    def validate_request(chain_id: str, address: str) -> bool:
        if not chain_id or not address:
            return False
        if len(address) != 44:  # Solana address length
            return False
        return True
    ```

### Monitoring and Logging

```python
import logging

logger = logging.getLogger(__name__)

class MonitoredClient:
    def __init__(self):
        self.logger = logger
        
    def log_request(self, method: str, **kwargs):
        self.logger.info(f"API Request: {method}", extra=kwargs)
        
    def log_response(self, method: str, response: Dict):
        self.logger.info(f"API Response: {method}", extra={"data": response})
```
