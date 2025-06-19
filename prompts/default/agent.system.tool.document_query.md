## Document Query Tool

### Overview
The document_query tool provides capabilities to read, extract, and analyze content from a wide variety of document types, both remote and local. It supports OCR (Optical Character Recognition) for processing images and PDFs.

### Capabilities
- Extract text content from webpages and remote documents
- Read local document content from the filesystem
- Answer specific queries about document content
- Process multiple file formats including HTML, PDF, Office documents, and text files

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `document` | string | Yes | URL or file path to the document. Web documents require "http://" or "https://" prefix. Local files can use optional "file:" protocol prefix but must include full filesystem path. |
| `queries` | list[str] | No | Optional list of specific questions to answer about the document. If omitted, returns full document text. |

### Usage Guidelines

- **Document Access**:
  - Web documents must include complete URLs with protocol (http:// or https://)
  - Local documents must use absolute paths
  - Ensure you have proper permissions to access the document

- **Query Formulation**:
  - For general content extraction, omit the queries parameter
  - For specific information, provide focused questions as an array
  - Multiple queries are processed in sequence against the same document

- **Best Practices**:
  - Use specific queries to extract targeted information from large documents
  - When dealing with complex documents, break down analysis into multiple specific queries
  - For comprehensive analysis, combine document_query with memory tools to store and process document content

### Example 1: Extract Full Document Content

```json
{
  "thoughts": [
    "I need to get the full content of a web document"
  ],
  "tool_name": "document_query",
  "tool_args": {
    "document": "https://example.com/sample-document.pdf"
  }
}
```

**Response:**
```plaintext
... Here is the entire content of the web document requested ...
```

### Example 2: Answer Specific Questions About a Document

```json
{
  "thoughts": [
    "I need specific information about this technical document"
  ],
  "tool_name": "document_query",
  "tool_args": {
    "document": "https://example.com/technical-specification.pdf",
    "queries": [
      "What is the main topic?",
      "What are the system requirements?",
      "What version is this document?"
    ]
  }
}
```

**Response:**
```plaintext
# What is the main topic?
The document covers API specifications for the XYZ cloud service platform.

# What are the system requirements?
System requirements include:
- 64-bit operating system
- 8GB RAM minimum
- 100MB available disk space
- Internet connection with minimum 5Mbps bandwidth

# What version is this document?
This is version 2.4.1 last updated on March 15, 2025.
```