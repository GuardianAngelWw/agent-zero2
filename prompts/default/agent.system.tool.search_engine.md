## Search Engine Tool

### Overview
The search_engine tool provides access to web search results, returning a list of relevant URLs, titles, and descriptions based on your query.

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `query` | string | The search query to find relevant web results |

### Usage Guidelines

- **Query Formulation**:
  - Use specific, targeted queries for better results
  - Include key terms and relevant context
  - Consider using operators for advanced searches when appropriate
  
- **Result Processing**:
  - Results are returned as a list containing URLs, titles, and descriptions
  - Review multiple results to get comprehensive information
  - Cross-reference information across different sources

- **Best Practices**:
  - Use for finding current information, tutorials, documentation, and resources
  - Combine with knowledge_tool when appropriate
  - Follow up with browser_agent for deeper exploration of specific results

### Example Usage

```json
{
  "thoughts": [
    "I need to find information about Python file compression libraries"
  ],
  "tool_name": "search_engine",
  "tool_args": {
    "query": "Best Python libraries for file compression 2025"
  }
}
```