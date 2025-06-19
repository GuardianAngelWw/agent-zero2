## Knowledge Tool

### Overview
The knowledge_tool provides access to both online information and stored memories to answer specific questions. It's designed for direct factual responses rather than providing guidance.

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `question` | string | The specific question you need answered. Should be clear and focused. |

### Usage Guidelines

- **Question Formulation**:
  - Ask direct, specific questions rather than open-ended ones
  - Request results or facts first, not guidance
  - Phrase questions in a way that encourages factual answers

- **Information Sources**:
  - The tool draws from both online sources (current information) and stored memories
  - Online sources provide up-to-date information
  - Memory provides stored knowledge and previous interactions
  - Always verify memory-based information with online sources when currency is important

- **Best Practices**:
  - Use for factual queries rather than subjective questions
  - Verify critical information across multiple sources
  - When information seems outdated, specifically request current data

### Example Usage

```json
{
  "thoughts": [
    "I need to find the current exchange rate between USD and EUR"
  ],
  "tool_name": "knowledge_tool",
  "tool_args": {
    "question": "What is the current exchange rate between USD and EUR?"
  }
}
```