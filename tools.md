# 🛠️ Claude Tools — The Complete Reference

> **Every tool in the Claude ecosystem worth knowing in 2026.**
> Frameworks, vector databases, deployment platforms, and automation tools.
> Organized by tier and category. Updated April 2026.

---

## 📋 Table of Contents

- [🔴 Tier 1 — Core Infrastructure](#-tier-1--core-infrastructure)
- [🟠 Tier 2 — High Value Developer Tools](#-tier-2--high-value-developer-tools)
- [🟡 Tier 3 — Extended and Automation Tools](#-tier-3--extended-and-automation-tools)
- [🔵 Bonus — Agent Frameworks](#-bonus--agent-frameworks)
- [📊 Stack Selector](#-stack-selector)

---

## 🔴 Tier 1 — Core Infrastructure

> Foundation of every serious Claude project. Pick 3–5 to start.

---

### 1️⃣ LangChain
**Category:** LLM Orchestration · **Stars:** 95k+ · **License:** MIT
**Site:** langchain.com · **GitHub:** langchain-ai/langchain

The most widely adopted framework for building LLM apps. Connects Claude to data, tools, APIs through chains and agents. Every vector DB and document loader in its ecosystem works with Claude out of the box.

**Key components:**
- **LangChain Core** — chains, prompts, document loaders
- **LangGraph** — stateful multi-agent workflows
- **LangSmith** — debugging, tracing, evaluation
- **LangServe** — deploy chains as REST APIs

**Install:**
```bash
pip install langchain langchain-anthropic
```

```python
from langchain_anthropic import ChatAnthropic
llm = ChatAnthropic(model="claude-opus-4-7")
response = llm.invoke("Explain MCP in one sentence")
```

**Best for:** Teams needing wide ecosystem support. Default choice for most projects.

---

### 2️⃣ LlamaIndex
**Category:** Data Framework / RAG Specialist · **Stars:** 44k+ · **License:** MIT
**Site:** llamaindex.ai · **GitHub:** run-llama/llama_index

Purpose-built for RAG. When data retrieval is your core feature, LlamaIndex beats LangChain. Handles the full pipeline: 300+ data connectors → intelligent chunking → advanced retrieval.

**Key components:**
- **LlamaHub** — 300+ data source connectors
- **Query Engines** — hybrid search, re-ranking, recursive retrieval
- **LlamaParse** — high-accuracy PDF/document parsing
- **LlamaCloud** — managed indexing service

**Install:**
```bash
pip install llama-index llama-index-llms-anthropic
```

```python
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader
from llama_index.llms.anthropic import Anthropic

llm = Anthropic(model="claude-opus-4-7")
documents = SimpleDirectoryReader("./docs").load_data()
index = VectorStoreIndex.from_documents(documents)
query_engine = index.as_query_engine(llm=llm)
response = query_engine.query("What are the key findings?")
```

**Best for:** Document Q&A, knowledge base search, enterprise document intelligence. Combine with LangChain for orchestration.

---

### 3️⃣ Flowise
**Category:** Visual AI Workflow Builder · **Stars:** 35k+ · **License:** Apache 2.0
**Site:** flowiseai.com · **GitHub:** FlowiseAI/Flowise

Drag-and-drop interface for building LLM applications. No code needed for prototyping. Connect Claude, vector databases, tools, and document loaders visually.

**Install:**
```bash
npx flowise start
# or Docker
docker run -d -p 3000:3000 flowiseai/flowise
```

**Best for:** Rapid prototyping, non-technical builders, internal tools and demos.

---

### 4️⃣ Dify
**Category:** AI Application Platform · **Stars:** 65k+ · **License:** Apache 2.0
**Site:** dify.ai · **GitHub:** langgenius/dify

Production-ready AI app platform. Goes beyond Flowise with prompt versioning, A/B testing, user analytics, and team collaboration built in. Claude is a first-class model here.

**Install:**
```bash
git clone https://github.com/langgenius/dify.git
cd dify/docker && docker-compose up -d
```

**Best for:** Teams shipping production AI apps who need more than a prototype tool.

---

### 5️⃣ Supabase
**Category:** Backend-as-a-Service · **Stars:** 72k+ · **License:** Apache 2.0
**Site:** supabase.com · **Pricing:** Free tier → Pro $25/month

Open-source Firebase alternative. PostgreSQL + Auth + Storage + Edge Functions + pgvector in one place. The Supabase MCP server gives Claude direct access to your entire backend.

**pgvector for RAG (no separate vector DB needed):**
```sql
CREATE EXTENSION vector;
ALTER TABLE documents ADD COLUMN embedding vector(1536);

-- Semantic search
SELECT * FROM documents
ORDER BY embedding <-> '[query_embedding]'
LIMIT 5;
```

**Install MCP:**
```bash
npx -y @supabase/mcp-server-supabase
```

**Best for:** Full-stack Claude apps needing auth, storage, and database in one place.

---

### 6️⃣ Pinecone
**Category:** Vector Database (Cloud-Native)
**Site:** pinecone.io · **Pricing:** Free 2GB → Standard $70/month

The original managed vector database. Zero infrastructure — just API calls. Millisecond nearest-neighbor search at any scale.

```python
import pinecone
pc = pinecone.Pinecone(api_key="your_key")
index = pc.Index("claude-knowledge-base")
index.upsert(vectors=[{"id": "doc1", "values": embedding, "metadata": {"text": chunk}}])
results = index.query(vector=query_embedding, top_k=5, include_metadata=True)
```

**Best for:** Production RAG at scale (10k+ docs), teams who don't want to manage infrastructure.

---

### 7️⃣ Weaviate
**Category:** Vector Database (Semantic Search) · **Stars:** 12k+ · **License:** BSD
**Site:** weaviate.io · **Pricing:** Open source → Cloud $25/month

Open-source vector DB with native multi-modal support. Unique feature: built-in vectorization — no separate embedding pipeline. Best-in-class hybrid search (vector + keyword combined).

**Best for:** Hybrid search RAG, multi-modal data, teams preferring self-hosted.

---

### 8️⃣ Qdrant
**Category:** Vector Database (Performance) · **Stars:** 22k+ · **License:** Apache 2.0
**Site:** qdrant.tech · **Pricing:** Open source → Cloud available

Rust-based vector DB built for speed and production reliability. Powers the best Claude memory systems (mem0, mempalace). Go-to for persistent agent context.

```bash
docker run -p 6333:6333 qdrant/qdrant
```

```python
from qdrant_client import QdrantClient
client = QdrantClient("localhost", port=6333)
results = client.search(collection_name="memory", query_vector=embedding, limit=5)
```

**Best for:** Agent memory systems, high-performance RAG, self-hosted production.

---

### 9️⃣ Chroma
**Category:** Vector Database (Local/Dev) · **Stars:** 16k+ · **License:** Apache 2.0
**Site:** trychroma.com · **Pricing:** Free and open source

The easiest vector DB to get started with. Runs locally in Python — no Docker, no cloud, no setup friction. Zero-to-RAG in 5 minutes.

```bash
pip install chromadb
```

```python
import chromadb
client = chromadb.Client()
collection = client.create_collection("docs")
collection.add(documents=["your text"], ids=["doc1"])
results = collection.query(query_texts=["your question"], n_results=3)
```

**Best for:** Local development, prototyping, private/offline deployments. Graduate to Qdrant for production.

---

### 🔟 Redis
**Category:** In-Memory Cache · **Stars:** 67k+ · **License:** BSD
**Site:** redis.io · **Pricing:** Open source → Cloud free tier

Sub-millisecond reads. The fastest data store that exists. Essential for session management, prompt caching, and rate limiting in production Claude systems.

**Use cases:**
- Cache conversation history per user session
- Store rate limit counters per API key
- Queue background tasks (embeddings, ingestion)
- Pub/sub for real-time streaming

```bash
docker run -d -p 6379:6379 redis:alpine
pip install redis
```

**Best for:** Production multi-user deployments, cost optimization via prompt caching.

---

## 🟠 Tier 2 — High Value Developer Tools

---

### 1️⃣1️⃣ Neo4j
**Category:** Graph Database · **Site:** neo4j.com · **Pricing:** Community free → AuraDB free tier

The leading graph database. Stores entities and relationships. Enables queries like "What teams worked on systems that depend on this module?" — impossible in relational databases. Pairs with Memory MCP for advanced knowledge graphs.

**Best for:** Enterprise knowledge management, codebase intelligence, complex relationship modeling.

---

### 1️⃣2️⃣ Apache Airflow
**Category:** Workflow Orchestration · **Stars:** 38k+ · **License:** Apache 2.0
**Site:** airflow.apache.org

Industry standard for orchestrating data pipelines. Schedule, monitor, and retry multi-step workflows as DAGs (directed acyclic graphs) in Python. The orchestration layer for keeping RAG knowledge bases updated.

**Best for:** Production ETL pipelines feeding RAG systems, scheduled embedding generation.

---

### 1️⃣3️⃣ Docker
**Category:** Containerization · **Site:** docker.com · **Pricing:** Free personal use

Package your Claude application with all dependencies into a portable container. Runs identically everywhere. Every MCP server and every production LangChain app runs in Docker.

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0"]
```

**Best for:** Every production Claude deployment. Non-negotiable at scale.

---

### 1️⃣4️⃣ Kubernetes
**Category:** Container Orchestration · **Site:** kubernetes.io · **Pricing:** Open source

Manages fleets of containers — auto-scaling, self-healing, load balancing. When you have multiple Claude services at scale. Kubernetes MCP server gives Claude visibility into cluster health from natural language.

**Best for:** Enterprise AI infrastructure, high-traffic production deployments.

---

### 1️⃣5️⃣ Vercel
**Category:** Frontend Deployment · **Site:** vercel.com · **Pricing:** Free hobby → Pro $20/month

Push to GitHub → Vercel builds and deploys. Zero-config CI/CD, preview URLs for every PR, edge network distribution. Vercel MCP server lets Claude manage deployments from inside Claude Code.

**Install MCP:**
```bash
npx -y @vercel/mcp-adapter
```

**Best for:** Frontend Claude applications, Next.js apps, rapid zero-config deployment.

---

### 1️⃣6️⃣ FastAPI
**Category:** Python Backend Framework · **Stars:** 78k+ · **License:** MIT
**Site:** fastapi.tiangolo.com

Fastest Python framework for APIs. Async-native — critical for streaming Claude responses. Auto-generates OpenAPI docs.

**Claude streaming endpoint:**
```python
from fastapi import FastAPI
from fastapi.responses import StreamingResponse
import anthropic

app = FastAPI()
client = anthropic.Anthropic()

@app.post("/chat")
async def chat(prompt: str):
    def generate():
        with client.messages.stream(
            model="claude-opus-4-7", max_tokens=1024,
            messages=[{"role": "user", "content": prompt}]
        ) as stream:
            for text in stream.text_stream:
                yield text
    return StreamingResponse(generate(), media_type="text/plain")
```

**Best for:** Production Claude API services, microservices, streaming responses.

---

### 1️⃣7️⃣ Flask
**Category:** Python Web Framework · **Stars:** 68k+ · **License:** BSD

Minimal Python web framework. Easier to start than FastAPI but no async support. Add only what you need.

**Best for:** Simple Claude API wrappers, prototypes, internal low-traffic tools.

---

### 1️⃣8️⃣ Streamlit
**Category:** Python Dashboard Builder · **Stars:** 35k+ · **License:** Apache 2.0
**Site:** streamlit.io

Turn Python scripts into interactive web apps in minutes. No HTML or JavaScript needed.

**Claude chatbot in 10 lines:**
```python
import streamlit as st
import anthropic

client = anthropic.Anthropic()
st.title("Claude Assistant")
if prompt := st.chat_input("Message Claude"):
    with st.chat_message("assistant"):
        response = client.messages.create(
            model="claude-opus-4-7", max_tokens=1024,
            messages=[{"role": "user", "content": prompt}]
        )
        st.write(response.content[0].text)
```

**Best for:** Internal analytics tools, Claude demos, data exploration interfaces.

---

### 1️⃣9️⃣ Gradio
**Category:** ML Interface Builder · **Stars:** 34k+ · **License:** Apache 2.0
**Site:** gradio.app

Build shareable ML demos instantly. One Python function → shareable web interface. Auto-generates a public link.

**Best for:** Sharing Claude demos, testing multi-modal capabilities, quick team prototypes.

---

### 2️⃣0️⃣ Hugging Face
**Category:** ML Model Platform · **Site:** huggingface.co · **Pricing:** Free tier generous

GitHub for machine learning. 500k+ models, datasets, and hosted apps. The Transformers library runs models locally. Use Claude for generation and HuggingFace models for free local embeddings.

**Free local embeddings for RAG:**
```python
from sentence_transformers import SentenceTransformer
model = SentenceTransformer("all-MiniLM-L6-v2")  # Free, runs locally
embeddings = model.encode(["your document chunks here"])
```

**Best for:** Free embedding generation, open-source model experimentation.

---

## 🟡 Tier 3 — Extended and Automation Tools

---

### 2️⃣1️⃣ Ollama
**Category:** Local Model Runner · **Stars:** 90k+ · **License:** MIT
**Site:** ollama.com · **Pricing:** Free

Run LLMs entirely on your machine. No API keys. No internet. No cost per token. OpenAI-compatible API so it drops into any LangChain setup.

```bash
curl -fsSL https://ollama.com/install.sh | sh
ollama pull llama3.2
```

```python
from langchain_community.llms import Ollama
llm = Ollama(model="llama3.2")  # Free, local, private
```

**Best for:** Local development, privacy-sensitive workloads, reducing API costs for simple tasks.

---

### 2️⃣2️⃣ OpenRouter
**Category:** LLM API Gateway · **Site:** openrouter.ai · **Pricing:** Pay-per-token

One API key. 200+ models. Claude, GPT-4, Gemini, Llama, Mistral — unified OpenAI-compatible interface with automatic fallback if a provider goes down.

```python
# Route to cheaper model for simple tasks
model = "anthropic/claude-opus-4-7" if complex else "meta-llama/llama-3.1-8b-instruct"
```

**Best for:** Cost optimization, multi-model architectures, fallback reliability.

---

### 2️⃣3️⃣ Postman
**Category:** API Testing · **Site:** postman.com · **Pricing:** Free tier generous

Industry standard for testing and documenting APIs. When Claude API calls aren't working, Postman isolates the issue — inspect raw requests, test payloads, verify auth headers.

**Quick Claude API test:**
```
POST https://api.anthropic.com/v1/messages
Headers: x-api-key: {{key}} | anthropic-version: 2023-06-01
Body: {"model": "claude-opus-4-7", "max_tokens": 1024, "messages": [{"role": "user", "content": "Hello"}]}
```

**Best for:** API debugging, MCP server testing, team API documentation.

---

### 2️⃣4️⃣ Zapier
**Category:** No-Code Automation · **Site:** zapier.com · **Pricing:** Free 100 tasks/month → Starter $19.99/month

Connect 6,000+ web apps without code. Trigger-action automation with native Claude/Anthropic integration built in.

**Example workflows:**
- New email → Claude summarizes → adds to Notion
- Form submission → Claude drafts response → sends for review
- New Slack message → Claude categorizes → routes to right channel

**Best for:** Non-technical teams, rapid automation, connecting Claude to existing SaaS.

---

### 2️⃣5️⃣ Make (formerly Integromat)
**Category:** Visual Automation Platform · **Site:** make.com · **Pricing:** Free 1,000 ops/month → Core $9/month

More powerful than Zapier for complex automations. Multi-step workflows with branching logic, data transformation, loops, and error handling. 1,500+ app integrations.

**Best for:** Complex multi-step automations, marketing and operations workflows, teams needing conditional logic.

---

## 🔵 Bonus — Agent Frameworks

| Framework | What It Does | Best For | Stars |
|-----------|-------------|---------|-------|
| **CrewAI** | Teams of specialized agents with defined roles | Multi-agent task decomposition | 25k+ |
| **AutoGen** (Microsoft) | Conversational multi-agent system | Research agents, code gen teams | 35k+ |
| **LangGraph** | Stateful graph-based agent workflows | Complex decision trees, cyclic flows | Built into LangChain |
| **DSPy** | Auto-optimize prompts and pipelines | Teams wanting framework-tuned prompts | 20k+ |
| **Haystack** | Enterprise RAG with evaluation built in | Production RAG with A/B testing | 18k+ |
| **Claude-Flow** | Enterprise multi-agent orchestration | Large-scale Claude deployments | 11k+ |

---

## 📊 Stack Selector

**Just Getting Started:**
```
Chroma + LangChain + Streamlit + Claude API
= Working RAG app in a weekend
```

**Production SaaS:**
```
Supabase (backend + pgvector) + FastAPI + Pinecone + Docker + Vercel + Redis
= Production-ready Claude application
```

**Enterprise Scale:**
```
LlamaIndex (ingestion) + LangGraph (orchestration) + Qdrant (vectors) +
Kubernetes (infra) + Airflow (pipelines) + Neo4j (knowledge graph)
= Full enterprise AI stack
```

**Non-Technical / No-Code:**
```
Flowise or Dify (visual builder) + Zapier or Make (automation) + Supabase
= AI apps without writing code
```

**Privacy-First / Local:**
```
Ollama (local LLM) + Chroma (local vectors) + LlamaIndex + Flask
= Fully offline, zero cloud dependencies
```

---

## Quick Reference

| # | Tool | Category | Pricing | Priority |
|---|------|---------|---------|---------|
| 1 | LangChain | Orchestration | Open source | ⭐ CORE |
| 2 | LlamaIndex | RAG Framework | Open source | ⭐ CORE |
| 3 | Flowise | Visual Builder | Open source | ⭐ CORE |
| 4 | Dify | App Platform | Open source | ⭐ CORE |
| 5 | Supabase | Backend/DB | Free → $25/mo | ⭐ CORE |
| 6 | Pinecone | Vector DB | Free → $70/mo | ⭐ CORE |
| 7 | Weaviate | Vector DB | Open source | ⭐ CORE |
| 8 | Qdrant | Vector DB | Open source | ⭐ CORE |
| 9 | Chroma | Vector DB | Free | ⭐ CORE |
| 10 | Redis | Cache | Open source | ⭐ CORE |
| 11 | Neo4j | Graph DB | Free tier | ⭐ ADVANCED |
| 12 | Airflow | Pipelines | Open source | ⭐ ADVANCED |
| 13 | Docker | Containers | Free personal | ⭐ ADVANCED |
| 14 | Kubernetes | Orchestration | Open source | ⭐ ADVANCED |
| 15 | Vercel | Deployment | Free → $20/mo | ⭐ ADVANCED |
| 16 | FastAPI | Backend | Open source | ⭐ ADVANCED |
| 17 | Flask | Backend | Open source | ⭐ ADVANCED |
| 18 | Streamlit | Dashboard | Open source | ⭐ ADVANCED |
| 19 | Gradio | UI Builder | Open source | ⭐ ADVANCED |
| 20 | Hugging Face | ML Platform | Free tier | ⭐ ADVANCED |
| 21 | Ollama | Local Models | Free | ⭐ EXTENDED |
| 22 | OpenRouter | API Gateway | Pay per token | ⭐ EXTENDED |
| 23 | Postman | API Testing | Free tier | ⭐ EXTENDED |
| 24 | Zapier | No-Code Auto | Free → $19.99/mo | ⭐ EXTENDED |
| 25 | Make | Visual Auto | Free → $9/mo | ⭐ EXTENDED |

---

## Related Resources in This Repo

- [`commands.md`](./commands.md) — Complete Claude command reference
- [`mcp-servers.md`](./mcp-servers.md) — MCP servers and memory systems
- [`plugins.md`](./plugins.md) — Skills, plugins and extensions
- [`workflows.md`](./workflows.md) — Production workflow patterns
- [`agent-frameworks.md`](./agent-frameworks.md) — Agent orchestration

---

*Part of **Claude OS** — a complete operating layer for Claude.*
*Star the repo if this saved you time.*

**Last Updated:** April 2026 · 25 tools documented · All tiers covered
