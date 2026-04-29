<div align="center">

# 🧠 The Ultimate Claude Bundle

**The most complete Claude resource on GitHub.**

Commands · MCP Servers · Plugins · Tools · Workflows · Agent Frameworks

[![Files](https://img.shields.io/badge/Files-6%20References-blue?style=flat-square)](#-whats-inside)
[![Lines](https://img.shields.io/badge/Lines-4%2C700%2B-green?style=flat-square)](#-whats-inside)
[![Updated](https://img.shields.io/badge/Updated-April%202026-orange?style=flat-square)](#)
[![License](https://img.shields.io/badge/License-MIT-purple?style=flat-square)](#license)

</div>

---

## The Problem

You can spend 40 hours searching GitHub, Reddit, Medium, and Anthropic's docs —
and still not have a complete picture of what Claude can actually do.

The ecosystem exploded in 2025–2026.

- **14,600+** repositories indexed
- **1,200+** MCP servers available
- **400+** plugins and skills
- **25+** agent frameworks
- **50+** built-in commands

Nobody documented all of it in one place.

**Until now.**

---

## 📦 What's Inside

6 reference files. 4,700+ lines. Everything verified and current as of April 2026.

| File | What It Covers | Lines | Size |
|------|---------------|-------|------|
| [`commands.md`](./commands.md) | Every slash command, CLI flag, keyboard shortcut, and prompt code | 653 | 24 KB |
| [`mcp-servers.md`](./mcp-servers.md) | 25 MCP servers + 10 memory systems, install configs, usage patterns | 781 | 24 KB |
| [`plugins.md`](./plugins.md) | 40+ plugins and skills across 6 tiers, install commands, role-based setups | 829 | 29 KB |
| [`tools.md`](./tools.md) | 25 tools — frameworks, vector DBs, deployment platforms, automation | 561 | 20 KB |
| [`workflows.md`](./workflows.md) | 25 production workflows from RAG pipelines to multi-agent orchestration | 1,178 | 33 KB |
| [`agent-frameworks.md`](./agent-frameworks.md) | 25 agent frameworks with benchmarks, code examples, decision guide | 732 | 27 KB |

**Total:** 4,734 lines · 157 KB of curated, production-verified reference material

---

## 🗺️ How to Navigate This Repo

**New to Claude Code?**
```
commands.md → mcp-servers.md → plugins.md
```
Learn what Claude can do natively → connect it to the world → extend with skills.

**Building a production app?**
```
tools.md → workflows.md → agent-frameworks.md
```
Pick your stack → follow the workflow pattern → choose the right orchestration.

**Looking for something specific?**

| I want to... | Go to |
|-------------|-------|
| Learn all Claude commands | [`commands.md`](./commands.md) |
| Connect Claude to GitHub / Postgres / Slack | [`mcp-servers.md`](./mcp-servers.md) |
| Give Claude persistent memory | [`mcp-servers.md#memory-systems`](./mcp-servers.md#-memory-systems--persistent-context) |
| Install the best plugins | [`plugins.md`](./plugins.md) |
| Build a RAG pipeline | [`workflows.md`](./workflows.md) |
| Compare CrewAI vs LangGraph | [`agent-frameworks.md`](./agent-frameworks.md) |
| Pick a vector database | [`tools.md`](./tools.md) |
| Deploy Claude to production | [`tools.md`](./tools.md) + [`workflows.md`](./workflows.md) |

---

## ⚡ Quick Wins — 5 Things to Do Right Now

### 1. Install the 3 plugins that matter most
```bash
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace      # Plans before it builds
/plugin install context7@claude-plugins-official         # Live docs, no hallucinated APIs
claude skills add juliusbrussee/caveman                  # 65-75% fewer output tokens
```

### 2. Set up your core MCP servers
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
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    }
  }
}
```

### 3. Initialize project memory
```bash
/init           # Creates CLAUDE.md — Claude reads this every session
/memory         # Edit it with your stack, conventions, and rules
```

### 4. Learn the 5 keyboard shortcuts that save the most time
```
Shift+Tab    → Cycle modes: Normal → Auto-Accept → Plan Mode
Ctrl+R       → Search command history
Esc Esc      → Rewind conversation or code
Ctrl+T       → Toggle task list
/compact     → Compress context when it hits 70%
```

### 5. Use the right thinking mode for the right task
```
L99          → Maximum reasoning depth (architecture decisions)
XRAY         → Root cause analysis (debugging)
OODA         → Tactical decisions under uncertainty
BEASTMODE    → Maximum effort, no shortcuts
/plan        → Force planning before execution (production code)
```

---

## 📊 The Ecosystem at a Glance

### MCP Servers — By Category

| Category | Must-Have Servers |
|---------|------------------|
| **Version Control** | GitHub, Git, GitLab |
| **Database** | PostgreSQL, SQLite, Redis, Supabase, MongoDB |
| **Communication** | Slack, Notion, Gmail, Discord |
| **Design** | Figma |
| **Deployment** | Vercel, Docker, AWS, Kubernetes |
| **Search** | Brave Search, Fetch, Puppeteer |
| **Memory** | Memory MCP, mem0, Memora, OmniMem |

### Plugins — By Role

| Role | Essential Installs |
|-----|-------------------|
| **All developers** | Superpowers, Context7, Caveman |
| **Full-stack** | + feature-dev, frontend-design, Playwright |
| **Backend** | + backend-architect, test-writer-fixer, security-sweep |
| **Product managers** | + Taskmaster, backlog, BMAD Method |
| **Security** | + security-sweep, AgentLint, Trail of Bits |
| **Content/Marketing** | + connect-apps, marketing-skills |

### Agent Frameworks — Decision Matrix

| Need | Framework |
|------|----------|
| Fastest to demo | **CrewAI** |
| Production + state management | **LangGraph** |
| Research + code execution | **AutoGen** |
| Claude-only stack | **Claude Agent SDK** |
| Data-heavy agents | **LlamaIndex Agents** |
| Enterprise .NET/Azure | **Microsoft Agent Framework** |

### Tools — Stack by Use Case

| Use Case | Stack |
|---------|-------|
| Getting started | Chroma + LangChain + Streamlit |
| Production SaaS | Supabase + FastAPI + Pinecone + Vercel |
| Enterprise | LlamaIndex + LangGraph + Qdrant + Kubernetes |
| No-code | Flowise or Dify + Zapier or Make |
| Privacy-first | Ollama + Chroma + LlamaIndex (fully local) |

---

## 🏗️ Repo Structure

```
ultimate-claude-bundle/
│
├── README.md                  ← You are here
│
├── commands.md                ← Slash commands, CLI flags, shortcuts, prompt codes
├── mcp-servers.md             ← MCP servers (Tier 1–3) + memory systems
├── plugins.md                 ← Plugins, skills, extensions (Tier 1–6)
├── tools.md                   ← Frameworks, DBs, deployment, automation
├── workflows.md               ← 25 production workflows with steps + tools
└── agent-frameworks.md        ← 25 frameworks with benchmarks + code examples
```

---

## 🔢 Numbers That Matter

| Metric | Number |
|--------|--------|
| Commands documented | 80+ |
| MCP servers covered | 25 (from 1,200+ ecosystem) |
| Memory systems | 10 |
| Plugins and skills | 40+ |
| Tools documented | 25 |
| Workflows with full steps | 25 |
| Agent frameworks | 25 |
| Code examples included | 60+ |
| Total lines of reference | 4,700+ |
| Repos indexed as sources | 14,634 |

---

## 🧭 The Philosophy Behind This Bundle

Most Claude resources are either:

**Too shallow** — "Here are 10 cool things Claude can do" (no install commands, no real usage)

**Too scattered** — spread across 50 different GitHub repos, Medium articles, and Discord threads

**Too outdated** — written for Claude 2 or early MCP, missing everything from 2025–2026

This bundle is different. Every entry follows the same structure:
- What it does (one clear sentence)
- Why it matters for Claude specifically
- Working install command or code example
- Honest "best for" and limitations
- Updated to April 2026

**The rule we followed throughout:**

> If you can't explain what a tool does in one sentence and show working code in 10 lines, it doesn't belong here.

---

## 📈 Verified Production Metrics

Real companies. Real results. What's possible when you use this stack properly.

| Company | Workflow | Result |
|---------|---------|--------|
| **Stripe** | Claude Code across 1,370 engineers | 10k-line migration: 4 days vs 10 engineer-weeks |
| **Ramp** | Incident investigation workflow | 80% reduction in investigation time |
| **Wiz** | Multi-agent code migration | 50k lines Python→Go in ~20hrs (was 2–3 months) |
| **Rakuten** | Feature delivery pipeline | 24 working days → 5 working days |
| **Fountain** | Multi-agent code review | 50% faster delivery |
| **CRED** | Agent teams across dev lifecycle | 2x speed, maintained quality |

---

## 🗓️ Keeping This Current

Claude Code ships weekly. The ecosystem moves fast.

**How to know if something is outdated:**
- Run `/help` in Claude Code — always shows current command list
- Check `claude --version` before using CLI flags
- MCP server transport syntax changed in 2026 (use `--transport http` for cloud servers)

**Last full audit:** April 28, 2026
**Next planned update:** When Claude Code hits v3.0 or major ecosystem shift

---

## 🤝 Contributing

Found a command that works but isn't listed?
Know a plugin that deserves Tier 1?
Discovered a workflow pattern that ships faster?

**Open a PR with:**
- Name and one-line description
- Working install command or code example
- "Best for" and honest limitations
- Source (GitHub repo, official docs, your production experience)

We don't accept "it might be useful" entries. Show it working.

---

## 📚 Primary Sources

Everything in this bundle was verified against:

- Anthropic official documentation (code.claude.com/docs)
- Claude Code release notes (v2.1+)
- GitHub repository stars and activity (April 2026)
- Production reports: Gartner Q1 2026, Anthropic 2026 Agentic Coding Trends
- Community sources: awesome-claude-code, awesome-claude-plugins, quemsah/awesome-claude-plugins (14,634 repos)
- Benchmark data: GAIA benchmark, LangGraph production metrics, CrewAI execution data
- Direct testing and production deployments

---

## License

MIT — use, fork, share, build on top of this freely.
Attribution appreciated but not required.

---

<div align="center">

**If this saved you time, star the repo.**

Built for the Claude ecosystem. Updated by the community. April 2026.

[`commands.md`](./commands.md) · [`mcp-servers.md`](./mcp-servers.md) · [`plugins.md`](./plugins.md) · [`tools.md`](./tools.md) · [`workflows.md`](./workflows.md) · [`agent-frameworks.md`](./agent-frameworks.md)

</div>
