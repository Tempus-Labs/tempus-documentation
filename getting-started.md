---
icon: chart-simple-horizontal
---

# Getting Started

### Prerequisites

* [Python 3.10](https://www.python.org/)
* LLM API Key (OpenAI, Deepseek)
* [Chromedriver](https://developer.chrome.com/docs/chromedriver/downloads)

### Installation

Follow these simple steps to install and set up Tempus Labs:

{% stepper %}
{% step %}
**Step 1: Install Tempus**

Begin by installing the Tempus framework using pip:

```bash
pip install tempus-labs==1.0.0
```
{% endstep %}

{% step %}
**Step 2: Create the Environment File**

Set up a `.env` file to store your API key securely:

```python
OPENAI_API_KEY=your_api_key # Fill if you are using OpenAI
DEEPSEEK_API_KEY=your_api_key # Fill if you are using Deepseek
```
{% endstep %}

{% step %}
**Step 3: Basic usage**

```python
from tempus.agents import QuantAIAgent

# Initialize with default settings (OpenAI)
agent = QuantAIAgent()

# Basic analysis
response = agent.chat("Analyze BTC market trends")
print(response)

# Streaming responses
for chunk in agent.chat_stream("What's happening with ETH?"):
    print(chunk, end="")
```
{% endstep %}
{% endstepper %}

### Basic Concepts

#### QuantAIAgent

The `QuantAIAgent` is your main interface to the Tempus framework. It provides:

* Market analysis capabilities
* Real-time data streaming
* Multiple LLM provider support
* Conversation memory

#### Market Analysis Tools

Tempus comes with several built-in analysis tools:

* Contract Analysis: Deep dive into token contracts
* Ticker Analysis: Research specific tokens
* Market Trends: Track trending tokens
* Meta Market Analysis: Explore token categories (AI, Gaming, Meme)

#### Data Sources

Tempus integrates with:

* Pump.fun for real-time token data
* DexScreener for comprehensive market data
* Multi-chain support (focused on Solana)

### Next Steps

* Check out the [API Reference](api-reference.md) for detailed documentation
* Learn about advanced features in [Advanced Usage](usage/advanced-usage.md)
* See [Examples](usage/examples.md) for more use cases

You're now ready to explore the power of Tempus Labs for AI-driven quantitative insights on the Solana ecosystem!
