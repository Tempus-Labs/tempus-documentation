---
icon: square-terminal
---

# Prompt Templates

### Overview

The `Prompt Templates` component manages the system prompts and instructions used by the QuantAIAgent. These templates define how the AI agent interacts with users, analyzes market data, and formats responses. The component ensures consistent and effective communication patterns across the framework.

### Basic Concepts

#### Template Types

1. **System Prompts**
   * Define agent's role and capabilities
   * Set behavior guidelines
   * Establish response formats
2. **Analysis Templates**
   * Market analysis structures
   * Token evaluation patterns
   * Trend analysis frameworks

### Architecture

```
prompt_templates/
├── quant_ai.py         # QuantAI agent prompts
└── __init__.py         # Package initialization
```

### Basic Usage

#### Using Templates

```python
from tempus.prompt_templates.quant_ai import (
    QUANT_AI_CHATBOT_PROMPT,
    MARKET_ANALYSIS_TEMPLATE,
    TOKEN_ANALYSIS_TEMPLATE
)

# Create prompt template
from langchain_core.prompts import ChatPromptTemplate

prompt = ChatPromptTemplate.from_messages([
    ("system", QUANT_AI_CHATBOT_PROMPT),
    ("user", "{input}")
])
```

#### Customizing Templates

```python
# Extend base template
custom_prompt = QUANT_AI_CHATBOT_PROMPT + """
Additional Instructions:
1. Focus on DeFi metrics
2. Include liquidity analysis
3. Evaluate token utility
"""

# Create template with custom instructions
template = ChatPromptTemplate.from_messages([
    ("system", custom_prompt),
    ("user", "{input}")
])
```

### Template Structure

#### System Prompt

```python
QUANT_AI_CHATBOT_PROMPT = """
You are a specialized cryptocurrency market analyst with expertise in:
1. Technical Analysis
2. Market Trends
3. Token Evaluation
4. Risk Assessment

For each analysis:
- Provide data-driven insights
- Include relevant metrics
- Highlight key risks
- Suggest action points
"""
```

#### Analysis Templates

```python
MARKET_ANALYSIS_TEMPLATE = """
Analyze the following market aspects:
1. Price Action
   - Current price: {price}
   - 24h change: {change_24h}
   - Volume: {volume}

2. Market Context
   - Market cap: {market_cap}
   - Rank: {rank}
   - Category: {category}

3. Key Metrics
   - Liquidity: {liquidity}
   - Holders: {holders}
   - Distribution: {distribution}

Provide analysis focusing on:
- Current market position
- Notable trends
- Risk factors
- Opportunities
"""

TOKEN_ANALYSIS_TEMPLATE = """
Evaluate this token:
Symbol: {symbol}
Contract: {contract}
Network: {network}

Consider:
1. Token Fundamentals
2. Market Performance
3. Technical Indicators
4. Risk Assessment
"""
```

### Advanced Features

#### Dynamic Templates

```python
def create_dynamic_template(metrics: List[str]) -> str:
    """Create template based on available metrics"""
    sections = ["Basic Analysis:"]
    
    if "price" in metrics:
        sections.append("Price Analysis:\n- Current: {price}\n- Change: {change}")
    
    if "volume" in metrics:
        sections.append("Volume Analysis:\n- 24h: {volume}\n- 7d: {volume_7d}")
    
    return "\n\n".join(sections)
```

#### Template Composition

```python
def compose_templates(templates: List[str]) -> str:
    """Combine multiple templates"""
    return "\n\n".join([
        "Combined Analysis:",
        *templates
    ])
```

### Best Practices

1.  **Template Organization**

    ```python
    # Group related templates
    PRICE_TEMPLATES = {
        "basic": "Basic price analysis: {price}",
        "detailed": "Detailed price analysis: {price} with {indicators}"
    }

    VOLUME_TEMPLATES = {
        "basic": "Volume overview: {volume}",
        "detailed": "Volume analysis: {volume} across {timeframes}"
    }
    ```
2.  **Template Validation**

    ```python
    def validate_template(template: str, required_vars: Set[str]) -> bool:
        """Validate template has required variables"""
        template_vars = {
            var.strip("{}") for var in 
            re.findall(r"\{([^}]+)\}", template)
        }
        return required_vars.issubset(template_vars)
    ```
3.  **Version Control**

    ```python
    class TemplateVersion:
        def __init__(self, version: str, template: str):
            self.version = version
            self.template = template
            self.created_at = datetime.now()

    TEMPLATE_VERSIONS = {
        "v1.0": TemplateVersion("1.0", QUANT_AI_CHATBOT_PROMPT),
        "v1.1": TemplateVersion("1.1", UPDATED_PROMPT)
    }
    ```

### Extension Points

#### Custom Templates

```python
class CustomTemplate:
    def __init__(self, base_template: str):
        self.base = base_template
        
    def add_section(self, section: str) -> None:
        self.base += f"\n\n{section}"
        
    def get_template(self) -> str:
        return self.base
```

#### Template Processors

```python
class TemplateProcessor:
    def __init__(self, template: str):
        self.template = template
        
    def add_defaults(self, defaults: Dict[str, str]) -> None:
        """Add default values for template variables"""
        self.defaults = defaults
        
    def process(self, **kwargs) -> str:
        """Process template with given variables"""
        variables = {**self.defaults, **kwargs}
        return self.template.format(**variables)
```

### Performance Considerations

1.  **Template Caching**

    ```python
    from functools import lru_cache

    @lru_cache(maxsize=100)
    def get_processed_template(template_name: str, **kwargs) -> str:
        template = load_template(template_name)
        return template.format(**kwargs)
    ```
2.  **Lazy Loading**

    ```python
    class LazyTemplateLoader:
        def __init__(self):
            self._templates = {}
            
        def get_template(self, name: str) -> str:
            if name not in self._templates:
                self._templates[name] = load_template(name)
            return self._templates[name]
    ```

### Security Considerations

1.  **Input Validation**

    ```python
    def safe_format(template: str, **kwargs) -> str:
        """Safely format template with validated input"""
        safe_kwargs = {
            k: str(v).replace("{", "{{").replace("}", "}}")
            for k, v in kwargs.items()
        }
        return template.format(**safe_kwargs)
    ```
2.  **Template Sanitization**

    ```python
    def sanitize_template(template: str) -> str:
        """Remove potentially harmful template constructs"""
        return template.replace("{", "{{").replace("}", "}}")
    ```

### Testing Templates

```python
def test_template(template: str, test_cases: List[Dict]) -> bool:
    """Test template with various inputs"""
    for case in test_cases:
        try:
            result = template.format(**case)
            if not validate_output(result):
                return False
        except Exception:
            return False
    return True
```

