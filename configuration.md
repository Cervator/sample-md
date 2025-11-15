# Configuration Guide

This guide covers all configuration options for the Uplifted Mascot system.

## Environment Variables

The system uses environment variables for configuration. Create a `.env` file in your workspace:

```bash
# GCP Configuration
GCP_PROJECT_ID=your-project-id
GCP_REGION=us-central1

# Vector Search Configuration
VECTOR_INDEX_ID=your-index-id
VECTOR_ENDPOINT_ID=your-endpoint-id
DEPLOYED_INDEX_ID=um-deployed-index
```

## Document Processing Configuration

### Chunk Size

Control the size of text chunks during processing:

```python
# In process_docs.py
max_chunk_size = 1000  # characters
overlap = 200  # characters
```

**Recommendations:**
- Smaller chunks (500-800): Better for precise matching, more chunks
- Larger chunks (1500-2000): Better context, fewer chunks
- Default (1000): Good balance for most use cases

### File Filtering

Exclude certain files from processing:

```python
# Files to skip
exclude_patterns = [
    '.git',
    'node_modules',
    'CHANGELOG.md',
    'LICENSE.md'
]
```

## Embedding Configuration

### Model Selection

Choose the embedding model:

```python
# textembedding-gecko@001 produces 768-dimensional vectors
model = TextEmbeddingModel.from_pretrained("textembedding-gecko@001")
```

### Batch Size

Control API rate limiting:

```python
batch_size = 5  # Process 5 chunks at a time
time.sleep(0.5)  # Wait between batches
```

## Vector Search Configuration

### Index Settings

Configure the vector search index:

```yaml
dimensions: 768
approximateNeighborsCount: 10
distanceMeasureType: "DOT_PRODUCT_DISTANCE"
```

### Query Settings

Adjust query behavior:

```python
top_k = 5  # Number of results to retrieve
```

## RAG Service Configuration

### Mascot Personalities

Customize mascot personalities in `rag_service.py`:

```python
MASCOT_PERSONALITIES = {
    "gooey": "You are Gooey, a friendly...",
    "bill": "You are Bill, a pragmatic..."
}
```

### Response Generation

Control response generation:

```python
# Adjust context window
max_context_length = 4000  # characters

# Temperature (if using configurable models)
temperature = 0.7
```

## Performance Tuning

### For Faster Responses

- Reduce `top_k` to 3-5
- Use smaller chunk sizes
- Cache frequent queries

### For Better Accuracy

- Increase `top_k` to 10-15
- Use larger chunk sizes
- Fine-tune chunk overlap

## Security Configuration

### CORS Settings

Configure allowed origins:

```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://yourdomain.com"],  # Specific domains
    allow_credentials=True,
    allow_methods=["GET", "POST"],
    allow_headers=["*"],
)
```

### API Keys (Future)

When implementing API keys:

```python
# Add to request validation
api_key = request.headers.get("X-API-Key")
if not validate_api_key(api_key):
    raise HTTPException(status_code=401, detail="Invalid API key")
```

## Monitoring Configuration

### Logging

Configure logging levels:

```python
import logging
logging.basicConfig(level=logging.INFO)
```

### Metrics

Set up monitoring:

```python
# Track query metrics
query_count += 1
response_time = time.time() - start_time
```

## Best Practices

1. **Use Environment Variables**: Never hardcode sensitive values
2. **Version Control**: Keep `.env.example` in git, not `.env`
3. **Test Configurations**: Test changes in development first
4. **Document Changes**: Update this guide when adding new options

