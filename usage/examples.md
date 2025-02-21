---
icon: code
---

# Examples

### Basic Usage

#### Initialize Agent

```python
from tempus.agents.quant_agent import QuantAIAgent

# Default initialization (OpenAI)
agent = QuantAIAgent()

# Or with Deepseek
agent_deepseek = QuantAIAgent(llm_provider="deepseek")
```

#### Simple Analysis

```python
# Basic BTC analysis
btc_analysis = agent.chat("Analyze BTC market trends")
print(btc_analysis)

# ETH analysis with streaming
for chunk in agent.chat_stream("What's happening with ETH?"):
    print(chunk, end="")
```

### Market Analysis

#### Token Analysis

```python
# Analyze specific token
def analyze_token(symbol: str):
    agent = QuantAIAgent()
    
    # Get basic analysis
    basic = agent.chat(f"Quick analysis of {symbol}")
    print(f"Basic {symbol} Analysis:")
    print(basic)
    
    # Get detailed analysis
    detailed = agent.chat(f"Deep dive into {symbol} including market trends")
    print(f"\nDetailed {symbol} Analysis:")
    print(detailed)
    
# Usage
analyze_token("SOL")
```

#### Trend Analysis

```python
def analyze_trends():
    agent = QuantAIAgent()
    
    # Get current trends
    trends = agent.chat("What are the top trending tokens right now?")
    print("Current Trends:")
    print(trends)
    
    # Get predictions
    predictions = agent.chat("Predict potential market movements for these trending tokens")
    print("\nPredictions:")
    print(predictions)

# Usage
analyze_trends()
```

#### Meta Market Analysis

```python
def analyze_category(category: str):
    agent = QuantAIAgent()
    
    prompt = f"""Analyze the {category} token category:
    1. Current market leaders
    2. Recent trends
    3. Market sentiment
    4. Future outlook"""
    
    analysis = agent.chat(prompt)
    print(f"{category.upper()} Market Analysis:")
    print(analysis)

# Usage
analyze_category("ai")
analyze_category("gaming")
analyze_category("meme")
```

### Data Integration

### Real-time Data

```python
from tempus.data.pump_fun_client import PumpFunClient

def monitor_trending():
    client = PumpFunClient()
    agent = QuantAIAgent()
    
    # Get trending tokens
    trending = client.get_trending_tokens(limit=5)
    
    # Analyze each token
    for token in trending:
        analysis = agent.chat(f"Quick analysis of {token['symbol']}")
        print(f"\nAnalysis for {token['symbol']}:")
        print(analysis)

# Usage
monitor_trending()
```

#### Market Data Integration

```python
from tempus.data.dex_client import DexClient

def analyze_pair(pair_name: str):
    dex_client = DexClient()
    agent = QuantAIAgent()
    
    # Get pair data
    pairs = dex_client.search_pairs(pair_name)
    
    if pairs:
        # Analyze first pair
        pair = pairs[0]
        analysis = agent.chat(f"""Analyze this trading pair:
        Symbol: {pair['symbol']}
        Price: {pair['price']}
        Volume: {pair['volume24h']}""")
        
        print(f"Analysis for {pair_name}:")
        print(analysis)
    else:
        print(f"No data found for {pair_name}")

# Usage
analyze_pair("ETH/USDT")
```

### Advanced Examples

#### Custom Analysis Pipeline

