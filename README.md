# Pathway RAG App with Real-Time Document Indexing

This project demonstrates how to create a Retrieval-Augmented Generation (RAG) application using [Pathway](https://github.com/pathwaycom/pathway) that provides always up-to-date knowledge to your Language Model (LLM) without the need for a separate ETL process.

[![Deploy with GCP](https://www.gstatic.com/pantheon/images/welcome/supercloud.svg)](https://pathway.com/developers/user-guide/deployment/gcp-deploy) [![Deploy with Render](../../../assets/render.png)](https://pathway.com/developers/user-guide/deployment/render-deploy)

## Features

- Create a vector store with real-time document indexing from:
  - Google Drive
  - Microsoft 365 SharePoint
  - Local directory
- Connect to an LLM model of your choice
- Get accurate and precise responses to your questions
- Filter questions by folders or file types
- Summarize texts using LLMs
- Generate executive summaries for questions across different files

## Table of Contents

- [How It Works](#how-it-works)
- [Configuration](#configuration)
- [Running the Project](#running-the-project)
- [Using the App](#using-the-app)
- [API Endpoints](#api-endpoints)

## How It Works

This pipeline uses Pathway connectors to read data from various sources (local drive, Google Drive, Microsoft SharePoint) with low-latency change tracking. The contents are read into a Pathway Table as binary objects, parsed using the [unstructured](https://unstructured.io/) library, and split into chunks. These chunks are then embedded using an LLM API.

Finally, the embeddings are indexed using Pathway's machine-learning library, allowing users to query the created index through simple HTTP requests.

## Configuration

### LLM Model

Configure the LLM model in `config.yaml`:

```yaml
llm_config:
  model: "gemini/gemini-pro"
```

### Webserver

Configure the host and port in `config.yaml`:

```yaml
host_config:
  host: "0.0.0.0"
  port: 8000
```

### Data Sources

Configure data sources in the `sources` section of `config.yaml`. Supported types:
- Local
- Google Drive
- SharePoint (commercial offering)

Example configuration:

```yaml
sources:
  - local_files:
    kind: local
    config:
      path: "data/"
  # Uncomment and configure for Google Drive or SharePoint
```

### API Key

Set your API key in the `.env` file:

```
GEMINI_API_KEY=your_api_key_here
```

## Running the Project

### Locally

1. Install dependencies:
   ```
   pip install -r requirements.txt
   ```

2. Run the app:
   ```
   python app.py
   ```

### With Docker

1. Build the Docker image:
   ```
   docker build -t qa .
   ```

2. Run the container:
   ```
   docker run -v `pwd`/data:/app/data -p 8000:8000 qa
   ```

## Using the App

Once the app is running, you can interact with it using the provided API endpoints. Use `curl` commands or a tool like Postman to send requests to `http://localhost:8000/v1/endpoint_name`.

## API Endpoints

- `/v1/retrieve`: Perform similarity search
- `/v1/statistics`: Get basic indexer health stats
- `/v1/pw_list_documents`: Retrieve metadata of processed files
- `/v1/pw_ai_answer`: Ask questions about documents or interact with the LLM
- `/v1/pw_ai_summary`: Summarize a list of texts

For detailed usage of each endpoint, refer to the [API documentation](https://pathway.com/solutions/ai-pipelines).

---

For more information on Pathway and its capabilities, visit the [Pathway documentation](https://pathway.com/developers/user-guide/introduction/welcome).
