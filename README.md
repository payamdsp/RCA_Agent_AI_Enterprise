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
- **Multi-Cloud Support**: AWS, GCP, Azure, or on-premises

## Key Features

✅ **Multi-Source Integration**: Prometheus metrics, kubectl logs, KB articles
✅ **Semantic Search**: Find relevant RCA articles and troubleshooting guides
✅ **Real-Time Anomaly Analysis**: Correlate metrics and logs to identify root causes
✅ **Enterprise KB Retrieval**: Vector-based similarity search on company documentation
✅ **Cloud-Agnostic Deployment**: Works on AWS, GCP, Azure, or on-premises Kubernetes
✅ **Multi-Vector DB Support**: FAISS, Pinecone, Weaviate, pgvector
✅ **Conversational Interface**: Natural language RCA assistance for engineers
✅ **Production-Ready**: Docker, Kubernetes, and Helm support

## Quick Start

### Prerequisites
- Python 3.9+
- Docker & Docker Compose
- Kubernetes cluster (optional)
- OpenAI API key or compatible LLM

### Installation

```bash
# Clone repository
git clone https://github.com/payamdsp/RCA_Agent_AI_Enterprise.git
cd RCA_Agent_AI_Enterprise

# Install dependencies
pip install -r requirements.txt

# Configure environment
cp .env.example .env
# Edit .env with your settings
```

### Run with Docker Compose

```bash
docker-compose up -d
```

Services will be available at:
- RCA Agent API: http://localhost:8000
- Weaviate: http://localhost:8080
- OpenSearch: http://localhost:9200
- Prometheus: http://localhost:9090
- Grafana: http://localhost:3000
- PostgreSQL: localhost:5432

## Architecture

```
┌─────────────────────────────────────┐
│      User Interface Layer           │
│  (Chat UI, Slack Bot, Web Portal)  │
└─────────────────────────────────────┘
              │
┌─────────────────────────────────────┐
│   LangChain Orchestration Layer     │
│  (Chains, Memory, Tool Routing)     │
└─────────────────────────────────────┘
              │
┌─────────────────────────────────────┐
│  Retrieval & Recommendation Engine  │
│  (Vector Search, Semantic Search)   │
└─────────────────────────────────────┘
              │
┌─────────────────────────────────────┐
│   Data Ingestion & Processing       │
│  (Prometheus, kubectl, KB Articles) │
└─────────────────────────────────────┘
```

## Configuration

### Vector Database Selection

```env
# Options: faiss, pinecone, weaviate, pgvector
VECTOR_DB_TYPE=faiss

# FAISS (Local)
FAISS_INDEX_PATH=./data/faiss_index

# Pinecone (Cloud)
PINECONE_API_KEY=your-key
PINECONE_ENVIRONMENT=your-env

# Weaviate (Self-Hosted)
WEAVIATE_URL=http://localhost:8080

# pgvector (PostgreSQL)
PGVECTOR_HOST=localhost
PGVECTOR_PORT=5432
```

### LLM Configuration

```env
# Options: openai, anthropic, ollama, bedrock
LLM_PROVIDER=openai
LLM_MODEL=gpt-4
OPENAI_API_KEY=sk-xxx
```

### AWS Integration (Optional)

```env
# Cloud-agnostic: AWS services are optional
AWS_BEDROCK_ENABLED=false        # Use AWS Bedrock for LLM
AWS_OPENSEARCH_ENABLED=false     # Use AWS OpenSearch
AWS_RDS_ENABLED=false            # Use AWS RDS with pgvector
AWS_CLOUDWATCH_ENABLED=false     # Use CloudWatch for logs
```

## API Usage

### Analyze Anomaly

```bash
curl -X POST http://localhost:8000/api/v1/analyze \
  -H "Content-Type: application/json" \
  -d '{
    "metric_name": "cpu_usage",
    "metric_value": 95,
    "service": "api-gateway",
    "namespace": "production"
  }'
```

### Chat with RCA Agent