```python
from typing import Dict, List
import asyncio

class MarketAnalysisPipeline:
    def __init__(self):
        self.agent = QuantAIAgent()
        self.pump_client = PumpFunClient()
        self.dex_client = DexClient()
        
    async def analyze_token(self, token: Dict) -> Dict:
        """Analyze a single token"""
        symbol = token['symbol']
        
        # Get token pairs
        pairs = self.dex_client.search_pairs(symbol)
        
        # Get AI analysis
        analysis = self.agent.chat(f"""
        Analyze {symbol} with this data:
        Price: {token['price']}
        Volume: {token['volume']}
        Pairs: {len(pairs)} trading pairs
        """)
        
        return {
            'symbol': symbol,
            'data': token,
            'pairs': pairs,
            'analysis': analysis
        }
        
    async def run_analysis(self) -> List[Dict]:
        """Run complete market analysis"""
        # Get trending tokens
        trending = self.pump_client.get_trending_tokens(limit=10)
        
        # Analyze all tokens
        tasks = [self.analyze_token(token) for token in trending]
        results = await asyncio.gather(*tasks)
        
        return results

# Usage
async def main():
    pipeline = MarketAnalysisPipeline()
    results = await pipeline.run_analysis()
    
    for result in results:
        print(f"\nAnalysis for {result['symbol']}:")
        print(result['analysis'])

asyncio.run(main())
```

#### Sentiment Analysis

```python
def analyze_market_sentiment():
    agent = QuantAIAgent()
    
    # Get overall market sentiment
    sentiment = agent.chat("""
    Analyze current market sentiment considering:
    1. Top 10 tokens by volume
    2. Recent price movements
    3. Trading activity
    4. Social media trends
    """)
    
    print("Market Sentiment Analysis:")
    print(sentiment)
    
    # Get specific recommendations
    recommendations = agent.chat("""
    Based on the current sentiment:
    1. What sectors show promise?
    2. What are the potential risks?
    3. What strategies might be effective?
    """)
    
    print("\nRecommendations:")
    print(recommendations)

# Usage
analyze_market_sentiment()
```

#### Portfolio Analysis

```python
def analyze_portfolio(holdings: Dict[str, float]):
    agent = QuantAIAgent()
    
    # Convert holdings to text
    portfolio_text = "\n".join(
        f"- {symbol}: {amount}" 
        for symbol, amount in holdings.items()
    )
    
    # Get portfolio analysis
    analysis = agent.chat(f"""
    Analyze this portfolio:
    {portfolio_text}
    
    Consider:
    1. Risk assessment
    2. Diversification
    3. Market exposure
    4. Potential adjustments
    """)
    
    print("Portfolio Analysis:")
    print(analysis)

# Usage
holdings = {
    "BTC": 0.5,
    "ETH": 5.0,
    "SOL": 100.0
}
analyze_portfolio(holdings)
```

#### Market Alert System

```python
import time
from typing import Callable

class MarketAlertSystem:
    def __init__(self):
        self.agent = QuantAIAgent()
        self.pump_client = PumpFunClient()
        self.alerts: List[Callable] = []
        
    def add_alert(self, condition: Callable, callback: Callable):
        """Add new alert condition and callback"""
        self.alerts.append((condition, callback))
        
    def check_alerts(self, market_data: Dict):
        """Check all alerts against market data"""
        for condition, callback in self.alerts:
            if condition(market_data):
                callback(market_data)
                
    def run(self, interval: int = 60):
        """Run alert system"""
        while True:
            try:
                # Get market data
                trending = self.pump_client.get_trending_tokens()
                
                # Get AI analysis
                analysis = self.agent.chat(
                    f"Quick analysis of market movement in {len(trending)} tokens"
                )
                
                # Check alerts
                market_data = {
                    'trending': trending,
                    'analysis': analysis,
                    'timestamp': time.time()
                }
                self.check_alerts(market_data)
                
                # Wait for next check
                time.sleep(interval)
                
            except Exception as e:
                print(f"Error in alert system: {e}")
                time.sleep(interval)

# Usage
def price_alert(data: Dict) -> bool:
    """Example condition: check if any token moved >10%"""
    for token in data['trending']:
        if abs(token['price_change_24h']) > 10:
            return True
    return False

def alert_callback(data: Dict):
    """Example callback: print alert"""
    print("Price Alert:", data['analysis'])

# Setup and run alerts
alert_system = MarketAlertSystem()
alert_system.add_alert(price_alert, alert_callback)
alert_system.run()
```
