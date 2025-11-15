# Getting Started

Welcome to the Uplifted Mascot system! This guide will help you get up and running quickly.

## Prerequisites

Before you begin, ensure you have:

- Python 3.9 or higher installed
- Google Cloud Platform account with Vertex AI enabled
- Basic familiarity with command-line tools
- Git installed for repository access

## Quick Start

### Step 1: Clone the Repository

First, clone the documentation repository to your local machine:

```bash
git clone https://github.com/your-org/your-docs.git
cd your-docs
```

### Step 2: Set Up Environment

Create a virtual environment and install dependencies:

```bash
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install google-cloud-aiplatform
```

### Step 3: Authenticate

Authenticate with Google Cloud:

```bash
gcloud auth application-default login
export GCP_PROJECT_ID="your-project-id"
```

### Step 4: Process Documents

Run the document processing script:

```bash
python process_docs.py . chunks.json
```

### Step 5: Create Embeddings

Generate embeddings for your chunks:

```bash
python create_embeddings.py chunks.json embeddings.json
```

## Next Steps

Once you've completed the quick start:

1. Review the [Configuration Guide](configuration.md) for advanced settings
2. Check the [API Reference](api-reference.md) for detailed API documentation
3. Read the [Architecture Overview](architecture.md) to understand the system design

## Troubleshooting

If you encounter issues, see the [Troubleshooting Guide](troubleshooting.md) for common solutions.

## Support

For additional help, visit our documentation or contact support.

