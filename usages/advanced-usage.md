---
icon: scale-balanced
---

# Advanced Usage

## Custom Model Configuration

### Using Different Models

```python
# Initialize with specific OpenAI model
agent = QuantAIAgent(
    llm_provider="openai",
    model_name="gpt-4"
)

# Switch to Deepseek
agent = QuantAIAgent(
    llm_provider="deepseek",
    model_name="deepseek-chat"
)
```

### Switching Models During Runtime

```python
agent = QuantAIAgent()

# Switch to different model
agent.set_model("deepseek", "deepseek-reasoning")
```

## Streaming with Progress Updates

### Basic Streaming

```python
def process_stream():
    agent = QuantAIAgent()
    for chunk in agent.chat_stream("Analyze pump.fun market"):
        chunk.pretty_print()
```

### Custom Stream Processing

```python
def process_stream_with_progress():
    agent = QuantAIAgent()
    chunks = []
    
    for chunk in agent.chat_stream("Deep dive into pump.fun market"):
        chunks.append(chunk)
        # Show progress
        print(f"Received {len(chunks)} chunks", end="\r")
        
    print("\nAnalysis complete!")
    return "".join(chunks)
```

## Error Handling

### Basic Error Handling

```python
try:
    response = agent.chat("Analyze invalid_contract_address")
except Exception as e:
    print(f"Analysis failed: {e}")
```

### Comprehensive Error Handling

```python
from typing import Optional

def safe_analysis(contract: str) -> Optional[dict]:
    agent = QuantAIAgent()
    
    try:
        response = agent.chat(f"Analyze contract {contract}")
        return {"success": True, "data": response}
    except ValueError as e:
        return {"success": False, "error": "Invalid input", "details": str(e)}
    except ConnectionError as e:
        return {"success": False, "error": "Connection failed", "details": str(e)}
    except Exception as e:
        return {"success": False, "error": "Unknown error", "details": str(e)}
```

## Working with Data Sources

### PumpFun Integration

```python
from tempus.data.pump_fun_client import PumpFunClient

client = PumpFunClient()

# Get trending tokens
trending = client.get_trending_tokens(limit=10)

# Get AI tokens
ai_tokens = client.get_meta_tokens(meta="ai", limit=5)
```

### DexScreener Integration

```python
from tempus.data.dex_client import DexClient

client = DexClient()

# Search for specific pair
pairs = client.search_pairs("ai16z")

# Get token pairs for contract
token_pairs = client.get_token_pairs(
    chain_id="solana",
    token_address="your_token_address"
)
```

## Performance Optimization

### Memory Management

```python
def optimize_memory():
    agent = QuantAIAgent()
    
    # Perform multiple analyses
    for _ in range(10):
        response = agent.chat("Some analysis")
        
        # Clear conversation history periodically
        if _ % 3 == 0:
            agent.clear_history()
```

### Batch Processing

```python
async def batch_process_tokens(tokens: List[str]):
    agent = QuantAIAgent()
    results = []
    
    for token in tokens:
        try:
            response = agent.chat(f"Quick analysis of {token}")
            results.append({"token": token, "analysis": response})
        except Exception as e:
            results.append({"token": token, "error": str(e)})
            
    return results
```

## Custom Extensions

### Adding New Analysis Tools

```python
from typing import Dict, Any

def custom_analysis_tool(agent: QuantAIAgent) -> Dict[str, Any]:
    """
    Example of extending Tempus with custom analysis
    """
    # Get base analysis
    base_response = agent.chat("Analyze market")
    
    # Add custom metrics
    custom_metrics = {
        "timestamp": datetime.now().isoformat(),
        "custom_score": calculate_custom_score(),
        "base_analysis": base_response
    }
    
    return custom_metrics
```

## Rate Limiting and Backoff

### Implementing Rate Limits

```python
from time import sleep
from functools import wraps

def rate_limit(calls: int, period: int):
    def decorator(func):
        last_reset = time.time()
        calls_made = 0
        
        @wraps(func)
        def wrapper(*args, **kwargs):
            nonlocal last_reset, calls_made
            
            now = time.time()
            if now - last_reset >= period:
                calls_made = 0
                last_reset = now
                
            if calls_made >= calls:
                sleep_time = period - (now - last_reset)
                if sleep_time > 0:
                    sleep(sleep_time)
                calls_made = 0
                last_reset = time.time()
                
            calls_made += 1
            return func(*args, **kwargs)
        return wrapper
    return decorator

# Usage
@rate_limit(calls=5, period=60)
def rate_limited_analysis(agent: QuantAIAgent, query: str):
    return agent.chat(query)
```
