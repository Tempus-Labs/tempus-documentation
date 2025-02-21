---
icon: brain-circuit
---

# Agent

## Overview

The `Agent` module in the Tempus AI framework is a core component designed to process prompts and generate comprehensive analytical reports on cryptocurrency markets. It integrates with various data sources, utilizes advanced AI models, and provides actionable insights for traders and analysts.

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

## Component

* **Agent Name:** Unique identifier for the agent instance.
* **Agent Type:** Defines the analytical scope (e.g., Quantitative Analysis).
* **Agent Description:** Overview of the agent's analytical capabilities.

## Methods

* `initialize()` – Prepares the agent with necessary configurations, including selecting the AI model and initializing tools.
* `run(prompt: str)` – Processes the input prompt, invokes the relevant analytical tools, and returns a structured market analysis report.
* `chat(message: str) -> str` – Handles user queries, engages in conversation, and returns insights based on market conditions.
* `chat_stream(message: str)` – Provides a real-time streaming response for interactive analysis.
* `set_model(llm_provider: str, model_name: Optional[str])` – Allows switching between different AI models for enhanced customization and accuracy.
* `terminate()` – Safely shuts down the agent and clears temporary data.
