---
icon: square-terminal
---

# Prompt Templates

## Overview

Prompt templates standardize the Agent’s responses, ensuring consistent and structured analysis. The `Prompt Templates` module manages templates for querying the agent, ensuring consistency in generated reports.

## Template Structure

```bash
Prompt: "I want to know about coin and the ticker is $TOKENNAME"

Template Applied:
Cryptocurrency Analysis Report:
Name: {{token_name}}
Price (USD): {{price}}
Volume (24h): {{volume_24h}}
Price Change (24h): {{price_change_24h}}
Recommendations:
{{recommendations}}
```

## Template Variables

* `{{token_name}}`: Fetched from `TokenData`
* `{{price}}`: Pulled from `TokenData`
* `{{volume_24h}}`: From `TransactionData`
* `{{price_change_24h}}`: From `PriceChangeData`

## Example Applied Template Output

_(As shown in your provided sample)_

## Components

**Template Name:** Unique identifier for the template. **Template Format:** Predefined structure for agent prompts. **Placeholders:** Variables for customization (e.g., `$TICKER`, `$PERIOD`).

## Methods

* `get_template(name: str)` – Retrieves a template by name.
* `list_templates()` – Displays all available templates.
* `render(name: str, **kwargs)` – Populates a template with variables.
