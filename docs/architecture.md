# Architecture

## Overview

The repository demonstrates two RAG implementations in n8n:

1. Pinecone-based architecture with separate ingestion and retrieval workflows.
2. Supabase-based architecture with ingestion and retrieval combined in one workflow.

## Pinecone variant

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

Each document receives metadata such as `source`, `date`, `department` and `role`.

### Retrieval

```text
Chat Trigger
  ↓
AI Agent
  ├── OpenAI Chat Model
  └── Pinecone Vector Store tool
         └── OpenAI Embeddings
```

The vector store is attached to the AI Agent in `Retrieve Documents (As Tool for AI Agent)` mode.

The agent is instructed to use retrieved knowledge for internal-policy questions and avoid unsupported answers.

### Critical Pinecone configuration

Ingestion and retrieval must use the same:

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

## Supabase variant

```text
Ingestion branch:
Manual Trigger
  → Default Data Loader
  → OpenAI Embeddings
  → Supabase Vector Store

Retrieval branch:
Chat Trigger
  → AI Agent
     ├── OpenAI Chat Model
     ├── Simple Memory
     └── Supabase Vector Store tool
          └── OpenAI Embeddings
```

The Supabase example uses a `documents` table and a `match_documents` query function for vector similarity search.

This variant demonstrates that the same RAG pattern can be implemented with a PostgreSQL/pgvector-backed vector store rather than Pinecone.

## Why keep both variants

The two implementations demonstrate the same core retrieval pattern with different storage backends:

- Pinecone is shown as a dedicated managed vector database.
- Supabase is shown as a PostgreSQL-based alternative with vector search.
- The retrieval layer can be changed without changing the overall AI-agent architecture.

## Portfolio note

The repository contains sanitized workflow exports and synthetic demo knowledge rather than private company data. Credential references and instance-specific identifiers are removed before publication.
