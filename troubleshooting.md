# Troubleshooting Guide

Common issues and solutions for the Uplifted Mascot system.

## Authentication Issues

### Problem: "GCP_PROJECT_ID environment variable not set"

**Solution:**
```bash
export GCP_PROJECT_ID="your-project-id"
# Or add to .env file
echo "GCP_PROJECT_ID=your-project-id" >> .env
```

### Problem: "Authentication failed"

**Solution:**
```bash
# Re-authenticate
gcloud auth application-default login

# Verify authentication
gcloud auth list
```

## Document Processing Issues

### Problem: "No markdown files found"

**Solution:**
- Check repository path is correct
- Verify files have `.md` extension
- Check file permissions

### Problem: "Chunks are too small or too large"

**Solution:**
Adjust chunk size in `process_docs.py`:
```python
max_chunk_size = 1000  # Increase or decrease as needed
overlap = 200  # Adjust overlap
```

### Problem: "Memory error during processing"

**Solution:**
- Process files individually
- Reduce batch size
- Increase system memory

## Embedding Issues

### Problem: "Rate limit exceeded"

**Solution:**
Increase delay between batches:
```python
time.sleep(1.0)  # Increase from 0.5
```

Or reduce batch size:
```python
batch_size = 3  # Reduce from 5
```

### Problem: "Embedding API errors"

**Solution:**
- Check API is enabled: `gcloud services list --enabled | grep aiplatform`
- Verify project has credits
- Check API quotas

## Vector Search Issues

### Problem: "Index not found"

**Solution:**
- Verify INDEX_ID is correct
- Check index exists: `gcloud ai indexes list`
- Ensure index is deployed to endpoint

### Problem: "Index creation taking too long"

**Solution:**
- This is normal for large datasets (30-60 minutes)
- Check status: `gcloud ai indexes describe INDEX_ID`
- Monitor in GCP console

### Problem: "Query returns no results"

**Solution:**
- Verify index is deployed
- Check query embedding is created correctly
- Ensure data was uploaded to index
- Try increasing `top_k`

## RAG Service Issues

### Problem: "Service won't start"

**Solution:**
Check environment variables:
```bash
echo $GCP_PROJECT_ID
echo $VECTOR_INDEX_ID
echo $VECTOR_ENDPOINT_ID
```

Verify all required variables are set.

### Problem: "CORS errors in browser"

**Solution:**
CORS middleware is already included. If issues persist:
- Check `allow_origins` configuration
- Verify frontend URL matches allowed origins
- Check browser console for specific error

### Problem: "Slow response times"

**Solution:**
- Reduce `top_k` in queries
- Check Vector Search latency
- Monitor Gemini API response times
- Consider caching frequent queries

### Problem: "Out of memory errors"

**Solution:**
Increase container resources:
```yaml
resources:
  requests:
    memory: "1Gi"  # Increase from 512Mi
    cpu: "500m"    # Increase from 250m
```

## Frontend Issues

### Problem: "Cannot connect to RAG service"

**Solution:**
- Verify RAG service is running
- Check service URL is correct
- Test with curl: `curl http://localhost:8000/health`
- Check firewall rules

### Problem: "Responses not displaying"

**Solution:**
- Check browser console for errors
- Verify JavaScript is enabled
- Check network tab for API calls
- Ensure CORS is configured

### Problem: "Widget not loading in iframe"

**Solution:**
- Check iframe src URL is correct
- Verify parent page allows iframes
- Check browser security settings

## General Issues

### Problem: "Costs higher than expected"

**Solution:**
- Monitor API usage in GCP console
- Implement response caching
- Reduce query frequency
- Optimize chunk sizes

### Problem: "Responses are inaccurate"

**Solution:**
- Increase `top_k` for more context
- Improve source documentation quality
- Adjust chunk sizes
- Review retrieved sources

### Problem: "System is slow"

**Solution:**
- Check GCP region selection
- Optimize Vector Search queries
- Use caching
- Scale service horizontally

## Getting Help

If you continue to experience issues:

1. Check the logs: `kubectl logs -l app=um-rag-service`
2. Review GCP console for errors
3. Check GitHub issues
4. Contact support with:
   - Error messages
   - Steps to reproduce
   - System configuration
   - Logs (sanitized)

## Common Error Messages

### "Vertex AI API not enabled"
```bash
gcloud services enable aiplatform.googleapis.com
```

### "Insufficient permissions"
```bash
# Grant necessary roles
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member="user:YOUR_EMAIL" \
  --role="roles/aiplatform.user"
```

### "Bucket not found"
```bash
# Verify bucket exists
gsutil ls gs://BUCKET_NAME
```

### "Index deployment failed"
- Check index status
- Verify endpoint exists
- Review deployment logs

