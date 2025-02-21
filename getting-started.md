---
icon: chart-simple-horizontal
---

# Getting Started

## Prerequisites

* [Python 3.10](https://www.python.org/)
* LLM API Key (OpenAI, Deepseek)
* [Chromedriver](https://developer.chrome.com/docs/chromedriver/downloads)

## Installation

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

# Initialize agent with Deepseek
agent = QuantAIAgent(llm_provider="deepseek", model_name="deepseek-chat")

# Analyze pump.fun market
response = agent.chat("Analyze the top 10 trending ai meta on Pump.fun")
print(response)

# Stream chat for analyze $ai16z coin
for chunk in agent.chat_stream("What about $ai16z coin?"):
    print(chunk, end="")
```
{% endstep %}
{% endstepper %}

You're now ready to explore the power of Tempus Labs for AI-driven quantitative insights on the Solana ecosystem!
