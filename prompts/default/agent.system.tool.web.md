## Web Content Tool

### Overview
The webpage_content_tool retrieves and extracts text content from websites including news sites, wikis, blogs, and other web resources. This tool enables the agent to gather online information and reference current web content.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `url` | string | Yes | The complete URL of the webpage to extract content from. Must include protocol prefix (http:// or https://). |

### Usage Guidelines

- Always provide complete URLs including the protocol prefix (http:// or https://)
- The tool extracts the main textual content, filtering out most navigation elements and ads
- For news articles, the tool attempts to identify and extract the primary article content
- The tool does not execute JavaScript or interact with dynamic content
- Response size is limited and very large pages may be truncated

### Best Practices

- Use this tool when you need to reference or analyze current web content
- For structured data extraction from websites, consider using more specialized tools
- When fetching news content, verify the source is reputable
- Check that the URL is correctly formatted before submitting

### Example Usage

```json
{
    "thoughts": [
        "I need to get information about recent developments in renewable energy from a news site"
    ],
    "tool_name": "webpage_content_tool",
    "tool_args": {
        "url": "https://example.com/renewable-energy-news"
    }
}
```