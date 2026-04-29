# 🧩 Claude Plugins, Skills & Extensions — The Complete Reference

> **Every plugin, skill, and extension worth installing in 2026.**  
> Official Anthropic plugins → viral community skills → developer toolkits → specialty packs.  
> Sourced from 14,634 repositories. Updated April 2026.

---

## 📋 Table of Contents

- [Skills vs Plugins vs MCP — What's What?](#skills-vs-plugins-vs-mcp--whats-what)
- [How to Install Plugins & Skills](#how-to-install-plugins--skills)
- [🔴 Tier 1 — Official Anthropic Plugins](#-tier-1--official-anthropic-plugins)
- [🟠 Tier 2 — Viral Community Plugins](#-tier-2--viral-community-plugins)
- [🟡 Tier 3 — Developer Productivity Skills](#-tier-3--developer-productivity-skills)
- [🟢 Tier 4 — Design & Frontend Skills](#-tier-4--design--frontend-skills)
- [🔵 Tier 5 — Specialty & Domain Skills](#-tier-5--specialty--domain-skills)
- [⚪ Tier 6 — Large Skill Libraries](#-tier-6--large-skill-libraries)
- [🧰 Plugin Management Tools](#-plugin-management-tools)
- [📦 Starter Setups by Role](#-starter-setups-by-role)
- [⚠️ Plugin Philosophy](#️-plugin-philosophy)

---

## Skills vs Plugins vs MCP — What's What?

Confused by all three terms? Here's the exact breakdown:

| Type | What It Is | Size | Example |
|------|-----------|------|---------|
| **Skill** | Markdown instruction file (`.skill` or `SKILL.md`) | ~100 tokens at scan, <5k active | TDD workflow, code review checklist |
| **Plugin** | Bundle of Skills + Commands + MCP configs + Hooks | Varies | Superpowers, feature-dev |
| **MCP Server** | External tool connection (GitHub, DB, browser) | Can use 50k+ tokens | GitHub MCP, PostgreSQL MCP |

```
Analogy:
Skill  = Recipe card      (teaches Claude how to cook something)
Plugin = Full cookbook    (skills + commands + automation bundled together)  
MCP    = Kitchen plumbing (connects Claude to external tools and data)
```

**How Skills work in 2026:**  
Each skill uses only ~100 tokens during metadata scanning. When activated, full skill loads at <5k tokens. This means you can have dozens installed without killing your context — Claude only loads what's relevant.

> ⚠️ **Security:** Skills can execute arbitrary code in Claude's environment. Only install from trusted sources.

---

## How to Install Plugins & Skills

### Method 1: Plugin Marketplace (Recommended)

```bash
# Add a marketplace
/plugin marketplace add <repo-path>

# Install a plugin from it
/plugin install <plugin-name>@<marketplace-name>

# Examples
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace

/plugin install context7@claude-plugins-official
/plugin install feature-dev@claude-plugins-official
/plugin install frontend-design@claude-plugins-official
/plugin install playwright@claude-plugins-official
```

### Method 2: Skills CLI

```bash
# Add a skill directly
claude skills add <repo-name>

# Examples
claude skills add juliusbrussee/caveman
claude skills add anthropics/claude-code-plugins
claude skills add eyaltoledano/taskmaster
```

### Method 3: npm Package Manager (ccpi)

```bash
# Install the ccpi package manager
pnpm add -g @intentsolutionsio/ccpi

# Use it
ccpi search <keyword>
ccpi install <plugin-name>
ccpi list --installed
ccpi update
```

### Where Skills Live

```
~/.claude/skills/          # Global skills (all projects)
.claude/skills/            # Project-level skills (current project)
~/.claude/commands/        # Legacy commands (still works)
.claude/commands/          # Legacy project commands (still works)
```

> **Note:** In 2026, `.claude/commands/` and `.claude/skills/` are unified. Old command files still work. New standard is Skills with YAML frontmatter.

---

## 🔴 Tier 1 — Official Anthropic Plugins

> **Made by Anthropic. Verified quality. Zero surprises. Start here.**  
> Official marketplace: [claude.com/plugins](https://claude.com/plugins) · [github.com/anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official)

---

### 1. Superpowers ⭐ MUST-HAVE
**Stars:** 121,000+ (one of the fastest-growing OSS projects of 2026)  
**Official:** Yes (accepted into Anthropic marketplace January 2026)  
**GitHub:** [obra/superpowers-dev](https://github.com/obra/superpowers-dev)

The most important plugin in the ecosystem. 66 specialized skills that transform Claude Code into a disciplined senior engineer — not just a code generator.

**What it does:**
- Forces planning before building — the methodology that made it viral
- TDD workflows with automatic test-first enforcement
- Brainstorming, spec writing, subagent development, code review
- Multi-agent coordination with isolated context windows
- CI/CD integration — runs your test suite to verify code before finishing

**Why it exploded:**  
The repo started gaining ~2,000 stars/day at peak. Not because of features — because it articulated a methodology experienced developers already knew: *plan before you build, test before you ship.*

**Workflow it unlocks:**
```
Describe feature → Superpowers brainstorms → Writes spec → 
Plans implementation → Subagents build in parallel → 
Tests verify → Review → Ship
```

**Install:**
```bash
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

**Best for:** Every developer. Non-negotiable install.

---

### 2. Context7 ⭐ MUST-HAVE
**Stars:** 53,864+  
**Official:** Yes (Anthropic marketplace)  
**GitHub:** [upstash/context7](https://github.com/upstash/context7)  
**Plugin page:** [claude.com/plugins/context7](https://claude.com/plugins/context7)

Fixes Claude's #1 problem for developers: outdated documentation.

Claude's training data is months old. When you're using Next.js 15, React 19, Tailwind 4, or any fast-moving library — outdated docs lead to hallucinated APIs and deprecated patterns. Context7 pulls live, version-specific documentation from source repos directly into context.

**How it works:**  
Two core tools:
- `resolve-library-id` — finds the library in Context7's index
- `query-docs` — fetches current documentation for your exact version

**Usage:**
```
"Create a Next.js 15 server component that fetches from Supabase. use context7"

"Configure Cloudflare Worker to cache JSON API responses. use context7"

"use library /supabase/supabase for API and docs"
```

**Supports:** React, Next.js, Supabase, Prisma, Tailwind, thousands more.

**Install:**
```bash
/plugin install context7@claude-plugins-official
```

**Best for:** Any developer using modern frameworks.

---

### 3. feature-dev (89,000+ Installs)
**Official:** Yes (Anthropic marketplace)

Turns a feature brief into production code via a structured 7-phase workflow. Not just code generation — it runs a senior engineer's full process.

**7 phases:**
1. Requirements gathering
2. Codebase exploration (parallel agents study your architecture)
3. Architecture design
4. Implementation
5. Testing
6. Code review
7. Documentation

**Usage:**
```
/feature-dev "Add OAuth2 authentication with JWT tokens and refresh token rotation"
```

Claude handles the rest. You review at key checkpoints.

**Install:**
```bash
/plugin install feature-dev@claude-plugins-official
```

**Best for:** Building complete features, not snippets.

---

### 4. frontend-design (277,000+ Installs)
**Official:** Yes (Anthropic marketplace)

Gives Claude professional design instincts before writing a single line of CSS. Without this, Claude generates forgettable "AI slop" — the generic aesthetic every AI-generated interface shares.

**What it does:**  
Before writing any CSS, Claude considers:
- Purpose and intended tone
- Visual differentiation strategy
- Unexpected font pairings
- Asymmetric layouts and orchestrated animations
- Layered visual depth

**The result:** Branded, intentional designs — not templates.

**Install:**
```bash
/plugin install frontend-design@claude-plugins-official
```

**Best for:** Any frontend work. Pair with `feature-dev` for full-stack features.

---

### 5. simplify
**Official:** Yes (Anthropic)

Code clarity agent. After implementation, before review — simplify runs a dedicated pass to reduce complexity while preserving functionality.

**Install:**
```bash
/plugin install simplify@claude-plugins-official
```

---

### 6. Playwright Plugin
**Official:** Yes (via Anthropic marketplace)

Claude controls a live Chrome browser. Navigate pages, run E2E tests, fill forms, scrape data — all from natural language.

**Usage:**
```
"Test the checkout flow: browse products, add to cart, fill test card, pay, verify confirmation"
```
Claude opens a visible browser and executes while you watch.

**Install:**
```bash
/plugin install playwright@claude-plugins-official
```

---

### 7. connect-apps
**Official:** Yes (Anthropic marketplace)

Connect Claude to 500+ services — Gmail, Slack, GitHub, Notion, and more — to take real actions across your entire tool stack from a single prompt.

**Install:**
```bash
/plugin install connect-apps@claude-plugins-official
```

---

### 8. code-review
**Official:** Yes (Anthropic)

Free. Flags issues a careful human reviewer would catch: security vulnerabilities, performance anti-patterns, logic edge cases, correctness issues.

**Install:**
```bash
/plugin install code-review@claude-plugins-official
```

---

## 🟠 Tier 2 — Viral Community Plugins

> Community-built. Battle-tested. Viral for a reason.

---

### 9. Caveman 🪨 (27,900+ Stars)
**GitHub:** [juliusbrussee/caveman](https://github.com/juliusbrussee/caveman)  
**Stars:** 27,900+

Makes Claude talk like caveman. Cuts ~65–75% of output tokens while keeping full technical accuracy.

**Before (69 tokens):**
```
"The reason your React component is re-rendering is likely because you're creating 
a new object reference on each render cycle. When you pass an inline object as a 
prop, React's shallow comparison sees it as a different object every time, which 
triggers a re-render. I'd recommend using useMemo to memoize the object."
```

**After (19 tokens):**
```
"New object ref each render. Inline prop = new ref = re-render. Wrap in useMemo."
```

Same fix. 75% fewer words. A March 2026 research paper found forced brevity improved accuracy by 26 percentage points on certain benchmarks — less verbose = more correct.

**Commands:**
```bash
/caveman          # Toggle on/off
/caveman lite     # Remove filler only
/caveman full     # Fragment style (default)
/caveman ultra    # Maximum terse
/caveman wenyan   # Classical Chinese (most token-efficient language)
```

**Bonus:**
```bash
/caveman-commit   # Git commits ≤50 chars
/caveman-review   # One-line PR comments
/caveman:compress # Compress CLAUDE.md (saves 46% input tokens)
```

**Works with:** Claude Code, Codex, Cursor, Gemini CLI, Copilot, 40+ agents.

**Install:**
```bash
claude skills add juliusbrussee/caveman
```

---

### 10. Taskmaster (eyaltoledano)
**GitHub:** [eyaltoledano/taskmaster](https://github.com/eyaltoledano/taskmaster)

Task management inside Claude Code. Break projects into subtasks, track dependencies, generate implementation plans — all persisted across sessions.

**What it does:**
- Parses PRDs into structured task lists
- Tracks dependencies and blocking relationships
- Generates implementation order based on dependencies
- Maintains state across session restarts
- 24 MCP tools for task/project/tag management

**Install:**
```bash
claude skills add eyaltoledano/taskmaster
```

**Best for:** Complex multi-step projects that span multiple sessions.

---

### 11. CaveKit (JuliusBrussee)
**GitHub:** [JuliusBrussee/cavekit](https://github.com/JuliusBrussee/cavekit)

From the creator of Caveman. A complete spec-driven development plugin: natural language → blueprint → parallel build plan → working software with automated testing and cross-model peer review.

**16 slash commands including:**
```bash
/ck:spec     # Write specification
/ck:build    # Execute build plan
/ck:check    # Drift detection
/ck:ship     # Ship workflow
/ck:review   # Code review
/ck:research # Pre-build research
```

**Architecture:** 12 named sub-agents, per-task token budgets, auto-backpropagation from test failures, Codex peer review.

**Install:**
```bash
/plugin marketplace add juliusbrussee/cavekit
/plugin install ck@cavekit
```

---

### 12. Repomix
**GitHub:** [yamadashy/repomix](https://github.com/yamadashy/repomix)  
**Stars:** Top trending April 2026

Packs your entire repository into a single AI-readable file. The workflow it kills: copying and pasting 15 individual files to give Claude codebase context.

**Usage:**
```bash
npx repomix        # Pack current directory
npx repomix --remote github.com/user/repo  # Pack any GitHub repo
```

Claude reads the complete codebase context in one shot.

**Best for:** Large codebase analysis, architecture reviews, onboarding new AI sessions.

---

### 13. BMAD Method
**GitHub:** [bmadcode/BMAD-METHOD](https://github.com/bmadcode/BMAD-METHOD)  
**Stars:** 45,826+

Breakthrough Method for Agile AI-Driven Development. A structured multi-agent workflow for building full products — from PRD to production. One of the most comprehensive agent orchestration methodologies available as a plugin.

**Install:**
```bash
claude skills add bmadcode/BMAD-METHOD
```

---

### 14. backlog
**Plugin:** backlog (available in marketplace)

Persistent, cross-session task management. 24 MCP tools for tasks, projects, tags, dependencies, and docs. 7 skills for planning, standups, and handoffs. Event-sourced storage, agent coordination — everything survives session restarts.

**Best for:** Teams using Claude Code collaboratively across sessions.

---

## 🟡 Tier 3 — Developer Productivity Skills

> Single-purpose skills that solve specific painful problems.

---

### Code Quality

| Plugin | What It Does | Install |
|--------|-------------|---------|
| **code-review** | Comprehensive review: security, patterns, edge cases | `/plugin install code-review@claude-plugins-official` |
| **test-writer-fixer** | Auto-write and fix unit tests (Jest, Vitest, Pytest) | `/plugin install test-writer-fixer@claude-plugins-official` |
| **debugger** | Advanced bug tracking — traces complex execution paths | `/plugin install debugger@claude-plugins-official` |
| **bug-fix** | Analyzes stack traces and fixes bugs end-to-end | `/plugin install bug-fix@claude-plugins-official` |
| **security-sweep** | OWASP Top 10 (2025) + Mobile + LLM vulnerability scan | `/plugin install security-sweep@composio` |
| **AgentLint** | Lint your repo for AI agent compatibility — 33 checks | `claude skills add agentlint` |

---

### Architecture & Backend

| Plugin | What It Does | Install |
|--------|-------------|---------|
| **backend-architect** | API design, database schemas, system design patterns | `/plugin install backend-architect@claude-plugins-official` |
| **mcp-builder** | Guides creation of production-quality MCP servers | `claude skills add anthropics/mcp-builder` |
| **agent-sdk-dev** | Claude Agent SDK development helper | `/plugin install agent-sdk-dev@claude-plugins-official` |

---

### Git & Workflow

| Plugin | What It Does | Install |
|--------|-------------|---------|
| **commit-commands** | Smart git commit messages from diffs | `/plugin install commit-commands@claude-plugins-official` |
| **documentation-generator** | READMEs, API docs, guides from code | `/plugin install documentation-generator@claude-plugins-official` |
| **ccusage** | CLI dashboard: token spend, costs, session analytics | `npm install -g ccusage` |

---

### Search & Research

| Plugin | What It Does | Notes |
|--------|-------------|-------|
| **Tavily** | Enhanced web search + research integration | Requires API key |
| **Brave Search MCP** | Live internet access, real-time data | Via MCP |
| **ArXiv skill** | Academic paper search and summarization | Community |

---

## 🟢 Tier 4 — Design & Frontend Skills

> For developers who care about what their UI looks like.

---

### frontend-design (Already in Tier 1 — Essential)

---

### artifacts-builder
Suite of tools for creating elaborate, multi-component HTML artifacts using React, Tailwind CSS, and shadcn/ui. For complex interactive interfaces that go beyond single-file outputs.

**Install:**
```bash
/plugin install artifacts-builder@claude-plugins-official
```

---

### theme-factory
Applies professional font and color themes to artifacts — slides, docs, reports, HTML landing pages. 10 pre-set themes. One command to go from default to designed.

**Install:**
```bash
/plugin install theme-factory@claude-plugins-official
```

---

### canvas-design
Creates visual art as PNG and PDF outputs — posters, one-pagers, visual assets — using design philosophy and aesthetic principles.

**Install:**
```bash
/plugin install canvas-design@claude-plugins-official
```

---

### ui-ux-pro-max-skill
**Stars:** 16,900+  
**GitHub:** [ui-ux-pro-max-skill](https://github.com/win4r/ui-ux-pro-max-skill)

Design intelligence skill for building professional UI/UX across multiple platforms. Goes beyond frontend-design into multi-platform design systems.

**Install:**
```bash
claude skills add win4r/ui-ux-pro-max-skill
```

---

### nano-banana
Google Gemini image generation plugin. Text-to-image, style transfer, 4K output, multi-reference composition — from a single `/genimage` command inside Claude Code.

**Install:**
```bash
/plugin install nano-banana@composio
```

---

## 🔵 Tier 5 — Specialty & Domain Skills

> Skills for specific industries, stacks, and use cases.

---

### Security & Compliance

| Skill | Stars | What It Does | Install |
|-------|-------|-------------|---------|
| **security-sweep** | — | OWASP Top 10 + LLM Top 10 vulnerability scanning | `/plugin install security-sweep` |
| **security-guidance** | — | Secure coding best practices, OWASP guidelines | `/plugin install security-guidance` |
| **Trail of Bits security** | Community | Security research, vulnerability detection, audit workflows | `claude skills add trailofbits/claude-skills` |
| **agentlint** | — | 33 checks for AI agent compatibility | `claude skills add agentlint` |

---

### Mobile Development

| Skill | What It Does | Install |
|-------|-------------|---------|
| **React Native / Expo** | Project setup, component generation, native modules | `claude skills add react-native/expo-dev` |
| **Swift concurrency** | async/await, actors, structured concurrency — no callback hell | `claude skills add avdlee/swift-concurrency-agent-skill` |

---

### Cloud & DevOps

| Skill | What It Does | Install |
|-------|-------------|---------|
| **AWS skills** | CloudFormation, Lambda, S3, IAM policy generation | `claude skills add aws/cloud-skills` |
| **Firebase** | Firebase integration patterns and deployment | `claude skills add firebase/firebase` |
| **Docker automation** | Container management and orchestration | `claude skills add docker/container-skills` |

---

### Content & Marketing

| Skill | What It Does | Notes |
|-------|-------------|-------|
| **Content pipeline** | Blog posts, social media, email sequences with SEO | Community |
| **Brand Voice** | Teach Claude your exact brand tone and forbidden words | Custom |
| **Marketing skills (alirezarezvani)** | 43 marketing skills — CRO, copywriting, SEO, analytics | See Tier 6 |
| **YouTube workflow** | Research, ideas, briefs, titles, thumbnails — full production pipeline | jeremylongshore |

---

### Productivity & Management

| Skill | What It Does | Install |
|-------|-------------|---------|
| **maestro-orchestrate** | 22 subagents, 4-phase multi-agent development orchestration | `/plugin install maestro-orchestrate` |
| **backlog** | Persistent cross-session task management | `/plugin install backlog` |
| **executive-assistant-skills** | C-suite decision frameworks, strategy tools | `ccpi install executive-assistant-skills` |

---

### Research & Science

| Skill | Stars | What It Does | Install |
|-------|-------|-------------|---------|
| **scientific-agent-skills** | 18,400+ | Research, science, engineering, analysis, finance, writing | `claude skills add scientific-agent-skills` |
| **graphify** | 15,300+ | Knowledge graph creation from codebases and wikis | `claude skills add graphify` |

---

## ⚪ Tier 6 — Large Skill Libraries

> Full skill ecosystems — install selectively, not all at once.

---

### alirezarezvani/claude-skills — 232+ Skills
**Stars:** 5,200+  
**GitHub:** [alirezarezvani/claude-skills](https://github.com/alirezarezvani/claude-skills)

The most comprehensive open-source skill library. Works with Claude Code, Codex, Gemini CLI, Cursor, and 8 more agents.

**Categories:**
- 24 core engineering skills
- 25 advanced/POWERFUL-tier engineering
- 12 product management skills
- 43 marketing skills (CRO, copywriting, SEO, analytics)
- 12 regulatory/quality management (MDR, ISO)
- 6 project management
- 28 C-level advisory (full C-suite coverage)
- 4 business & growth

**Install by domain:**
```bash
/plugin marketplace add alirezarezvani/claude-skills
/plugin install engineering-skills@claude-code-skills        # 24 core
/plugin install engineering-advanced-skills@claude-code-skills # 25 advanced
/plugin install product-skills@claude-code-skills            # 12 product
/plugin install marketing-skills@claude-code-skills          # 43 marketing
/plugin install c-level-skills@claude-code-skills            # 28 C-level
```

---

### jeremylongshore/claude-code-plugins-plus-skills — 423 Plugins
**GitHub:** [jeremylongshore/claude-code-plugins-plus-skills](https://github.com/jeremylongshore/claude-code-plugins-plus-skills)  
**Marketplace:** [tonsofskills.com](https://tonsofskills.com)

423 plugins, 2,849 skills, 177 agents across 18 categories. Updated daily by GitHub Actions. The largest single-repo plugin collection.

**Notable packs:**
- `devops-automation-pack` — full DevOps automation workflow
- `apollo-skills` — 24 skills for Apollo.io sales engagement
- `clay-skills` — 30 skills for Clay data enrichment workflows
- `google-design-sprint` — Google Ventures 5-day methodology
- `lean-startup` — Validated learning and MVP frameworks

**Install:**
```bash
/plugin marketplace add jeremylongshore/claude-code-plugins
/plugin install devops-automation-pack@claude-code-plugins-plus
ccpi install <pack-name>  # Via ccpi package manager
```

---

### Agent Skills Library — 1,400+ Skills
**Stars:** Top trending  

Installable library of 1,400+ agentic skills for Claude Code, Cursor, Codex CLI, Gemini CLI, and more. Includes installer CLI, bundles, workflows, and official/community skill collections.

---

## 🧰 Plugin Management Tools

### CCHub — Desktop Manager
Desktop app for managing the Claude Code ecosystem: MCP marketplace, config profiles, skills & plugins browser, workflow templates, security audit. Built with Tauri v2 + React + Rust.

```bash
/plugin install CCHub@claude-plugins-official
```

### ccusage — Cost Analytics
CLI tool for analyzing Claude Code session logs. Token consumption dashboard, cost breakdown, model usage tracking.

```bash
npm install -g ccusage
ccusage          # Dashboard
ccusage --week   # Weekly summary
```

### cc-statusline — Context Monitor
Real-time status bar showing context%, active tools, running agents, and todo progress — visible while Claude works.

### claude-familiar (/buddy extension)
Enhances Claude Code's `/buddy` companion with personality, mood, interactive commands (`fortune`, `roast`, `haiku`, `focus timer`). Mood shifts on tool success/failure.

```bash
/plugin install claude-familiar@composio
```

---

## 📦 Starter Setups by Role

### For Full-Stack Developers
```bash
# Essential 5
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
/plugin install context7@claude-plugins-official
/plugin install feature-dev@claude-plugins-official
/plugin install frontend-design@claude-plugins-official
/plugin install code-review@claude-plugins-official
claude skills add juliusbrussee/caveman
```

### For Backend / API Developers
```bash
/plugin install superpowers@superpowers-marketplace
/plugin install context7@claude-plugins-official
/plugin install backend-architect@claude-plugins-official
/plugin install test-writer-fixer@claude-plugins-official
/plugin install security-sweep@composio
claude skills add eyaltoledano/taskmaster
```

### For Product Managers
```bash
/plugin install feature-dev@claude-plugins-official
claude skills add alirezarezvani/claude-skills
/plugin install product-skills@claude-code-skills
/plugin install backlog
```

### For Security Engineers
```bash
/plugin install security-sweep@claude-plugins-official
/plugin install security-guidance@claude-plugins-official
claude skills add trailofbits/claude-skills
/plugin install debugger@claude-plugins-official
```

### For Content / Marketing Teams
```bash
/plugin install connect-apps@claude-plugins-official
/plugin install marketing-skills@claude-code-skills
/plugin install documentation-generator@claude-plugins-official
claude skills add juliusbrussee/caveman
```

### The Absolute Minimum (Everyone)
```bash
# These 3. Nothing else until you know what you need.
/plugin install superpowers@superpowers-marketplace
/plugin install context7@claude-plugins-official
claude skills add juliusbrussee/caveman
```

---

## ⚠️ Plugin Philosophy

> From the most experienced Claude Code users in the community:

**Less is more.**

Every skill adds to your context window baseline. Twenty skills competing for attention is worse than three focused ones. The context engineering principle applies here: less noise, better signal.

**Before installing, ask:**
- Could I write these instructions myself in 20 minutes?
- Does this match my actual workflow, not a generic best practice?
- Am I dependent on someone maintaining a repo I don't control?

**The best Claude Code users run 2–5 plugins max.** Some run zero — they write precise `CLAUDE.md` files instead, tailored to their actual codebase.

**The rule:**
```
Start with 3 plugins.
Add one more only when you hit a repeated friction point.
Never install "just in case."
```

---

## 📊 Quick Reference — Top Plugins

| Plugin | Stars | Type | Install |
|--------|-------|------|---------|
| Superpowers | 121k+ | Official | `/plugin install superpowers` |
| Context7 | 53.8k+ | Official | `/plugin install context7` |
| Caveman | 27.9k+ | Community | `claude skills add juliusbrussee/caveman` |
| feature-dev | 89k installs | Official | `/plugin install feature-dev` |
| frontend-design | 277k installs | Official | `/plugin install frontend-design` |
| Taskmaster | Active | Community | `claude skills add eyaltoledano/taskmaster` |
| CaveKit | Growing | Community | `/plugin install ck@cavekit` |
| BMAD Method | 45.8k+ | Community | `claude skills add bmadcode/BMAD-METHOD` |
| Repomix | Trending | Utility | `npx repomix` |
| claude-skills (232+) | 5.2k+ | Library | `/plugin marketplace add alirezarezvani/claude-skills` |

---

## Where to Find More Plugins

| Resource | What's There |
|----------|-------------|
| [claude.com/plugins](https://claude.com/plugins) | Official Anthropic-verified plugins |
| [claudemarketplace.com](https://claudemarketplace.com) | 150+ community skills with ratings |
| [claudepluginhub.com](https://claudepluginhub.com) | Directory with descriptions and reviews |
| [tonsofskills.com](https://tonsofskills.com) | 400+ skills via ccpi package manager |
| [github.com/ComposioHQ/awesome-claude-plugins](https://github.com/ComposioHQ/awesome-claude-plugins) | Curated list with adoption metrics |
| [github.com/quemsah/awesome-claude-plugins](https://github.com/quemsah/awesome-claude-plugins) | Automated metrics from 14,634 repos |
| [github.com/travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) | Curated skills with community validation |

---

## 🔗 Related Resources in This Repo

- [`commands.md`](./commands.md) — Complete Claude command reference
- [`mcp-servers.md`](./mcp-servers.md) — MCP servers and memory systems
- [`github-repos.md`](./github-repos.md) — Top GitHub repositories
- [`workflows.md`](./workflows.md) — Production workflow patterns
- [`agent-frameworks.md`](./agent-frameworks.md) — Agent orchestration

---

*Part of the **Ultimate Claude Bundle** — the most complete Claude resource on GitHub.*  
*Star the repo if this saved you time.*

---

**Last Updated:** April 2026 · 14,634 repos indexed · 40+ plugins documented
