# Architecture

## Overview

The project contains two independent n8n workflows connected through the same Pinecone index and namespace.

### Ingestion

```text
Manual Trigger
  ↓
Prepare sample fragments
  ↓
Default Data Loader
  ↓
OpenAI Embeddings
  ↓
Pinecone Vector Store
```

Each document receives metadata:

- `source`
- `date`
- `department`
- `role`

The current sample knowledge base contains synthetic company rules about returns, delivery incidents and sales permissions.

### Retrieval

```text
Chat Trigger
  ↓
AI Agent
  ├── OpenAI Chat Model
  └── Pinecone Vector Store tool
         └── OpenAI Embeddings
```

The vector store is attached to the AI Agent as a tool in `Retrieve Documents (As Tool for AI Agent)` mode.

The agent system prompt forces retrieval for questions about internal company policies and prevents unsupported answers from general model knowledge.

## Critical configuration

The most common source of retrieval failure is a mismatch between ingestion and retrieval configuration.

Both workflows must use the same:

```text
Pinecone index
Pinecone namespace
Embedding model / compatible vector dimensions
```

The public example uses:

```text
Index: rag-knowledge-base
Namespace: demo
```

## Why two workflows

Separating ingestion from retrieval makes the design easier to operate:

- the knowledge base can be updated independently;
- chat queries do not re-index documents;
- ingestion can later be triggered by file uploads, schedules or external storage;
- retrieval remains fast and focused on user requests.

## Portfolio note

This repository uses synthetic demo regulations rather than private company data.
