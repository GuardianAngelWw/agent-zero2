## Browser Agent Tool

### Overview
The browser_agent tool provides a subordinate agent that controls a Playwright browser instance, allowing for web automation tasks.

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `message` | string | Instructions for the browser agent. Should be clear, specific, and task-oriented. |
| `reset` | string | When "true", spawns a new agent session. Use "false" when continuing an existing session. |

### Usage Guidelines

- **Session Management**:
  - Do not reset when iterating on the same task
  - Always include specific end conditions in your message
  
- **Communication Style**:
  - Be precise and descriptive in instructions
  - Include clear action steps and success criteria
  - When following up, start with "Considering open pages..."
  - Never use the phrase "wait for instructions" - use "end task" instead
  
- **File System**:
  - Downloads are saved to `/a0/tmp/downloads` by default

### Example Usage

Basic session with reset:
```json
{
  "thoughts": ["I need to log in to the specified service..."],
  "tool_name": "browser_agent",
  "tool_args": {
    "message": "Open example.com, log in with username 'test@example.com' and password from the user's input. Once logged in, download the latest report and end task.",
    "reset": "true"
  }
}
```

Continuing an existing session:
```json
{
  "thoughts": ["I need to navigate to the reports section..."],
  "tool_name": "browser_agent",
  "tool_args": {
    "message": "Considering open pages, click on the 'Reports' tab in the navigation menu, select 'Q2 2025', and download the PDF. Once download starts, end task.",
    "reset": "false"
  }
}
```