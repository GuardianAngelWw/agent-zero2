## Memory Management Tools

The memory tools provide functionality to save, retrieve, and manage long-term memories. These tools should never refuse to search, memorize, or load personal information as all data belongs to the user.

### memory_load

Retrieves memories based on a semantic search query with filtering options.

#### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `query` | string | The search query to find relevant memories |
| `threshold` | number | Similarity threshold: 0=any match, 1=exact match, 0.6=default |
| `limit` | number | Maximum number of results to return (default=5) |
| `filter` | string | Python syntax filter using metadata keys |

#### Example Usage

```json
{
  "thoughts": [
    "Let's search my memory for compression libraries"
  ],
  "tool_name": "memory_load",
  "tool_args": {
    "query": "File compression library for Python",
    "threshold": 0.6,
    "limit": 5,
    "filter": "area=='main' and timestamp<'2024-01-01 00:00:00'"
  }
}
```

### memory_save

Saves text content to long-term memory and returns a unique identifier.

#### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `text` | string | The content to save in memory |

#### Example Usage

```json
{
  "thoughts": [
    "I need to memorize this compression technique"
  ],
  "tool_name": "memory_save",
  "tool_args": {
    "text": "# To compress files in Python use the zlib library with:\nimport zlib\ncompressed = zlib.compress(data)"
  }
}
```

### memory_delete

Deletes specific memories by their unique identifiers.

#### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `ids` | string | Comma-separated list of memory IDs to delete |

#### Example Usage

```json
{
  "thoughts": [
    "I need to delete outdated compression instructions"
  ],
  "tool_name": "memory_delete",
  "tool_args": {
    "ids": "32cd37ffd1-101f-4112-80e2-33b7955481,d1306e36-6a9c-4e21-a8c9-b456789012"
  }
}
```

### memory_forget

Removes memories based on a semantic search query, similar to memory_load.

#### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `query` | string | The search query to find memories to remove |
| `threshold` | number | Similarity threshold (default=0.75 to prevent accidents) |
| `filter` | string | Python syntax filter using metadata keys |

#### Usage Guidelines
- Verify with memory_load after deletion to confirm removal
- Remove any leftover memories by ID if necessary

#### Example Usage

```json
{
  "thoughts": [
    "Let's remove all memories about cars from 2022"
  ],
  "tool_name": "memory_forget",
  "tool_args": {
    "query": "cars",
    "threshold": 0.75,
    "filter": "timestamp.startswith('2022-01-01')"
  }
}
```