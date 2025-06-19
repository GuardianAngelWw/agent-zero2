## Task Scheduler System

### Overview
The task scheduler is a subsystem that enables agent-zero to execute arbitrary tasks defined by a "system prompt" and "user prompt". Tasks run asynchronously in the background and can be scheduled to execute at specific times or intervals.

### Key Concepts

- **Execution Context**: When a task executes, prompts run in a conversation context to complete the defined task
  - **Dedicated Context**: Task runs in its own chat session (set with `dedicated_context: true`)
  - **Shared Context**: Task runs in the chat where it was created, including chat history (default)

- **Task Types**:
  - **Scheduled**: Recurring tasks using crontab syntax (e.g., `*/5 * * * *` for every 5 minutes)
  - **Planned**: One-time tasks at specific future datetime(s)
  - **AdHoc**: Manually triggered tasks with no automatic schedule

- **Important Guidelines**:
  - Do not manually run tasks that are already scheduled
  - Do not create recursive prompts that would create endless task loops
  - When a user asks to execute a task, first check if it exists before creating a new one

### Scheduler Tools

#### scheduler:list_tasks

Lists all tasks in the system with their details.

##### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `state` | list(str) | No | Filter by task state: "idle", "running", "disabled", "error" |
| `type` | list(str) | No | Filter by task type: "adhoc", "planned", "scheduled" |
| `next_run_within` | int | No | Tasks whose next run is within this many minutes |
| `next_run_after` | int | No | Tasks whose next run is after this many minutes |

##### Example Usage

```json
{
  "thoughts": [
    "I need to find planned tasks in the idle or error state that will run within 20 minutes"
  ],
  "tool_name": "scheduler:list_tasks",
  "tool_args": {
    "state": ["idle", "error"],
    "type": ["planned"],
    "next_run_within": 20
  }
}
```

#### scheduler:find_task_by_name

Finds tasks by partial or exact name match.

##### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | str | Yes | Task name to search for (partial matches allowed) |

##### Example Usage

```json
{
  "thoughts": [
    "I need to find all email notification tasks"
  ],
  "tool_name": "scheduler:find_task_by_name",
  "tool_args": {
    "name": "email"
  }
}
```

#### scheduler:show_task

Shows detailed information about a specific task.

##### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `uuid` | str | Yes | UUID of the task to display |

##### Example Usage

```json
{
  "thoughts": [
    "I need to see the details of task xxx-yyy-zzz"
  ],
  "tool_name": "scheduler:show_task",
  "tool_args": {
    "uuid": "xxx-yyy-zzz"
  }
}
```

#### scheduler:run_task

Manually executes a task that is not in the "running" state.

##### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `uuid` | str | Yes | UUID of the task to run |
| `context` | str | No | Text to prepend to the task prompt as additional context |

##### Usage Guidelines
- Best used with "adhoc" tasks or for testing scheduled tasks
- Only run tasks in the "idle" state
- Context parameter allows passing results from one task to another

##### Example Usage

```json
{
  "thoughts": [
    "I need to run the data analysis task with today's metrics as input"
  ],
  "tool_name": "scheduler:run_task",
  "tool_args": {
    "uuid": "xyz-123",
    "context": "Today's metrics: Visitors: 1,532, Conversions: 87, Revenue: $12,450"
  }
}
```

#### scheduler:delete_task

Removes a task from the scheduler system.

##### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `uuid` | str | Yes | UUID of the task to delete |

##### Example Usage

```json
{
  "thoughts": [
    "I need to remove the outdated notification task"
  ],
  "tool_name": "scheduler:delete_task",
  "tool_args": {
    "uuid": "xyz-123"
  }
}
```

#### scheduler:create_scheduled_task

Creates a recurring task using crontab scheduling.

##### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | str | Yes | Name of the task (displayed in task lists) |
| `system_prompt` | str | Yes | System prompt for task execution context |
| `prompt` | str | Yes | User prompt defining the task to execute |
| `schedule` | dict | Yes | Crontab fields as key-value pairs: minute, hour, day, month, weekday |
| `attachments` | list[str] | No | File paths or URLs to include as attachments |
| `dedicated_context` | bool | No | If true, run in separate chat; if false (default), run in creation context |

##### Example Usage

```json
{
  "thoughts": [
    "I need to create a scheduled task to check for new data every 20 minutes"
  ],
  "tool_name": "scheduler:create_scheduled_task",
  "tool_args": {
    "name": "Data Monitor",
    "system_prompt": "You are a data analysis assistant",
    "prompt": "Check the API at https://api.example.com/data for new entries since the last check. Summarize any new findings.",
    "attachments": [],
    "schedule": {
      "minute": "*/20",
      "hour": "*",
      "day": "*",
      "month": "*",
      "weekday": "*"
    },
    "dedicated_context": true
  }
}
```

#### scheduler:create_adhoc_task

Creates a manually triggered task with no automatic schedule.

##### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | str | Yes | Name of the task (displayed in task lists) |
| `system_prompt` | str | Yes | System prompt for task execution context |
| `prompt` | str | Yes | User prompt defining the task to execute |
| `attachments` | list[str] | No | File paths or URLs to include as attachments |
| `dedicated_context` | bool | No | If true, run in separate chat; if false (default), run in creation context |

##### Example Usage

```json
{
  "thoughts": [
    "I need to create an on-demand report generator task"
  ],
  "tool_name": "scheduler:create_adhoc_task",
  "tool_args": {
    "name": "Financial Report Generator",
    "system_prompt": "You are a financial analysis expert",
    "prompt": "Generate a financial analysis report based on the provided data",
    "attachments": [],
    "dedicated_context": true
  }
}
```

#### scheduler:create_planned_task

Creates a task that executes at specific future datetime(s).

##### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | str | Yes | Name of the task (displayed in task lists) |
| `system_prompt` | str | Yes | System prompt for task execution context |
| `prompt` | str | Yes | User prompt defining the task to execute |
| `plan` | list[str] | Yes | List of ISO datetime strings (YYYY-MM-DDThh:mm:ss format) |
| `attachments` | list[str] | No | File paths or URLs to include as attachments |
| `dedicated_context` | bool | No | If true, run in separate chat; if false (default), run in creation context |

##### Example Usage

```json
{
  "thoughts": [
    "I need to schedule a reminder task for tomorrow at 6:25 PM"
  ],
  "tool_name": "scheduler:create_planned_task",
  "tool_args": {
    "name": "Meeting Reminder",
    "system_prompt": "You are a helpful assistant",
    "prompt": "Remind the user about the quarterly planning meeting. Send details about required preparation.",
    "attachments": [],
    "plan": ["2025-04-29T18:25:00"],
    "dedicated_context": false
  }
}
```

#### scheduler:wait_for_task

Waits for a task to complete and returns the result of the last execution.

##### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `uuid` | str | Yes | UUID of the task to wait for |

##### Usage Guidelines
- Only works for tasks with `dedicated_context=True`
- Will wait if task is currently running

##### Example Usage

```json
{
  "thoughts": [
    "I need the results from the data analysis task before proceeding"
  ],
  "tool_name": "scheduler:wait_for_task",
  "tool_args": {
    "uuid": "xyz-123"
  }
}
```