### code_execution_tool

This tool enables you to execute terminal commands, Python, and Node.js code for computational tasks and software operations.

#### Configuration Parameters:

1. **code** (required)
   - Place executable code within this parameter
   - Ensure proper escaping and consistent indentation
   - Use proper syntax based on the selected runtime

2. **runtime** (required)
   - Available options:
     - `terminal`: For shell commands and system operations
     - `python`: For Python code execution
     - `nodejs`: For JavaScript execution in Node.js environment
     - `output`: To retrieve output from long-running processes
     - `reset`: To terminate a running process or session

3. **session** (optional)
   - Default: 0 (primary session)
   - Use different integer values for parallel execution contexts

#### Usage Guidelines:

- **Process Management**:
  - For long-running code, use `"runtime": "output"` to wait for and retrieve results
  - Use `"runtime": "reset"` to terminate unresponsive processes
  
- **Package Management**:
  - Use `pip` for Python dependencies (with `terminal` runtime)
  - Use `npm` for Node.js dependencies (with `terminal` runtime)
  - Use `apt-get` for system packages (with `terminal` runtime)

- **Best Practices**:
  - For output generation, use `print()` in Python or `console.log()` in Node.js
  - If errors occur, modify code before retrying; use knowledge_tool for debugging assistance
  - Remove all placeholders and example data before execution
  - Verify dependencies are installed before running dependent code
  - Note that outputs may include framework information in format: [SYSTEM: ...]

- **Constraints**:
  - Only use with "thoughts" tool between code executions
  - Wait for execution response before using other tools
  - Do not reuse code snippets without validating their appropriateness

#### Examples:

1. **Execute Python code**:
```json
{
    "thoughts": [
        "Need to get the current working directory",
        "I can use os.getcwd() in Python",
        "This will help determine where files will be saved"
    ],
    "tool_name": "code_execution_tool",
    "tool_args": {
        "runtime": "python",
        "session": 0,
        "code": "import os\nprint(os.getcwd())"
    }
}
```

2. **Execute terminal command**:
```json
{
    "thoughts": [
        "Need to install the zip utility",
        "Using apt-get to install the package"
    ],
    "tool_name": "code_execution_tool",
    "tool_args": {
        "runtime": "terminal",
        "session": 0,
        "code": "apt-get install zip"
    }
}
```

3. **Wait for output from long-running scripts**:
```json
{
    "thoughts": [
        "Waiting for the data processing task to complete"
    ],
    "tool_name": "code_execution_tool",
    "tool_args": {
        "runtime": "output",
        "session": 0
    }
}
```

4. **Reset a terminal session**:
```json
{
    "thoughts": [
        "The process is unresponsive and needs to be terminated"
    ],
    "tool_name": "code_execution_tool",
    "tool_args": {
        "runtime": "reset",
        "session": 0
    }
}
```