```bash
curl -X POST http://localhost:8000/api/v1/chat \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Why is API gateway experiencing high CPU?",
    "context": {"pod": "api-gateway-5d4f8", "namespace": "production"}\n  }'\n```\n\n### Search Knowledge Base\n\n```bash\ncurl \"http://localhost:8000/api/v1/knowledge-base?query=database+timeout&limit=10\"\n```\n\n### Get Metric Context\n\n```bash\ncurl -X POST http://localhost:8000/api/v1/metric-context \\\n  -H \"Content-Type: application/json\" \\\n  -d '{\n    \"metric_name\": \"http_request_latency_p99\",\n    \"duration\": \"5m\"\n  }'\n```\n\n## Knowledge Base Setup\n\n### Directory Structure\n\n```\ndata/kb_articles/\n├── database/\n│   ├── connection-timeout.md\n│   ├── slow-queries.md\n│   └── pool-exhaustion.md\n├── network/\n│   ├── dns-resolution.md\n│   └── latency.md\n└── application/\n    ├── memory-leak.md\n    └── high-cpu.md\n```\n\n### Article Format\n\n```markdown\n# Database Connection Timeout\n\n**Tags**: database, connection, timeout\n**Category**: Application Issues\n**Severity**: High\n\n## Symptoms\n- Connection pool exhaustion\n- \"Connection refused\" errors\n\n## Root Causes\n1. Connection leak in application\n2. Database unavailable\n\n## Resolution Steps\n1. Check database connectivity\n2. Review application logs\n3. Increase connection pool size\n```\n\n### Auto-Load KB\n\nSet `AUTO_LOAD_KB=true` in `.env` to automatically load articles on startup.\n\n## Kubernetes Deployment\n\n### Prerequisites\n\n```bash\nkubectl create namespace rca-agent\n```\n\n### Deploy with kubectl\n\n```bash\nkubectl apply -f k8s/namespace.yaml\nkubectl apply -f k8s/deployment.yaml\nkubectl apply -f k8s/service.yaml\n```\n\n### Deploy with Helm\n\n```bash\nhelm install rca-agent ./helm/rca-agent \\\n  --namespace rca-agent \\\n  --create-namespace\n```\n\n## AWS EKS Deployment (Optional)\n\n```bash\n# Create EKS cluster\neksctl create cluster --name rca-agent --region us-east-1\n\n# Enable AWS services in .env\nexport AWS_BEDROCK_ENABLED=true\nexport AWS_OPENSEARCH_ENABLED=true\n\n# Deploy with Helm\nhelm install rca-agent ./helm/rca-agent \\\n  --values helm/values-aws.yaml\n```\n\n## Monitoring & Observability\n\n### Prometheus Metrics\n\n- `rca_agent_queries_total` - Total queries processed\n- `rca_agent_query_duration_seconds` - Query processing time\n- `rca_agent_kb_retrieval_duration_seconds` - KB retrieval latency\n- `rca_agent_llm_token_usage` - LLM token consumption\n- `rca_agent_vector_db_latency_seconds` - Vector DB latency\n\n### Grafana Dashboards\n\nAccess Grafana at `http://localhost:3000` (admin/admin)\n\nDashboards available:\n- RCA Agent Overview\n- Vector Database Performance\n- LLM Token Usage\n- Query Analytics\n\n### Logging\n\nAll logs available in `./logs/` directory:\n- `rca_agent.log` - Main application logs\n- `vector_db.log` - Vector database operations\n- `llm_calls.log` - LLM API calls\n- `kubernetes.log` - Kubernetes integration logs\n\n## Advanced Configuration\n\n### Custom LangChain Chains\n\nExtend functionality in `src/chains/`:\n- Multi-step reasoning chain\n- Correlation analysis chain\n- Recommendation generation chain\n\n### Vector Embedding Strategy\n\n```python\nfrom src.services import VectorDatabaseFactory\n\nconfig = Config.from_env()\nvector_db = VectorDatabaseFactory.create(\n    db_type=config.VECTOR_DB_TYPE,\n    config=config\n)\n\n# Add embeddings\nawait vector_db.add_embeddings(\n    embeddings=embeddings_array,\n    metadata=metadata_list\n)\n\n# Search\nresults = await vector_db.search(query_embedding, top_k=10)\n```\n\n### Fine-Tuning\n\n```bash\npython scripts/fine_tune.py \\\n  --training_data ./data/training_examples.jsonl \\\n  --model gpt-4 \\\n  --epochs 3\n```\n\n## Troubleshooting\n\n### Vector DB Connection Issues\n\n```bash\n# Check Weaviate\ncurl http://localhost:8080/v1/.well-known/ready\n\n# Check PostgreSQL\npsql -h localhost -U postgres -d rca_kb\n\n# Check OpenSearch\ncurl -u admin:admin http://localhost:9200\n```\n\n### Low KB Relevance\n\n```bash\n# Rebuild index\npython scripts/rebuild_index.py --vector-db faiss\n\n# Re-ingest KB articles\npython scripts/ingest_kb.py --source ./data/kb_articles\n```\n\n### High LLM Costs\n\n```bash\n# Enable caching\nexport LANGCHAIN_CACHE=redis\nexport REDIS_URL=redis://localhost:6379\n```\n\n## Project Structure\n\n```\nRCA_Agent_AI_Enterprise/\n├── README.md                 # This file\n├── requirements.txt          # Python dependencies\n├── .env.example              # Configuration template\n├── main.py                   # FastAPI entry point\n├── Dockerfile                # Container image\n├── docker-compose.yml        # Local development stack\n├── src/\n│   ├── __init__.py\n│   ├── config.py             # Configuration management\n│   └── api/\n│       └── routes/           # API endpoints\n├── docs/\n│   ├── ARCHITECTURE.md       # System design\n│   ├── DEPLOYMENT.md         # Deployment guides\n│   ├── KB_INGESTION.md       # KB setup\n│   └── API.md                # API reference\n├── k8s/                      # Kubernetes manifests\n│   ├── namespace.yaml\n│   ├── deployment.yaml\n│   └── service.yaml\n├── helm/                     # Helm charts\n│   ├── Chart.yaml\n│   └── values.yaml\n├── monitoring/               # Prometheus & Grafana\n│   ├── prometheus.yml\n│   └── grafana/\n├── scripts/                  # Utility scripts\n│   ├── ingest_kb.py\n│   ├── collect_logs.py\n│   └── query_prometheus.py\n├── data/                     # Data directory\n│   ├── kb_articles/\n│   └── faiss_index/\n└── logs/                     # Application logs\n```\n\n## Contributing\n\n1. Fork the repository\n2. Create feature branch: `git checkout -b feature/enhancement`\n3. Commit changes: `git commit -am 'Add feature'`\n4. Push to branch: `git push origin feature/enhancement`\n5. Submit pull request\n\n## Cloud-Agnostic Philosophy\n\nThis solution is designed to work in any environment:\n\n- **No vendor lock-in**: All cloud services are optional\n- **Flexible architecture**: Swap components as needed\n- **Standard protocols**: Use Kubernetes, Prometheus, OpenAPI standards\n- **Multiple backends**: Choose vector DB, search engine, LLM provider\n\n## Support & Documentation\n\n- **GitHub Issues**: https://github.com/payamdsp/RCA_Agent_AI_Enterprise/issues\n- **Documentation**: See `/docs` folder\n- **API Docs**: http://localhost:8000/docs (Swagger UI)\n- **ReDoc**: http://localhost:8000/redoc\n\n## License\n\nMIT License - See LICENSE file for details\n\n## Roadmap\n\n- [ ] Multi-language support\n- [ ] GraphQL API\n- [ ] Mobile app integration\n- [ ] Advanced anomaly detection (Prophet, Isolation Forest)\n- [ ] Federated learning support\n- [ ] Custom LLM fine-tuning pipeline\n- [ ] Slack/Teams integration\n- [ ] PagerDuty/Incident.io integration\n\n## Authors\n\n**Payam DSP** - Principal Architect\n\n---\n\n**Last Updated**: January 2024\n**Version**: 1.0.0\n**Status**: Active Development\n"