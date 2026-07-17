# RCA Agent AI Enterprise

## Overview

**RCA Agent AI Enterprise** is an intelligent Root Cause Analysis (RCA) system powered by LLM-enabled retrieval and recommendation pipelines. It provides two complementary agents:

1. **Self-Healing Consumer AI Agent Chatbot**: Engages users through conversational prompts to diagnose and resolve issues autonomously
2. **Troubleshooting AI Agent**: Assists NOC and DevOps engineers with Tier 1 ticket handling for networking and application issues

This solution is **cloud-agnostic** and optimized for enterprise Kubernetes environments, leveraging:

- **LangChain**: Orchestration of LLM workflows
- **Vector Databases**: FAISS (local), Pinecone (cloud), Weaviate (self-hosted), pgvector (PostgreSQL)
- **Search Engines**: OpenSearch for indexed knowledge base retrieval
- **Data Sources**: Prometheus metrics and kubectl logs

## Key Features

✅ **Multi-Source Integration**: Prometheus metrics, kubectl logs, KB articles  
✅ **Semantic Search**: Find relevant RCA articles and troubleshooting guides  
✅ **Real-Time Anomaly Analysis**: Correlate metrics and logs to identify root causes  
✅ **Enterprise KB Retrieval**: Vector-based similarity search on company documentation  
✅ **Cloud-Agnostic**: Works on AWS, GCP, Azure, or on-premises Kubernetes  
✅ **Multi-Vector DB Support**: FAISS, Pinecone, Weaviate, pgvector  
✅ **Conversational Interface**: Natural language RCA assistance for engineers  
✅ **Production-Ready**: Docker, Kubernetes, and Helm support  

## Quick Start

### Prerequisites
- Python 3.9+
- Docker & Docker Compose (for local development)
- Kubernetes cluster (for production deployment)
- OpenAI API key or compatible LLM

### Installation

```bash
git clone https://github.com/payamdsp/RCA_Agent_AI_Enterprise.git
cd RCA_Agent_AI_Enterprise
pip install -r requirements.txt
cp .env.example .env
```

### Run with Docker Compose

```bash
docker-compose up -d
```

Access at http://localhost:8000

## Architecture

```
User Interface (Chat, Slack, Web Portal)
              ↓
LangChain Orchestration (Chains, Memory, Tool Routing)
              ↓
Retrieval Engine (Vector Search, Semantic Search)
              ↓
Data Sources (Prometheus, kubectl logs, KB Articles)
```

## API Endpoints

- `GET /api/v1/health` - Health check
- `POST /api/v1/analyze` - Analyze anomaly
- `POST /api/v1/chat` - Chat with RCA agent
- `GET /api/v1/knowledge-base` - Search KB
- `POST /api/v1/metric-context` - Get metric context

## Configuration

### Vector Database
```env
VECTOR_DB_TYPE=faiss  # Options: faiss, pinecone, weaviate, pgvector
```

### LLM Provider
```env
LLM_PROVIDER=openai  # Options: openai, anthropic, ollama, bedrock
OPENAI_API_KEY=sk-xxx
```

### AWS (Optional - Cloud-Agnostic)
```env
AWS_BEDROCK_ENABLED=false
AWS_OPENSEARCH_ENABLED=false
```

## Documentation

- [ARCHITECTURE.md](docs/ARCHITECTURE.md) - System design
- [DEPLOYMENT.md](docs/DEPLOYMENT.md) - Deployment guides
- [KB_INGESTION.md](docs/KB_INGESTION.md) - Knowledge base setup
- [API.md](docs/API.md) - API reference

## Cloud-Agnostic Philosophy

This solution works in any cloud environment with no vendor lock-in:
- Optional AWS Bedrock, OpenSearch, RDS, CloudWatch
- Support for GCP and Azure
- On-premises Kubernetes compatible

## License

MIT License

---

**Version**: 1.0.0  
**Last Updated**: January 2024
