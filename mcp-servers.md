# 🔌 Claude MCP Servers — The Complete Reference

> **Every MCP server worth knowing in 2026.**  
> Tier 1 core infrastructure → Tier 2 enterprise tools → Tier 3 extended workflows → Memory systems.  
> Vetted from 1,200+ servers in the ecosystem. Updated April 2026.

---

## 📋 Table of Contents

- [What is MCP?](#what-is-mcp)
- [How to Install MCP Servers](#how-to-install-mcp-servers)
- [🔴 Tier 1 — Core Infrastructure](#-tier-1--core-infrastructure-must-have)
- [🟠 Tier 2 — High Value Enterprise Tools](#-tier-2--high-value-enterprise-tools)
- [🟡 Tier 3 — Extended Workflow Automation](#-tier-3--extended-workflow-automation)
- [🧠 Memory Systems — Persistent Context](#-memory-systems--persistent-context)
- [📊 Quick Reference Table](#-quick-reference-table)
- [🔗 Related Resources](#-related-resources)

---

## What is MCP?

**Model Context Protocol (MCP)** is the open standard that gives Claude hands.

```
Without MCP → Claude is a brain in a box. Can think. Can't act.
With MCP    → Claude pushes code, queries databases, browses the web, deploys apps.
```

Released by Anthropic in November 2024. Adopted by OpenAI and Google DeepMind in early 2025. Donated to the Linux Foundation's Agentic AI Foundation in December 2025.

**It is now the universal interface between AI and the real world.**

| Primitive | What It Is |
|-----------|-----------|
| **Resources** | Data Claude can read (files, DB records, docs) |
| **Tools** | Actions Claude can take (create PR, run query, send message) |
| **Prompts** | Reusable templates for specific workflows |

**Key advantage:** MCP is client-agnostic. One server works with Claude Code, Claude Desktop, Cursor, Windsurf, VS Code, Cline — all of them. Build once, use everywhere.

---

## How to Install MCP Servers

### Method 1: Claude Code CLI (Recommended — 2026 Standard)

```bash
# HTTP transport (for cloud-based servers)
claude mcp add --transport http <name> <url>

# With auth header
claude mcp add --transport http github https://api.github.com/mcp \
  --header "Authorization: Bearer $GITHUB_TOKEN"

# Stdio transport (for local/npm packages)
claude mcp add --transport stdio <name> -- npx -y <package>

# With environment variables
claude mcp add --transport stdio postgres \
  --env DATABASE_URL=$DATABASE_URL \
  -- npx -y @modelcontextprotocol/server-postgres

# Manage servers
claude mcp list
claude mcp get <server-name>
claude mcp remove <server-name>
```

### Method 2: Config File (Claude Desktop / All Clients)

Edit your config file:
- **Mac:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`
- **Claude Code:** `.claude/settings.json`

```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "@package/name"],
      "env": {
        "API_KEY": "your_key_here"
      }
    }
  }
}
```

Restart Claude after editing. Changes take effect immediately in Claude Code via CLI.

> **Security rule:** Always use official or provider-maintained servers over unreviewed community forks. Watch for prompt injection — MCP servers that return web content can be vectors for injected instructions.

---

## 🔴 Tier 1 — Core Infrastructure (Must-Have)

> Install these first. These form the foundation of every agentic workflow.

---

### 1️⃣ Filesystem MCP Server
**Category:** Local File Access · **Priority:** ⭐ MUST-HAVE  
**GitHub:** [modelcontextprotocol/servers/filesystem](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem)

The most fundamental server. Gives Claude secure, sandboxed read/write access to your local files and directories. Everything else builds on this.

**Install:**
```bash
npx -y @modelcontextprotocol/server-filesystem /path/to/your/project
```
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/Users/you/projects"]
    }
  }
}
```
> ⚠️ Be explicit with paths. Only grant access to directories you want Claude to see.

---

### 2️⃣ GitHub MCP Server
**Category:** Version Control · **Priority:** ⭐ MUST-HAVE  
**GitHub:** [modelcontextprotocol/servers/github](https://github.com/modelcontextprotocol/servers/tree/main/src/github)  
**Official:** Yes (GitHub)

Full GitHub integration from inside Claude. Read repos, manage issues, create pull requests — all via natural language.

**What Claude can do:**
- Search repos and read code cross-repository
- Open, comment on, and close issues
- Create and review pull requests
- Manage branches and releases
- `"Analyze src/ directory and suggest fix for issue #42"` → Claude reads issue → finds root cause → creates PR

**Install:**
```bash
npx -y @modelcontextprotocol/server-github
```
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_PERSONAL_ACCESS_TOKEN": "your_token" }
    }
  }
}
```

---

### 3️⃣ PostgreSQL MCP Server
**Category:** Database · **Priority:** ⭐ CORE  
**GitHub:** [modelcontextprotocol/servers/postgres](https://github.com/modelcontextprotocol/servers/tree/main/src/postgres)

Natural language queries to your PostgreSQL database. Claude inspects your schema automatically — no manual table descriptions needed.

**Real workflow:**
```
"Show me the top 5 users by spend last month"
→ Claude inspects schema → generates SQL → executes → returns results
```

**Install:**
```bash
npx -y @modelcontextprotocol/server-postgres postgresql://user:pass@localhost/dbname
```
> 🔒 Read-only by default. Prevents accidental data modifications.

---

### 4️⃣ SQLite MCP Server
**Category:** Database · **Priority:** ⭐ CORE  
**GitHub:** [modelcontextprotocol/servers/sqlite](https://github.com/modelcontextprotocol/servers/tree/main/src/sqlite)

Same natural language database access for SQLite. Essential for local dev, prototyping, and embedded apps.

**Install:**
```bash
npx -y @modelcontextprotocol/server-sqlite /path/to/database.db
```

---

### 5️⃣ Slack MCP Server
**Category:** Team Communication · **Priority:** ⭐ CORE  
**GitHub:** [modelcontextprotocol/servers/slack](https://github.com/modelcontextprotocol/servers/tree/main/src/slack)  
**Official:** Yes (Slack)

Read Slack channels, summarize threads, post messages. January 2026 update added interactive drafting — compose in Claude, preview, then approve before posting.

**Power use case:**
```
"Summarize #engineering channel from the last 2 days before I respond"
→ Full context without reading 500 messages
```

**Install:**
```bash
npx -y @modelcontextprotocol/server-slack
```

---

### 6️⃣ Notion MCP Server
**Category:** Knowledge Base · **Priority:** ⭐ CORE  
**Remote URL:** `https://mcp.notion.com/mcp`  
**Official:** Yes (Notion)

Search wikis, read docs, append to Notion databases. Turns your entire knowledge base into Claude's active context.

**Install:**
```json
{
  "mcpServers": {
    "notion": {
      "type": "http",
      "url": "https://mcp.notion.com/mcp"
    }
  }
}
```

---

### 7️⃣ Google Drive MCP Server
**Category:** File Storage · **Priority:** ⭐ CORE  
**Remote URL:** `https://drivemcp.googleapis.com/mcp/v1`  
**Official:** Yes (Google)

Access, search, and read Google Drive files. Best combined with Gmail and Calendar for full Google Workspace coverage.

**Install:**
```json
{
  "mcpServers": {
    "google-drive": {
      "type": "http",
      "url": "https://drivemcp.googleapis.com/mcp/v1"
    }
  }
}
```

---

### 8️⃣ Brave Search MCP Server
**Category:** Web Search · **Priority:** ⭐ CORE  
**GitHub:** [modelcontextprotocol/servers/brave-search](https://github.com/modelcontextprotocol/servers/tree/main/src/brave-search)

Connects Claude to the live internet. Overcomes knowledge cutoff. Real-time news, current library docs, live technical references — all pulled directly into context.

**Install:**
```bash
npx -y @modelcontextprotocol/server-brave-search
```
```json
{
  "mcpServers": {
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-brave-search"],
      "env": { "BRAVE_API_KEY": "your_key" }
    }
  }
}
```

---

### 9️⃣ Puppeteer MCP Server
**Category:** Browser Automation · **Priority:** ⭐ CORE  
**GitHub:** [modelcontextprotocol/servers/puppeteer](https://github.com/modelcontextprotocol/servers/tree/main/src/puppeteer)

Full browser control via Puppeteer. Navigate pages, take screenshots, fill forms, scrape data, automate web workflows.

**Install:**
```bash
npx -y @modelcontextprotocol/server-puppeteer
```

---

### 🔟 Fetch MCP Server
**Category:** Web / API · **Priority:** ⭐ CORE  
**GitHub:** [modelcontextprotocol/servers/fetch](https://github.com/modelcontextprotocol/servers/tree/main/src/fetch)

Fetch any URL or external API. Pull live documentation, API responses, or web content directly into Claude's context window.

**Install:**
```bash
npx -y @modelcontextprotocol/server-fetch
```

---

## 🟠 Tier 2 — High Value Enterprise Tools

> These add serious depth. Connect Claude to your full engineering and design stack.

---

### 1️⃣1️⃣ Kubernetes MCP Server
**Category:** DevOps · **Priority:** ⭐ ADVANCED  
**GitHub:** [strowk/mcp-k8s-go](https://github.com/strowk/mcp-k8s-go) · [rohitg00/kubectl-mcp-server](https://github.com/rohitg00/kubectl-mcp-server) (870 ⭐)

View pods, read cluster logs, analyze deployment states, troubleshoot K8s clusters via natural language. Two strong community implementations available.

**Install:**
```bash
# Go implementation (recommended)
npx -y @modelcontextprotocol/server-kubernetes
```

---

### 1️⃣2️⃣ Redis MCP Server
**Category:** Database / Cache · **Priority:** ⭐ ADVANCED

Natural language cache operations. Read/write Redis keys, inspect TTLs, flush caches, analyze memory usage.

**Install:**
```bash
npx -y @modelcontextprotocol/server-redis
```

---

### 1️⃣3️⃣ Supabase MCP Server
**Category:** Backend as a Service · **Priority:** ⭐ ADVANCED  
**GitHub:** [supabase-community/supabase-mcp](https://github.com/supabase-community/supabase-mcp)

Manage Supabase projects, query databases, inspect Edge Functions, handle migrations — all from Claude.

**Install:**
```bash
npx -y @supabase/mcp-server-supabase
```

---

### 1️⃣4️⃣ MongoDB MCP Server
**Category:** Database · **Priority:** ⭐ ADVANCED

Query MongoDB collections via natural language. Inspect schemas, run aggregations, analyze documents.

**Install:**
```bash
npx -y @modelcontextprotocol/server-mongodb
```

---

### 1️⃣5️⃣ Stripe MCP Server
**Category:** Payments · **Priority:** ⭐ ADVANCED  
**Official:** Yes (Stripe)

Payment automation directly from Claude. Query customers, inspect charges, manage subscriptions, analyze revenue — read-only by default.

**Install:**
```bash
npx -y @stripe/mcp-server-stripe
```

---

### 1️⃣6️⃣ Figma MCP Server
**Category:** Design · **Priority:** ⭐ ADVANCED  
**Remote URL:** `https://mcp.figma.com/mcp`  
**Official:** Yes (Figma)

Read Figma files, extract design tokens, generate code from layouts. Bridges design and frontend development. Select a Figma frame, paste the link → get working code that matches the actual layout, spacing, and component hierarchy.

**Two modes:**
- Remote: `https://mcp.figma.com/mcp` (no install)
- Local: Desktop MCP server via Figma app

**Install:**
```json
{
  "mcpServers": {
    "figma": {
      "type": "http",
      "url": "https://mcp.figma.com/mcp"
    }
  }
}
```

---

### 1️⃣7️⃣ Jira MCP Server
**Category:** Project Management · **Priority:** ⭐ ADVANCED  
**Official:** Yes (Atlassian)  
**Remote URL:** `https://mcp.atlassian.com/v1/mcp`

Manage tickets, search issues, create tasks, update sprint boards — for teams running Jira.

**Install:**
```json
{
  "mcpServers": {
    "atlassian": {
      "type": "http",
      "url": "https://mcp.atlassian.com/v1/mcp"
    }
  }
}
```

---

### 1️⃣8️⃣ Linear MCP Server
**Category:** Project Tracking · **Priority:** ⭐ ADVANCED

Manage Linear issues, update project status, create tasks. The developer-favorite for issue tracking.

**Install:**
```bash
npx -y @linear/mcp-server
```

---

### 1️⃣9️⃣ Vercel MCP Server
**Category:** Deployment · **Priority:** ⭐ ADVANCED  
**GitHub:** [vercel/mcp-adapter](https://github.com/vercel/mcp-adapter)

Manage Vercel deployments, check build logs, inspect project statuses, trigger redeploys — without leaving Claude.

**Install:**
```bash
npx -y @vercel/mcp-adapter
```

---

### 2️⃣0️⃣ AWS MCP Server
**Category:** Cloud Infrastructure · **Priority:** ⭐ ADVANCED  
**Official:** Yes (AWS)

Access AWS resources, inspect EC2 instances, query S3, manage IAM policies, read CloudWatch logs. Enterprise-grade cloud control from Claude.

**Install:**
```bash
npx -y @aws/mcp-server
```

---

## 🟡 Tier 3 — Extended Workflow Automation

> Specialized servers that complete your full automation stack.

---

### 2️⃣1️⃣ Gmail MCP Server
**Category:** Email · **Priority:** ⭐ EXTENDED  
**Official:** Yes (Google)  
**Remote URL:** `https://gmailmcp.googleapis.com/mcp/v1`

Read, search, draft, and send emails. Combine with Google Calendar for a complete productivity workflow.

**Install:**
```json
{
  "mcpServers": {
    "gmail": {
      "type": "http",
      "url": "https://gmailmcp.googleapis.com/mcp/v1"
    }
  }
}
```

---

### 2️⃣2️⃣ Google Calendar MCP Server
**Category:** Scheduling · **Priority:** ⭐ EXTENDED  
**Official:** Yes (Google)  
**Remote URL:** `https://calendarmcp.googleapis.com/mcp/v1`

Manage calendar events, check availability, schedule meetings, create recurring events.

**Install:**
```json
{
  "mcpServers": {
    "google-calendar": {
      "type": "http",
      "url": "https://calendarmcp.googleapis.com/mcp/v1"
    }
  }
}
```

---

### 2️⃣3️⃣ Discord MCP Server
**Category:** Communication · **Priority:** ⭐ EXTENDED

Manage Discord bots, read channels, post messages, moderate servers. For teams running Discord communities.

**Install:**
```bash
npx -y @modelcontextprotocol/server-discord
```

---

### 2️⃣4️⃣ GitLab MCP Server
**Category:** Version Control · **Priority:** ⭐ EXTENDED  
**GitHub:** [modelcontextprotocol/servers/gitlab](https://github.com/modelcontextprotocol/servers/tree/main/src/gitlab)

Full GitLab integration. Same capabilities as GitHub MCP but for GitLab-hosted repos.

**Install:**
```bash
npx -y @modelcontextprotocol/server-gitlab
```

---

### 2️⃣5️⃣ Docker MCP Server
**Category:** Containers · **Priority:** ⭐ EXTENDED  
**Official:** Yes (Docker)

Manage Docker containers, inspect images, read logs, control compose stacks from Claude.

**Install:**
```bash
npx -y @docker/mcp-server
```

---

### Honorable Mentions

| Server | Category | Use Case |
|--------|---------|---------|
| **Playwright MCP** (Microsoft) | Browser Testing | Full E2E test automation via accessibility snapshots |
| **Context7** (Upstash) | Documentation | Live version-specific library docs |
| **Cloudflare MCP** | Edge / Cloud | Workers, KV, R2, D1 access |
| **HubSpot MCP** | CRM | Manage contacts, deals, pipelines |
| **Sentry MCP** | Error Tracking | Read errors, triage issues |
| **Exa Search MCP** | Search | AI-native semantic web search |
| **ArXiv MCP** | Research | Search academic papers |
| **Google Maps MCP** | Location | Places, directions, geocoding |
| **Airtable MCP** | Data | Manage Airtable bases and records |
| **Twilio MCP** | Messaging | Send SMS and WhatsApp messages |

---

## 🧠 Memory Systems — Persistent Context

> LLMs forget between sessions. These fix that.

The problem: Every new Claude session starts blank. No memory of your project conventions, past decisions, or coding standards.  
The solution: Memory MCP servers persist context across sessions so Claude remembers what matters.

---

### Tier 1 — Core Memory Systems

---

#### 1️⃣ Memory MCP (Official)
**Category:** Knowledge Graph Memory · **Priority:** ⭐ MUST-HAVE  
**GitHub:** [modelcontextprotocol/servers/memory](https://github.com/modelcontextprotocol/servers/tree/main/src/memory)

The official Anthropic memory server. Builds a knowledge graph of entities and relationships. Claude can store and retrieve structured facts across sessions.

**Install:**
```bash
npx -y @modelcontextprotocol/server-memory
```

---

#### 2️⃣ mem0 MCP (Self-Hosted)
**Category:** Vector Memory · **Priority:** ⭐ CORE  
**GitHub:** [elvismdev/mem0-mcp-selfhosted](https://github.com/elvismdev/mem0-mcp-selfhosted)

Persistent memory using vector databases (Qdrant + Ollama). Exposes `add_memory`, `search_memories`, `update_memory`. The highest-scoring AI memory system benchmarked as of 2026.

**Key tools:**
```
add_memory      → Store new information
search_memories → Semantic search across stored context  
update_memory   → Modify existing memories
delete_memory   → Remove outdated context
```

---

#### 3️⃣ OmniMem
**Category:** Multi-Layer Memory · **Priority:** ⭐ CORE  
**Site:** [omnimem.org](https://omnimem.org)

Separates memory into three layers: episodic (what happened), knowledge (what you know), and project state (what's in progress). Auto-cleanup and duplicate detection keep context lean across sessions and machines.

---

#### 4️⃣ Memora
**Category:** Local Semantic Memory · **Priority:** ⭐ CORE  
**GitHub:** [agentic-mcp-tools/memora](https://github.com/agentic-mcp-tools/memora)

Fully local, SQLite-backed memory server. Semantic search + live knowledge graph. No cloud dependencies — everything stays on your machine.

---

#### 5️⃣ mem-agent-mcp
**Category:** Obsidian-Style Memory · **Priority:** ⭐ CORE  
**GitHub:** [firstbatchxyz/mem-agent-mcp](https://github.com/firstbatchxyz/mem-agent-mcp)

Obsidian-like knowledge system for Claude. Connects to structured memory storage built for long-term, knowledge-heavy project navigation.

---

### Tier 2 — Advanced Memory Systems

---

#### 6️⃣ Local Memory MCP
**Category:** ChatGPT-Style Memory · **Priority:** ⭐ ADVANCED  
**GitHub:** [cunicopia-dev/local-memory-mcp](https://github.com/cunicopia-dev/local-memory-mcp)

ChatGPT-style persistent memory but fully local. Multi-database support and semantic search. Nothing leaves your machine.

---

#### 7️⃣ memcite
**Category:** Citation-Backed Memory · **Priority:** ⭐ ADVANCED  
**PyPI:** [pypi.org/project/memcite](https://pypi.org/project/memcite)

Python-based local memory server. Citation-backed retrieval — Claude knows *where* it learned something, not just *what* it learned. `memory_validate` command included.

---

#### 8️⃣ ctxvault
**Category:** Vault-Based Memory · **Priority:** ⭐ ADVANCED  
**GitHub:** [Filippo-Venturini/ctxvault](https://github.com/Filippo-Venturini/ctxvault)

Stores knowledge in semantic "vaults" using local vector databases. Trending for developers building personal knowledge bases.

---

### Tier 3 — Specialized Memory

| Tool | Use Case | Link |
|------|---------|------|
| **mem-search** | Recover previous dev decisions, load recent coding context | [zilliztech/memsearch](https://github.com/zilliztech/memsearch) |
| **Home Memory MCP** | Structured asset tracking and technical inventory | [impactjo/home-memory](https://github.com/impactjo/home-memory) |
| **Codebase-Memory** | Converts codebases to semantic graphs for massive project reasoning | [arxiv.org/abs/2603.27277](https://arxiv.org/abs/2603.27277) |

---

### Memory Setup Pattern

The standard setup for most developers:

```json
{
  "mcpServers": {
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    }
  }
}
```

Then tell Claude at session start:
```
Load my project context from memory. 
My project: [project name]
Retrieve: architecture decisions, coding standards, current task state
```

---

## 📊 Quick Reference Table

| # | Server | Category | Transport | Priority |
|---|--------|---------|-----------|---------|
| 1 | Filesystem | File Access | npx / stdio | ⭐ MUST-HAVE |
| 2 | GitHub | Version Control | npx / http | ⭐ MUST-HAVE |
| 3 | PostgreSQL | Database | npx / docker | ⭐ CORE |
| 4 | SQLite | Database | npx | ⭐ CORE |
| 5 | Slack | Communication | npx / docker | ⭐ CORE |
| 6 | Notion | Knowledge Base | http (remote) | ⭐ CORE |
| 7 | Google Drive | File Storage | http (remote) | ⭐ CORE |
| 8 | Brave Search | Web Search | npx | ⭐ CORE |
| 9 | Puppeteer | Browser | npx / docker | ⭐ CORE |
| 10 | Fetch | Web / API | npx | ⭐ CORE |
| 11 | Kubernetes | DevOps | docker | ⭐ ADVANCED |
| 12 | Redis | Cache | docker | ⭐ ADVANCED |
| 13 | Supabase | BaaS | npx | ⭐ ADVANCED |
| 14 | MongoDB | Database | docker | ⭐ ADVANCED |
| 15 | Stripe | Payments | npx | ⭐ ADVANCED |
| 16 | Figma | Design | http (remote) | ⭐ ADVANCED |
| 17 | Jira | Project Mgmt | http (remote) | ⭐ ADVANCED |
| 18 | Linear | Project Tracking | npx | ⭐ ADVANCED |
| 19 | Vercel | Deployment | npx | ⭐ ADVANCED |
| 20 | AWS | Cloud | npx | ⭐ ADVANCED |
| 21 | Gmail | Email | http (remote) | ⭐ EXTENDED |
| 22 | Google Calendar | Scheduling | http (remote) | ⭐ EXTENDED |
| 23 | Discord | Communication | npx | ⭐ EXTENDED |
| 24 | GitLab | Version Control | npx | ⭐ EXTENDED |
| 25 | Docker | Containers | npx | ⭐ EXTENDED |

---

### Minimal Starter Setup (Copy-Paste Ready)

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/projects"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_PERSONAL_ACCESS_TOKEN": "your_token" }
    },
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-brave-search"],
      "env": { "BRAVE_API_KEY": "your_key" }
    },
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    }
  }
}
```

> **Rule:** Don't install every server. Each adds context overhead. Start with 3–5. Add more as workflows demand them.

---

## 🔗 Related Resources in This Repo

- [`commands.md`](./commands.md) — Complete Claude command reference
- [`plugins.md`](./plugins.md) — Skills, plugins & extensions
- [`github-repos.md`](./github-repos.md) — Top GitHub repositories
- [`workflows.md`](./workflows.md) — Production workflow patterns
- [`agent-frameworks.md`](./agent-frameworks.md) — Agent orchestration

---

## Where to Find More MCP Servers

| Resource | What's There |
|----------|-------------|
| [modelcontextprotocol.io/examples](https://modelcontextprotocol.io/examples) | Official reference implementations |
| [glama.ai/mcp/servers](https://glama.ai/mcp/servers) | Searchable marketplace with previews |
| [mcp-awesome.com](https://mcp-awesome.com) | 1,200+ quality-verified servers |
| [github.com/punkpeye/awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers) | Community-maintained curated list |
| [github.com/jaw9c/awesome-remote-mcp-servers](https://github.com/jaw9c/awesome-remote-mcp-servers) | Remote/cloud MCP servers only |
| Docker MCP Catalog | Containerized servers with built-in isolation |

---

*Part of the **Ultimate Claude Bundle** — the most complete Claude resource on GitHub.*  
*Star the repo if this saved you time.*

---

**Last Updated:** April 2026 · MCP Ecosystem: 1,200+ servers · 25 servers documented
