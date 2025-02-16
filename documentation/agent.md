---
icon: brain-circuit
---

# Agent

## Overview

The `Agent` module is the core of Tempus AI, designed to process prompts and generate analytical reports on cryptocurrency markets. It coordinates interactions between data sources, models, and utility functions to deliver actionable insights.

## Capabilities

* Processes user prompts and queries.
* Analyzes live token data from the Solana blockchain.
* Generates reports with market trends, price movements, and trading recommendations.

## Agent Workflow

1. Receives prompt input.
2. Queries data from APIs (Dexscreener, Solana RPC).
3. Applies prompt templates for structured analysis.
4. Outputs a detailed market analysis report.

## Component

**Agent Name:** Identifier for the agent instance. \
**Agent Type:** Specifies the model type (e.g., Quantitative Analysis). **Agent Description:** Brief description of the agent’s analytical capabilities.

## Methods

* `initialize()` – Prepares the agent with necessary configurations.
* `run(prompt: str)` – Processes the input prompt and returns an analysis report.
* `terminate()` – Shuts down the agent safely.
