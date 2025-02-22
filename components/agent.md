---
icon: brain-circuit
---

# Agent

## Overview

The `Agents` component is the core of the Tempus framework, providing AI-powered market analysis capabilities. It uses LangChain and various LLM providers to create intelligent agents that can analyze cryptocurrency markets, understand market trends, and provide insights.

## Capabilities

* Processes user prompts and queries related to cryptocurrency analysis.
* Analyzes live token data from the Solana blockchain.
* Leverages AI-driven tools for contract analysis, market trend evaluation, and ticker monitoring.
* Supports multiple AI models, including OpenAI's GPT-4 and DeepSeek models.
* Generates structured reports with trading recommendations and market trend analysis.

## Agent Workflow

1. **Receives Prompt Input** – The agent processes user messages and commands.
2. **Queries Data from APIs** – The system retrieves data from external sources like Dexscreener and Solana RPC.
3. **Applies AI-Based Analysis** – The agent uses predefined prompt templates and market analysis tools to structure responses.
4. **Outputs a Detailed Market Report** – Generates insights, including price trends, volatility analysis, and contract evaluations.
5. **Memory Management** – Maintains conversation history and context for continuous engagement.

## Basic Concepts

### QuantAIAgent

The main agent class that provides:

* Market analysis capabilities
* LLM integration
* Conversation management
* Tool orchestration

### Chatbot

A helper class that manages:

* LLM interactions
* Response processing
* Error handling
* Stream management

### State Management

The agent maintains:

* Conversation history
* Tool states
* LLM configuration
* Memory checkpoints

## Architecture

```
agents/
├── quant_agent.py      # Main agent implementation
└── __init__.py         # Package initialization
```

## Basic Usage

```python
from tempus.agents.quant_agent import QuantAIAgent

# Initialize agent
agent = QuantAIAgent(
    llm_provider="openai",  # or "deepseek"
    model_name="gpt-4"      # optional
)

# Basic interaction
response = agent.chat("Analyze pump.fun market")

# Streaming interaction
for chunk in agent.chat_stream("Analyze solana trends"):
    print(chunk, end="")

# Switch models
agent.set_model("deepseek", "deepseek-chat")
```

## Advanced Features

### Conversation Management

The agent maintains conversation history for context:

```python
# Get conversation history
history = agent.history

# Clear history when needed
agent.clear_history()
```

### Tool Integration

The agent automatically manages market analysis tools:

```python
# List available tools
tools = agent.available_tools
print(f"Available tools: {tools}")
```

### Error Handling

The agent includes robust error handling:

```python
try:
    response = agent.chat("Analyze invalid_input")
except ValueError as e:
    print(f"Invalid input: {e}")
except Exception as e:
    print(f"Analysis failed: {e}")
```

## Configuration

### LLM Providers

Currently supported LLM providers:

* OpenAI (default: gpt-4)
* Deepseek (default: deepseek-chat)

```python
# OpenAI configuration
agent = QuantAIAgent(
    llm_provider="openai",
    model_name="gpt-4"
)

# Deepseek configuration
agent = QuantAIAgent(
    llm_provider="deepseek",
    model_name="deepseek-chat"
)
```

### Memory Management

The agent uses LangChain's memory system:

```python
from langgraph.checkpoint.memory import MemorySaver

# Custom memory configuration
memory = MemorySaver()
# Configure memory as needed
```

## Internal Components

### StateGraph

The agent uses LangChain's StateGraph for workflow:

```python
from langgraph.graph import StateGraph, MessagesState

# Graph structure
workflow = StateGraph(MessagesState)
workflow.add_node("agent", Chatbot(runnable))
workflow.add_node("tools", tool_node)
```

### Message Handling

The agent processes different message types:

* HumanMessage: User inputs
* AIMessage: Agent responses
* ToolMessage: Tool outputs
* SystemMessage: System prompts

## Best Practices

1. **Model Selection**
   * Use OpenAI for accuracy
   * Use Deepseek for cost-efficiency
2. **Memory Management**
   * Clear history periodically
   * Monitor memory usage
3. **Error Handling**
   * Implement proper try-except blocks
   * Log errors appropriately
4. **Performance**
   * Use streaming for long responses
   * Batch similar queries

## Common Issues

1.  **Rate Limiting**

    ```python
    # Implement rate limiting
    from time import sleep

    def rate_limited_chat(agent, query):
        try:
            return agent.chat(query)
        except Exception:
            sleep(1)  # Wait before retry
            return agent.chat(query)
    ```
2.  **Context Management**

    ```python
    # Manage context length
    if len(agent.history) > 10:
        agent.clear_history()
    ```
3.  **Model Switching**

    ```python
    # Fallback strategy
    try:
        response = agent.chat(query)
    except Exception:
        agent.set_model("deepseek")  # Switch to backup model
        response = agent.chat(query)
    ```

## Extension Points

### Custom Tools

Add custom tools to the agent:

```python
from typing import Dict, Any

def custom_tool() -> Dict[str, Any]:
    """Custom analysis tool"""
    return {"result": "analysis"}

# Add to agent's tools
agent.tools.append(custom_tool)
```

### Custom Prompts

Modify system prompts:

```python
from langchain_core.prompts import ChatPromptTemplate

custom_prompt = ChatPromptTemplate.from_messages([
    ("system", "Custom system message"),
    ("user", "{input}")
])
```

## Performance Optimization

1.  **Streaming Optimization**

    ```python
    def optimized_stream(query: str):
        buffer = []
        for chunk in agent.chat_stream(query):
            buffer.append(chunk)
            if len(buffer) > 100:
                process_chunks(buffer)
                buffer = []
    ```
2.  **Batch Processing**

    ```python
    def batch_process(queries: List[str]):
        results = []
        for query in queries:
            results.append(agent.chat(query))
            if len(results) % 5 == 0:
                agent.clear_history()
        return results
    ```

## Security Considerations

1. **API Key Management**
   * Use environment variables
   * Rotate keys regularly
2.  **Input Validation**

    ```python
    def validate_input(query: str) -> bool:
        # Remove harmful characters
        clean_query = sanitize(query)
        return len(clean_query) > 0
    ```
3.  **Rate Limiting**

    ```python
    from functools import wraps

    def rate_limit(func):
        calls = []
        @wraps(func)
        def wrapper(*args, **kwargs):
            now = time.time()
            calls = [c for c in calls if now - c < 60]
            if len(calls) >= 60:
                raise Exception("Rate limit exceeded")
            calls.append(now)
            return func(*args, **kwargs)
        return wrapper
    ```
