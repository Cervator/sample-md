# Architecture Overview

This document describes the architecture of the Uplifted Mascot system.

## System Components

The Uplifted Mascot system consists of five main components:

### 1. Knowledge Base

The knowledge base stores the source documentation. This is typically a Git repository containing markdown files.

**Location:** Git repositories (GitHub, GitLab, etc.)

**Format:** Markdown files (.md)

**Responsibilities:**
- Store project documentation
- Version control for documentation
- Source of truth for knowledge

### 2. Ingestion Pipeline

The ingestion pipeline processes documents and creates embeddings.

**Components:**
- Document processor: Chunks markdown files
- Embedding generator: Creates vector embeddings
- Format converter: Converts to Vector Search format

**Workflow:**
1. Read markdown files from repository
2. Chunk text into semantic segments
3. Generate embeddings using Vertex AI
4. Convert to JSONL format
5. Upload to Cloud Storage

### 3. Vector Storage

Vector storage provides semantic search capabilities.

**Technology:** Vertex AI Vector Search

**Responsibilities:**
- Store document embeddings
- Enable semantic search
- Return relevant chunks for queries

**Configuration:**
- Dimensions: 768 (for textembedding-gecko@001)
- Distance measure: Dot product
- Algorithm: Tree-based approximate nearest neighbor

### 4. RAG Service

The RAG (Retrieval-Augmented Generation) service provides the API endpoint.

**Technology:** FastAPI on GKE

**Responsibilities:**
- Receive user queries
- Retrieve relevant context from Vector Search
- Generate responses using Gemini
- Return formatted answers

**Endpoints:**
- `GET /health` - Health check
- `POST /ask-mascot` - Query endpoint

### 5. Frontend

The frontend provides the user interface.

**Technology:** HTML, CSS, JavaScript

**Components:**
- Full page interface (index.html)
- Embeddable widget (widget.html)

**Responsibilities:**
- Display chat interface
- Send queries to RAG service
- Display responses and sources

## Data Flow

### Query Flow

1. User submits question via frontend
2. Frontend sends POST request to RAG service
3. RAG service creates query embedding
4. Vector Search finds relevant chunks
5. RAG service builds prompt with context
6. Gemini generates response
7. Response returned to frontend
8. Frontend displays answer to user

### Ingestion Flow

1. Git repository updated with new docs
2. Jenkins triggers ingestion pipeline
3. Document processor chunks files
4. Embedding generator creates vectors
5. Format converter creates JSONL
6. Files uploaded to Cloud Storage
7. Vector Search index updated
8. New knowledge available for queries

## Technology Stack

### Backend

- **Python 3.9+**: Main programming language
- **FastAPI**: Web framework
- **Vertex AI**: Embeddings and LLM
- **GKE**: Container orchestration

### Frontend

- **HTML5**: Structure
- **CSS3**: Styling
- **JavaScript (ES6+)**: Interactivity

### Infrastructure

- **Google Cloud Platform**: Hosting
- **Cloud Storage**: File storage
- **Vector Search**: Semantic search
- **Kubernetes**: Container management

## Scalability Considerations

### Horizontal Scaling

The RAG service can be scaled horizontally:

```yaml
replicas: 3  # Run multiple instances
```

### Caching

Implement caching for:
- Frequent queries
- Embedding results
- Response generation

### Rate Limiting

Add rate limiting to prevent abuse:

```python
from slowapi import Limiter
limiter = Limiter(key_func=get_remote_address)
```

## Security Architecture

### Authentication

- API keys (future)
- OAuth 2.0 (future)
- Service account authentication (GCP)

### Data Privacy

- No user data stored
- Queries not logged (configurable)
- Source attribution in responses

### Network Security

- HTTPS/TLS for all connections
- CORS configuration
- Firewall rules

## Monitoring and Observability

### Metrics

Track:
- Query count
- Response time
- Error rate
- Vector Search latency

### Logging

Log:
- API requests
- Errors and exceptions
- Performance metrics

### Alerts

Set up alerts for:
- High error rates
- Slow response times
- Service downtime

## Future Enhancements

### Planned Features

1. **Multi-language Support**: Translate responses
2. **Conversation History**: Remember context
3. **Voice Integration**: Text-to-speech
4. **Analytics Dashboard**: Usage statistics
5. **Custom Mascots**: User-defined personalities

### Performance Improvements

1. **Response Caching**: Cache common queries
2. **Batch Processing**: Process multiple queries
3. **CDN Integration**: Faster frontend delivery
4. **Edge Computing**: Reduce latency

