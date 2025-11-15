# API Reference

Complete API documentation for the Uplifted Mascot system.

## Overview

The Uplifted Mascot API provides endpoints for querying the knowledge base and interacting with AI-powered mascots.

## Base URL

All API requests should be made to:

```
http://your-service-url:8000
```

## Authentication

Currently, the API does not require authentication for development. In production, you should implement API keys or OAuth.

## Endpoints

### Health Check

Check if the service is running and healthy.

**Endpoint:** `GET /health`

**Response:**
```json
{
  "status": "healthy",
  "vector_index_configured": true,
  "endpoint_configured": true
}
```

### Ask Mascot

Query the mascot with a question.

**Endpoint:** `POST /ask-mascot`

**Request Body:**
```json
{
  "project": "bifrost",
  "mascot": "gooey",
  "question": "How does the JEP workflow work?",
  "top_k": 5
}
```

**Response:**
```json
{
  "response": "The JEP (Just Enough Porting) workflow is a tiered system...",
  "sources": [
    "bifrost-protocol.md",
    "jep-workflow.md"
  ],
  "confidence": 0.92
}
```

**Parameters:**
- `project` (string, required): Project identifier
- `mascot` (string, required): Mascot name (e.g., "gooey", "bill")
- `question` (string, required): User's question
- `top_k` (integer, optional): Number of context chunks to retrieve (default: 5)
- `context` (string, optional): Context of the request ("web" or "in-game", default: "web")

## Error Responses

### 400 Bad Request

Returned when request parameters are invalid.

```json
{
  "detail": "Unknown mascot: invalid_mascot. Available: ['gooey', 'bill']"
}
```

### 500 Internal Server Error

Returned when an internal error occurs.

```json
{
  "detail": "Internal server error"
}
```

## Rate Limiting

Currently, there are no rate limits. In production, implement rate limiting to prevent abuse.

## Examples

### Example: Basic Query

```bash
curl -X POST http://localhost:8000/ask-mascot \
  -H "Content-Type: application/json" \
  -d '{
    "project": "bifrost",
    "mascot": "gooey",
    "question": "What is Bifrost?"
  }'
```

### Example: With Custom Top-K

```bash
curl -X POST http://localhost:8000/ask-mascot \
  -H "Content-Type: application/json" \
  -d '{
    "project": "bifrost",
    "mascot": "gooey",
    "question": "Explain the architecture",
    "top_k": 10
  }'
```

## Response Format

All successful responses follow this structure:

- `response` (string): The generated answer from the mascot
- `sources` (array): List of source files used to generate the answer
- `confidence` (float): Confidence score between 0.0 and 1.0

## Best Practices

1. **Be Specific**: More specific questions yield better results
2. **Use Context**: Include relevant context in your questions
3. **Check Sources**: Review the sources array to verify information
4. **Handle Errors**: Always implement proper error handling

