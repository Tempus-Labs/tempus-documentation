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
