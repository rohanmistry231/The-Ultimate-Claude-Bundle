# ⚙️ Claude Workflows — The Complete Reference

> **Every production workflow pattern worth knowing in 2026.**  
> From simple sequential pipelines to full multi-agent orchestration.  
> 25 documented workflows across 3 tiers + 5 core agentic patterns.  
> Updated April 2026.

---

## 📋 Table of Contents

- [The Workflow Mindset](#the-workflow-mindset)
- [🧠 The 5 Core Agentic Patterns](#-the-5-core-agentic-patterns)
- [🔴 Tier 1 — Core Workflows](#-tier-1--core-workflows-must-know)
- [🟠 Tier 2 — High Value Workflows](#-tier-2--high-value-workflows)
- [🟡 Tier 3 — Extended Workflows](#-tier-3--extended-workflows)
- [⚡ Power Workflow Combos](#-power-workflow-combos)
- [📐 Workflow Design Principles](#-workflow-design-principles)
- [📊 Quick Reference](#-quick-reference)

---

## The Workflow Mindset

Most developers use Claude like a smarter Stack Overflow: ask question, get answer, move on.

That's leaving 90% of the value on the table.

**Agents are just workflows with feedback loops.**

The real power of Claude isn't in individual responses — it's in chaining capabilities across multiple steps where each step builds on the last.

```
One LLM call:    Input → Output → Done
Workflow:        Input → Act → Verify → Iterate → Act → Verify → Done
Agentic:         Goal → Plan → Spawn agents → Parallel execution → 
                 Verify → Auto-fix → Merge → Ship
```

**What companies are doing with this:**
- Ramp cut incident investigation time by **80%**
- Rakuten reduced feature delivery from **24 days → 5 days**
- Wiz migrated a 50,000-line Python library to Go in **~20 hours** (estimated: 2–3 months manually)
- At Anthropic itself, the majority of code is now written by Claude Code

The patterns below are how that happens.

---

## 🧠 The 5 Core Agentic Patterns

> **Understand these first.** Every workflow in this document is built on one of these five patterns.

---

### Pattern 1: Sequential (The Foundation)
**When to use:** Tasks that must execute in strict order. Each step depends on the previous.

```
Input → Step A → Step B → Step C → Output
```

The canonical Claude Code version is the **Explore → Plan → Act** loop:

```bash
# Phase 1: Explore (read-only, build understanding)
"Read all files in src/auth/ and understand the current architecture"

# Phase 2: Plan (propose before touching anything)
Shift+Tab  # Enable plan mode
"Plan the migration from session auth to JWT tokens"

# Phase 3: Act (execute the approved plan)
Shift+Tab  # Switch to auto-accept
"Execute the plan"
```

**Real-world applications:** Code generation, document processing, data pipelines, deployment workflows.

---

### Pattern 2: Operator (Orchestrator + Specialists)
**When to use:** Complex tasks that need multiple specialist perspectives. One brain directing many hands.

```
Orchestrator Agent
    ├── Security Agent    → analyze vulnerabilities
    ├── Performance Agent → identify bottlenecks
    ├── Testing Agent     → generate test suite
    └── Docs Agent        → write documentation
```

**Claude Code implementation:**

```bash
# Define agents inline
claude --agents '{
  "security-reviewer": {
    "description": "Reviews code for OWASP Top 10 vulnerabilities",
    "model": "claude-opus-4-7"
  },
  "performance-analyst": {
    "description": "Identifies performance bottlenecks",
    "model": "claude-sonnet-4-6"
  }
}'
```

**Or via `.claude/agents/` directory:**

```markdown
---
name: security-reviewer
description: Reviews code for security vulnerabilities  
tools: Read, Glob, Grep
model: sonnet
---
Analyze code for SQL injection, XSS, exposed credentials, and insecure configs.
Return findings as CRITICAL / HIGH / MEDIUM / LOW.
```

**Real-world applications:** Code review, architecture review, full-stack feature building.

---

### Pattern 3: Split-and-Merge (True Parallelism)
**When to use:** Independent tasks that don't depend on each other. Maximum throughput.

```
Task
├── Agent 1 → worktree/feature-auth    (working in parallel)
├── Agent 2 → worktree/bugfix-payments (working in parallel)
└── Agent 3 → worktree/refactor-api   (working in parallel)
                        ↓
                   Merge Results
```

**Claude Code implementation:**

```bash
# Spawn parallel worktrees (each agent gets isolated git branch)
claude --worktree feature-auth
claude --worktree bugfix-payments
claude --worktree refactor-api

# Each creates .claude/worktrees/{name}/ with its own branch
# Worktrees auto-clean if no changes were made
```

**The key:** Each agent works in full isolation — no file conflicts, no stepping on each other.

**Real-world applications:** Large-scale refactors, parallel feature development, multi-file migrations.

---

### Pattern 4: Agent Teams (Built-in Multi-Agent Coordination)
**When to use:** Complex projects needing multiple Claude instances with built-in coordination. Introduced in v2.1.32 (Feb 2026).

```
Team Lead (Opus)
    ├── Claims Task #1 from shared task list
    ├── Assigns Task #2 to Agent B
    ├── Assigns Task #3 to Agent C
    └── Merges results via git coordination

Agent B → Executes Task #2 → commits → updates task list
Agent C → Executes Task #3 → commits → updates task list
```

**When Agent Teams win:** Complex refactoring, large-scale analysis, coordinated multi-file changes.  
**When Multi-Instance wins:** Independent features, prototype exploration, simple parallelization.

> ⚠️ Still experimental as of April 2026. Best on Opus 4.7. Token-intensive — be prepared to hit usage limits.

---

### Pattern 5: Headless (Full Automation)
**When to use:** Recurring tasks that should run without human involvement. Claude gets a goal, tools, and guardrails — then works autonomously.

```bash
# Nightly dependency audit (CI/CD)
claude --print "Scan for dependency updates. Open PRs for safe updates. 
Flag risky ones with analysis." --output-format json

# Event-triggered: failing test handler
claude --print "Test suite failing in CI. Read the errors, fix the code, 
re-run until passing." 

# Scheduled weekly report
claude --print "Audit API usage logs for the past week. 
Generate summary report. Identify anomalies."
```

**Real-world applications:** CI/CD automation, scheduled audits, event-triggered agents, nightly jobs.

> ⚠️ Irreversible actions (file deletion, DB writes, external API posts) need explicit guardrails. Design your workflows so destructive actions require confirmation or are logged for human review.

---

## 🔴 Tier 1 — Core Workflows (Must Know)

> High-ROI. Real-world. These are the workflows power users build first.

---

### 1️⃣ RAG Pipeline Workflow
**Category:** Data Retrieval / AI Apps  
**ROI:** Turns static documents into queryable knowledge bases  
**Tools:** LlamaIndex / LangChain, Qdrant / Pinecone, Filesystem MCP

**The Pipeline:**
```
Raw Data (PDFs, docs, URLs)
    ↓ Load & extract text
    ↓ Chunk into segments (500–1000 tokens)
    ↓ Embed → vector representations
    ↓ Store in vector database
    ↓ [Query time] User question → embed → similarity search
    ↓ Top-k chunks retrieved → injected into Claude context
    ↓ Claude generates answer grounded in retrieved context
```

**Claude Code workflow:**
```bash
# Step 1: Load and parse documents
"Read all PDFs in /docs, extract text, chunk at 800 tokens with 100 overlap"

# Step 2: Generate embeddings and store
"Embed all chunks using the embeddings API, store in Qdrant at localhost:6333"

# Step 3: Build retrieval function
"Write a Python function: query → embed → search Qdrant → return top 5 chunks"

# Step 4: Build the generation layer
"Wrap the retrieval function with Claude API call. Inject chunks as context."

# Step 5: Test end-to-end
/testit
```

**Use case:** Internal knowledge bots, legal document Q&A, technical documentation assistants.

---

### 2️⃣ Multi-Agent Collaboration Workflow
**Category:** Agent Orchestration  
**ROI:** Complex tasks that exceed one context window or need parallel expertise  
**Tools:** Claude Agent Teams, CrewAI, AutoGen, LangGraph

**The Pattern:**
```
Orchestrator receives goal
    ↓ Decomposes into subtasks
    ↓ Assigns roles (researcher, coder, reviewer, deployer)
    ↓ Agents work in parallel or sequence
    ↓ Results aggregated and verified
    ↓ Final output delivered
```

**Claude Code implementation:**
```bash
# Define the team
/agents
@agent-create researcher "Researches architecture patterns and gathers context"
@agent-create implementer "Writes production code based on spec"
@agent-create reviewer "Reviews code for security and quality"
@agent-create test-writer "Writes comprehensive test suite"

# Orchestrate
"Build OAuth2 authentication system with refresh token rotation"

# Main conversation handles orchestration
@researcher "Gather requirements and analyze existing auth code"
@implementer "Implement based on researcher's spec"
@test-writer "Write tests for every endpoint"
@reviewer "Security review — OWASP focus"
```

**Real-world result:** Rakuten used this pattern to cut feature delivery from 24 working days to 5.

---

### 3️⃣ Code Generation Workflow
**Category:** Development  
**ROI:** End-to-end feature delivery without manual file juggling  
**Tools:** Claude Code, Context7, Superpowers plugin

**The Pipeline:**
```
Prompt → Explore codebase → Plan architecture → 
Generate code → Run tests → Auto-fix failures → 
Code review → Deploy
```

**Full workflow:**
```bash
# 1. Set context
/init  # Creates CLAUDE.md if not exists
/memory  # Update with feature requirements

# 2. Explore before building
"Read src/auth/ and understand current patterns. Do not write any code yet."

# 3. Plan first (with Superpowers installed, this is automatic)
Shift+Tab  # Plan mode

# 4. Build with live docs
"Build JWT authentication middleware. use context7"
# Context7 fetches actual Next.js/Express docs for current version

# 5. Verify
/diff      # Review all changes
! npm test # Run tests directly

# 6. Auto-fix loop
"Tests are failing with this error: [paste]. Fix until all pass."

# 7. Review
/simplify

# 8. Ship
! git add . && git commit -m "feat: JWT auth middleware"
```

---

### 4️⃣ AI App Builder Workflow
**Category:** App Development  
**ROI:** Full AI-powered app from idea to deployed  
**Tools:** Claude Code, Flowise/Dify (visual), Vercel MCP

**The Pipeline:**
```
Define requirements → Scaffold project structure →
Build UI components → Connect LLM logic →
Integrate tools/APIs → Test → Deploy
```

**Claude Code workflow:**
```bash
# 1. Scaffold
SCAFFOLD "Build a Next.js AI chat app with streaming responses"

# 2. Build the LLM layer
ARCHITECT "Design the API routes for Claude integration with streaming"

# 3. Build UI
"Build the chat interface component. use context7 for React 19 patterns"

# 4. Integrate tools
"Add the following MCP tools to the app: brave-search, filesystem"

# 5. Deploy
"Configure for Vercel deployment. Generate vercel.json."
```

**For visual builders:** Flowise and Dify let you build the same pipelines with drag-and-drop — then Claude Code generates the production version.

---

### 5️⃣ Data Analysis Workflow
**Category:** Analytics / Business Intelligence  
**ROI:** Natural language → actionable business insights  
**Tools:** Claude (data analysis mode), PostgreSQL MCP, Jupyter MCP

**The Pipeline:**
```
Raw data source → Clean & format → 
Analyze patterns → Generate visualizations → 
Write insights report
```

**Workflow:**
```bash
# With PostgreSQL MCP connected:
"Show me monthly revenue by product category for the last 12 months. 
Identify the top 3 growth drivers and any declining segments."

# Claude:
# → Inspects schema automatically
# → Writes optimized SQL
# → Executes query
# → Analyzes results
# → Returns formatted insights with trend analysis

# For deeper analysis:
"Generate a Python script that produces visualizations for these insights 
using matplotlib. Export as PNG."
```

**Non-technical user workflow:**
```
"Read the CSV files in /reports folder. 
Tell me what's going wrong with Q3 performance in plain English."
```

---

### 6️⃣ Prompt Optimization Workflow
**Category:** Prompt Engineering  
**ROI:** Turns unreliable prompts into production-grade, consistent outputs  
**Tools:** Anthropic Console, MEGAPROMPT command, /autoprompt

**The Pipeline:**
```
Draft initial prompt → Run against test cases →
Identify failure modes → Refine via meta-prompting →
Test edge cases → Document and save
```

**Workflow:**
```bash
# Step 1: Start rough
"Write a prompt for [task]"

# Step 2: Use MEGAPROMPT to expand
MEGAPROMPT — "summarize customer feedback"
# Claude transforms brief into comprehensive prompt with:
# - Persona definition
# - Task specification  
# - Output format
# - Edge case handling
# - Example outputs

# Step 3: Use /autoprompt to optimize
/autoprompt — [paste your current prompt]
# Claude diagnoses weaknesses and rewrites before executing

# Step 4: Test against failure cases
"Run this prompt against these 5 test cases and report failure rate: [cases]"

# Step 5: Iterate
PROMPTFIX — [paste failing prompt]
```

---

### 7️⃣ Web Scraping Workflow
**Category:** Data Collection  
**ROI:** Automated competitor research, market intelligence, price monitoring  
**Tools:** Puppeteer MCP, Firecrawl, Brave Search MCP

**The Pipeline:**
```
Define target → Render JavaScript → 
Extract DOM elements → Parse to structured JSON → 
Store / process → Monitor for changes
```

**Workflow:**
```bash
# With Puppeteer MCP:
"Go to [competitor URL], extract all pricing information, 
product names, and feature lists. Return as JSON."

# For JavaScript-heavy sites:
"Navigate to [URL], wait for the content to load fully, 
then extract the data table with class 'pricing-table'"

# For batch collection with Firecrawl:
"Scrape all blog posts from [site] published in 2026. 
Extract title, date, summary, and URL. Save as CSV."

# For monitoring:
"Check [URL] every hour. Alert me if the price changes by more than 10%."
```

---

### 8️⃣ Deployment Workflow
**Category:** DevOps / CI/CD  
**ROI:** Zero-touch code shipping from commit to production  
**Tools:** Docker MCP, Vercel MCP, GitHub MCP, GitHub Actions

**The Pipeline:**
```
Write code → Build container → 
Run test suite → Pass gates → 
Push to staging → Smoke test → 
Promote to production
```

**Workflow:**
```bash
# With GitHub + Vercel MCPs:

# 1. Verify before shipping
/diff               # Review all changes
/simplify           # Code quality pass
! npm test          # Run full test suite

# 2. Create PR with context
"Create a GitHub PR for this branch. Write a detailed description 
of what changed, why, and how to test it."

# 3. Monitor deployment
"Check the Vercel deployment status for the PR. 
Read the build logs if it failed and suggest fixes."

# 4. Promote
"The staging deployment looks good. Promote to production."
```

**Headless version (fully automated):**
```bash
# GitHub Action trigger
claude --print "Tests are failing on PR #$(PR_NUMBER). 
Read the errors, fix the code, push the fix." --dangerously-skip-permissions
```

---

### 9️⃣ Memory Persistence Workflow
**Category:** Context / Memory Systems  
**ROI:** Claude remembers your project across sessions — no re-explaining  
**Tools:** mem0 MCP, Memory MCP, Memora, CLAUDE.md

**The Pipeline:**
```
Session ends → Extract key context → 
Store in vector DB / knowledge graph →
New session starts → Query for relevant context →
Inject into prompt → Claude has continuity
```

**CLAUDE.md approach (simplest):**
```markdown
# Project: E-Commerce API

## Architecture
- Node.js + Express + PostgreSQL via Prisma
- JWT authentication with refresh token rotation
- All database queries in /services, not in route handlers

## Current State (updated 2026-04-28)
- Auth module: complete ✅
- Payment service: in progress 🔄
- Admin dashboard: not started ❌

## Rules
- Use async/await, never callbacks
- Write tests for all endpoints
- Return structured errors: { error, code, timestamp }
```

**With Memory MCP:**
```bash
# At session end
"Extract all key decisions, patterns, and progress from this session.
Store in memory with project tag: ecommerce-api"

# At session start
"Load context from memory for project: ecommerce-api.
What was I working on? What decisions were made?"
```

---

### 🔟 Document Processing Workflow
**Category:** NLP / Document Intelligence  
**ROI:** Turn unstructured documents into queryable, structured data  
**Tools:** Claude Vision, LlamaParse, Fetch MCP, Vector DB

**The Pipeline:**
```
Raw documents (PDF, DOCX, images) →
OCR / parse → Extract structure →
Chunk intelligently → Embed →
Index → Semantic query interface
```

**Workflow:**
```bash
# Single document
"Read this PDF contract. Extract: parties, key dates, 
payment terms, and any unusual clauses. Return as structured JSON."

# Batch processing
"Process all PDFs in /legal-docs. For each:
1. Extract party names and dates
2. Identify document type (NDA, contract, invoice)
3. Flag any documents with missing signatures
Save results to processed-docs.json"

# Build Q&A interface
"Build a RAG pipeline over these 50 medical papers. 
I should be able to ask questions and get cited answers."
```

---

## 🟠 Tier 2 — High Value Workflows

---

### 1️⃣1️⃣ Chatbot Development Workflow
**Category:** Conversational AI  
**Tools:** LangChain, Supabase MCP, Claude API

**The Pipeline:**
```
Define persona → Build conversation flow → 
Attach tools (search, DB, APIs) → 
Test edge cases → Deploy endpoint → Monitor
```

**Quick implementation:**
```bash
# Build the bot structure
ARCHITECT "Design a customer support chatbot for SaaS. 
Needs: ticket lookup, FAQ search, human escalation."

# Build system prompt
"Write a production system prompt for this bot. 
Persona: helpful but concise. Escalate when: angry customer, 
billing issue, technical bug they can't resolve in 2 turns."

# Add tools
"Integrate Supabase MCP for ticket lookup. 
Add Brave Search for FAQ matching."

# Test failure modes
/redteam "What are the 10 ways a user could break or abuse this chatbot?"
```

---

### 1️⃣2️⃣ API Integration Workflow
**Category:** Integration Engineering  
**Tools:** Fetch MCP, Postman MCP, REST / GraphQL APIs

**The Pipeline:**
```
Read API documentation → Generate typed client → 
Test each endpoint → Handle auth → 
Error handling → Rate limiting → Ship
```

**Workflow:**
```bash
# With Fetch MCP + Context7:
"Read the Stripe API docs at [URL]. 
Generate a TypeScript client for: create customer, 
create subscription, handle webhooks. 
Include full error handling and TypeScript types."

# Test
APIBUILD "Test all endpoints against Stripe sandbox. 
Return a report of which pass and which fail."
```

---

### 1️⃣3️⃣ Automated Testing Workflow
**Category:** QA / Software Validation  
**Tools:** Jest / PyTest / Vitest, GitHub MCP, test-writer-fixer plugin

**The Pipeline:**
```
Read codebase → Understand business logic → 
Generate unit tests → Generate integration tests →
Run suite → Auto-fix failures → Coverage report
```

**Workflow:**
```bash
# Install test-writer-fixer plugin first
/plugin install test-writer-fixer@claude-plugins-official

# Generate
/testit — generate comprehensive Jest test suite for src/payments/

# Auto-fix loop
"Run npm test. For every failing test:
1. Determine if the test is wrong or the code is wrong
2. Fix the issue
3. Re-run
Continue until all tests pass."

# Coverage
"Generate a coverage report. Identify untested branches. 
Write tests for anything below 80% coverage."
```

---

### 1️⃣4️⃣ CI/CD Pipeline Workflow
**Category:** DevOps  
**Tools:** GitHub Actions, GitLab MCP, Docker MCP

**The Pipeline:**
```
Commit pushed → Tests triggered → 
Build container → Security scan → 
Deploy to staging → Smoke tests → 
Auto-merge if passing → Production
```

**Claude Code headless CI:**
```yaml
# .github/workflows/claude-review.yml
name: Claude Code Review
on: [pull_request]
jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Claude Review
        run: |
          claude --print "Review this PR for: security issues, 
          breaking changes, missing tests, and documentation gaps. 
          Comment on the PR with findings." \
          --output-format json
```

---

### 1️⃣5️⃣ Knowledge Graph Workflow
**Category:** Knowledge Engineering  
**Tools:** mem-agent-mcp, Neo4j, graphify skill

**The Pipeline:**
```
Extract entities from text → 
Identify relationships → 
Create nodes and edges → 
Store in graph DB → 
Query with natural language
```

**Workflow:**
```bash
# Using graphify skill:
"Read all files in /codebase. 
Build a knowledge graph of:
- Functions and their dependencies
- Modules and their relationships  
- Data flow between components
Export as interactive visualization."

# Query it
"Which modules would be affected if I change the auth middleware?"
```

---

### 1️⃣6️⃣ UI/UX to Code Workflow
**Category:** Design-to-Code  
**Tools:** Figma MCP, Claude Vision, frontend-design plugin

**The Pipeline:**
```
Figma file → Extract design tokens → 
Read component structure → 
Generate React components → 
Map to existing component library → 
Test in browser
```

**Workflow:**
```bash
# With Figma MCP connected:
"Read the Figma file at [URL]. 
Extract all design tokens: colors, spacing, typography.
Generate a tokens.ts file."

"For the Dashboard component at node [ID]:
1. Extract the exact layout structure
2. Generate a React component that matches it
3. Use our existing Button and Card components where applicable"

# With frontend-design plugin:
"Build the landing page from this Figma design. 
Push for distinctive choices — not just a 1:1 copy."
```

---

### 1️⃣7️⃣ Audio Processing Workflow
**Category:** Transcription / Meeting Intelligence  
**Tools:** Whisper API, Claude API

**The Pipeline:**
```
Audio recording → Transcribe (Whisper) → 
Clean transcript → Summarize (Claude) → 
Extract action items → Send to Notion/Slack
```

**Workflow:**
```bash
# After Whisper transcription
"Here is the transcript of our 1-hour product meeting:
[transcript]

1. Write a 5-bullet executive summary
2. List all action items with owner names
3. List all decisions made
4. Flag any blockers or dependencies
5. Write a Slack message summary (<100 words)"
```

---

### 1️⃣8️⃣ Localization Workflow
**Category:** Internationalization  
**Tools:** Claude (translation), Fetch MCP

**The Pipeline:**
```
Source strings (JSON/PO files) → 
Translate with context → 
Maintain brand voice → 
Validate formatting → 
Output locale files
```

**Workflow:**
```bash
"Translate this JSON file from English to Spanish (Latin America).
Rules:
- Keep all JSON keys unchanged
- Maintain formal tone for business terms
- Preserve {variable} placeholders exactly
- Flag any strings with cultural nuance that need human review
Return the complete translated JSON."
```

---

### 1️⃣9️⃣ Recommendation System Workflow
**Category:** Machine Learning  
**Tools:** Vector DBs, Scikit-learn, PostgreSQL MCP

**The Pipeline:**
```
Collect user behavior → 
Embed items and user preferences → 
Find similar vectors → 
Filter by constraints → 
Rank and return recommendations
```

**Workflow:**
```bash
ARCHITECT "Design a content recommendation system.
User data: reading history, time spent, explicit ratings.
Items: blog posts with tags, categories, author.
Constraints: no recently read, respect user block list.
Scale: 100k users, 50k items."

# Implementation
"Build the embedding pipeline using sentence-transformers.
Store in Qdrant. Expose via FastAPI endpoint."
```

---

### 2️⃣0️⃣ Data Pipeline (ETL) Workflow
**Category:** Data Engineering  
**Tools:** PostgreSQL MCP, dbt, Apache Airflow

**The Pipeline:**
```
Extract (source systems) → 
Transform (clean, reshape, validate) → 
Load (data warehouse) → 
Schedule (recurring) → 
Monitor (alerts on failure)
```

**Workflow:**
```bash
"Design an ETL pipeline:
Source: PostgreSQL (production DB)
Transform: 
  - Anonymize PII fields
  - Calculate derived metrics (LTV, churn probability)
  - Aggregate to daily/weekly rollups
Destination: BigQuery data warehouse
Schedule: nightly at 2 AM UTC
Alerting: Slack message on failure"

# Claude generates:
# - Python ETL scripts
# - Airflow DAG definition
# - dbt transformation models
# - Monitoring setup
```

---

## 🟡 Tier 3 — Extended Workflows

---

### 2️⃣1️⃣ Content Generation Workflow
**Category:** Content Marketing / SEO  
**Tools:** Brave Search MCP, Notion MCP, Claude

```
Keyword research → Outline generation →
Draft content → SEO optimization → 
Internal linking → Publish
```

**Workflow:**
```bash
"Research the top 10 articles ranking for 'Claude Code workflows'.
Analyze: headings, word count, topics covered, gaps.
Generate an outline that covers all their topics 
PLUS the gaps they missed.
Target: 3000 words, developer audience."

# Draft
"Write the full article following this outline. 
Technical tone. Code examples in every section."

# Optimize  
"SEO pass: check keyword density, add semantic keywords, 
optimize headings, add meta description."
```

---

### 2️⃣2️⃣ Social Media Automation Workflow
**Category:** Marketing Automation  
**Tools:** Brave Search MCP, Slack MCP, Make.com

```
Monitor news sources → Extract relevant items →
Generate platform-specific posts → Review queue →
Schedule and publish
```

**Workflow:**
```bash
"Search for AI developer news from the last 24 hours.
Select the 3 most relevant items for a developer audience.
For each, write:
- Twitter/X: 280 chars, hook-first, include hashtags
- LinkedIn: 3 paragraphs, professional tone
- Slack (#dev-news): bullet summary

Queue them for review before posting."
```

---

### 2️⃣3️⃣ Email Automation Workflow
**Category:** Customer Communication  
**Tools:** Gmail MCP, SendGrid

```
Read inbound emails → Categorize intent →
Draft contextual reply → Human review queue →
Send approved replies
```

**Workflow:**
```bash
# With Gmail MCP:
"Read all unread emails in the support inbox.
Categorize each as: billing, bug report, feature request, general.
For billing and bug reports: draft a reply acknowledging the issue
and giving a 24-hour resolution timeline.
For feature requests: draft a reply thanking them and asking 3 clarifying questions.
Put all drafts in review — do not send."
```

---

### 2️⃣4️⃣ Infrastructure Backup Workflow
**Category:** Operations / Disaster Recovery  
**Tools:** AWS MCP, Google Drive MCP, Docker MCP

```
Trigger on schedule → Snapshot data volumes →
Compress → Encrypt → Transfer to cold storage →
Verify integrity → Alert on completion
```

**Workflow:**
```bash
# Headless scheduled workflow
claude --print "
1. Create EBS snapshots for all production volumes tagged 'backup:true'
2. Export PostgreSQL database to S3 with today's date prefix
3. Verify the S3 upload completed (check file size > 0)
4. Delete snapshots older than 30 days
5. Post completion summary to Slack #ops channel
" --dangerously-skip-permissions
```

---

### 2️⃣5️⃣ System Monitoring Workflow
**Category:** Observability / SRE  
**Tools:** Sentry MCP, PagerDuty, Datadog (via Fetch MCP)

```
Continuous log monitoring → Anomaly detection →
Root cause analysis → Alert routing →
Suggested fix → Auto-remediation (where safe)
```

**Workflow:**
```bash
"Monitor the error logs for the last hour.
Flag: error rate spikes (>2x baseline), 
new error types not seen before, 
errors affecting >10 unique users.

For each flagged issue:
1. Identify the root cause in the codebase
2. Estimate user impact
3. Draft a fix
4. Open a GitHub issue with full context
5. Alert #on-call Slack if severity > HIGH"
```

---

## ⚡ Power Workflow Combos

> These combinations are greater than the sum of their parts.

---

### The Full-Stack Feature Machine
**Superpowers + Context7 + feature-dev + Agent Teams**

```
1. Superpowers brainstorm → architecture decision
2. Context7 fetches live docs for chosen stack
3. feature-dev runs 7-phase workflow
4. Agent Teams: parallel frontend + backend + tests
5. /simplify → code review pass
6. /diff → verify → commit
```

**Result:** Production-quality features in hours, not days.

---

### The Research-to-Deploy Pipeline
**Brave Search + RAG + Code Generation + Vercel MCP**

```
1. Brave Search → gather current best practices
2. RAG pipeline → index gathered research
3. Code Generation → implement using indexed knowledge
4. Automated Testing → verify correctness
5. Vercel MCP → deploy
```

---

### The Zero-Human CI Loop
**GitHub MCP + Headless Claude + Auto-fix**

```
1. PR opened → Claude reads diff
2. Code review → comments on PR
3. Tests fail → Claude reads errors → fixes code → pushes
4. All gates pass → auto-merge
5. Production deployment → smoke tests
6. Any failure → Claude diagnoses → opens incident issue
```

---

### The Knowledge Base Machine
**Document Processing + RAG + Notion MCP + Memory MCP**

```
1. Upload docs to /knowledge folder
2. Claude processes, chunks, embeds all docs
3. Stored in vector DB with Notion MCP index
4. Memory MCP persists context across sessions
5. Team queries in natural language
6. Claude returns cited answers from company knowledge
```

---

## 📐 Workflow Design Principles

> From Anthropic's research on building effective Claude agents:

**1. Start simple, add complexity only when proven necessary**
- A single LLM call solves 80% of tasks
- Add workflow complexity only when the simple version fails
- Agents are not always better than a well-crafted prompt

**2. Design for the failure case first**
- What happens when Claude misunderstands?
- What happens when an API is down?
- What happens when a file doesn't exist?
- Build explicit error handling into every step

**3. Progressive autonomy — don't start headless**
```
Phase 1: Manual — user does everything, Claude suggests
Phase 2: Assisted — Claude does work, user approves each step
Phase 3: Supervised — Claude runs, user reviews output
Phase 4: Autonomous — Claude handles end-to-end, reports results
```
Start at Phase 2. Advance only when Phase 2 is consistently reliable.

**4. Irreversible actions need explicit gates**
- File deletion → confirm
- Database writes → confirm  
- External API posts (emails, messages) → confirm
- Never auto-execute destructive actions without a human gate

**5. Context management at scale**
- Monitor context with `/context` during long workflows
- Compact at 70% (`/compact retain [key context]`)
- Pass only essential information between workflow steps
- Don't chain the full output of Step A into Step B — summarize it

**6. The context window is your biggest constraint**
- 20 skills fighting for attention = worse than 3 focused ones
- Sub-agents help because they use separate context windows
- The operator pattern keeps your main context clean

---

## 📊 Quick Reference

| Workflow | Category | Key Tools | Complexity |
|----------|---------|-----------|-----------|
| RAG Pipeline | Data AI | LlamaIndex, Qdrant | ⭐⭐⭐ |
| Multi-Agent | Orchestration | Agent Teams, CrewAI | ⭐⭐⭐⭐⭐ |
| Code Generation | Dev | Claude Code, Context7 | ⭐⭐ |
| AI App Builder | App Dev | Flowise, Vercel MCP | ⭐⭐⭐ |
| Data Analysis | Analytics | PostgreSQL MCP | ⭐⭐ |
| Prompt Optimization | Prompt Eng | MEGAPROMPT, Console | ⭐⭐ |
| Web Scraping | Data | Puppeteer MCP, Firecrawl | ⭐⭐ |
| Deployment | DevOps | Docker, Vercel, GitHub | ⭐⭐⭐ |
| Memory Persistence | Context | mem0, Memory MCP | ⭐⭐⭐ |
| Document Processing | NLP | Claude Vision, Vector DB | ⭐⭐⭐ |
| Chatbot Dev | Conversational | LangChain, Supabase | ⭐⭐⭐ |
| API Integration | Integration | Fetch MCP, Postman | ⭐⭐ |
| Automated Testing | QA | Jest, test-writer-fixer | ⭐⭐ |
| CI/CD Pipeline | DevOps | GitHub Actions | ⭐⭐⭐ |
| Knowledge Graph | Knowledge | Neo4j, graphify | ⭐⭐⭐⭐ |
| UI/UX to Code | Frontend | Figma MCP, frontend-design | ⭐⭐⭐ |
| Audio Processing | NLP | Whisper API | ⭐⭐ |
| Localization | i18n | Claude translation | ⭐ |
| Recommendation | ML | Vector DB, Scikit-learn | ⭐⭐⭐⭐ |
| ETL Pipeline | Data Eng | dbt, Airflow, PostgreSQL | ⭐⭐⭐⭐ |
| Content Generation | Marketing | Brave Search, Notion | ⭐ |
| Social Automation | Marketing | Slack MCP | ⭐⭐ |
| Email Automation | Communication | Gmail MCP | ⭐⭐ |
| Infrastructure Backup | Ops | AWS MCP, Google Drive | ⭐⭐⭐ |
| System Monitoring | SRE | Sentry, PagerDuty | ⭐⭐⭐ |

---

## 🔗 Related Resources in This Repo

- [`commands.md`](./commands.md) — Complete Claude command reference
- [`mcp-servers.md`](./mcp-servers.md) — MCP servers and memory systems
- [`plugins.md`](./plugins.md) — Skills, plugins & extensions
- [`agent-frameworks.md`](./agent-frameworks.md) — Agent orchestration frameworks
- [`tools.md`](./tools.md) — Tools & utilities

---

*Part of **Claude OS** — a complete operating layer for Claude.*
*Star the repo if this saved you time.*

---

**Last Updated:** April 2026 · 25 workflows documented · 5 core agentic patterns
