# ⚡ Claude Commands — The Complete Reference

> **The most comprehensive Claude command reference on GitHub.**  
> Native CLI commands, slash commands, keyboard shortcuts, prompt codes, and power patterns — all in one place.  
> Updated for Claude Code v2.1+ and Opus 4.7 · April 2026

---

## 📋 Table of Contents

- [What Are Claude Commands?](#what-are-claude-commands)
- [Command Types Explained](#command-types-explained)
- [🔴 Native System Commands](#-native-system-commands) ← Start Here
- [🟠 Session & Context Commands](#-session--context-commands)
- [🟡 Code & Development Commands](#-code--development-commands)
- [🟢 Writing & Style Commands](#-writing--style-commands)
- [🔵 Thinking & Reasoning Commands](#-thinking--reasoning-commands)
- [🟣 Analysis & Strategy Commands](#-analysis--strategy-commands)
- [⚪ Creative & Content Commands](#-creative--content-commands)
- [🔶 Learning & Mastery Commands](#-learning--mastery-commands)
- [🔷 Research & Deep Dive Commands](#-research--deep-dive-commands)
- [⚫ Power & Hidden Mode Commands](#-power--hidden-mode-commands)
- [🎯 Artifacts & Creation Commands](#-artifacts--creation-commands)
- [⌨️ Keyboard Shortcuts](#️-keyboard-shortcuts)
- [🔧 CLI Flags & Launch Options](#-cli-flags--launch-options)
- [🏗️ Custom Commands (Build Your Own)](#️-custom-commands-build-your-own)
- [💡 Pro Workflows](#-pro-workflows)

---

## What Are Claude Commands?

Claude commands are **precision triggers** — single keywords or short codes that shift Claude's behavior, mode, or output style instantly.

There are three layers:

| Layer | What It Is | Example |
|-------|-----------|---------|
| **Native CLI** | Built into Claude Code binary | `/compact`, `/mcp`, `/plan` |
| **Prompt Codes** | Community-discovered activation phrases | `L99`, `OODA`, `XRAY` |
| **Custom Commands** | Your own `.md` files in `~/.claude/commands/` | `/deploy`, `/standup` |

The first layer is official. The second layer works consistently but isn't documented. The third layer is yours to build.

---

## Command Types Explained

```
Type 1: CLI Launch Flags    →  claude --print "question"
Type 2: Slash Commands      →  /compact  (typed inside session)
Type 3: Prompt Codes        →  L99  (typed at start of message)
Type 4: Keyboard Shortcuts  →  Shift+Tab  (key combos during session)
Type 5: Custom Skills       →  /your-command  (your .md files)
```

---

## 🔴 Native System Commands

> **These ship with Claude Code. 100% official. Use these first.**

### Core Session Management

| Command | What It Does | When To Use |
|---------|-------------|-------------|
| `/init` | Creates `CLAUDE.md` in project root — persistent project memory | First thing in any new project |
| `/compact` | Compresses conversation history, preserves key context | Context hits 70–80% |
| `/clear` | Hard wipes all conversation history | Switching to a completely different task |
| `/context` | Shows real-time context window usage (%) | Monitor regularly |
| `/cost` | Shows token consumption + session cost | After intensive work, monitor spend |
| `/memory` | Opens `CLAUDE.md` for editing mid-session | Adding new rules without leaving session |
| `/rewind` | Reverts conversation and/or code to a checkpoint | After bad change, experimental failure |
| `/resume` | Loads and continues a previous conversation | Returning to yesterday's work |
| `/fork` | Creates a temporary conversation branch to explore | Risky refactors, experiments |
| `/export` | Exports session as searchable documentation | After solving hard problems |

### Model & Mode Commands

| Command | What It Does | When To Use |
|---------|-------------|-------------|
| `/model` | Switch between Opus, Sonnet, Haiku mid-session | Quality vs speed trade-offs |
| `/model sonnet` | Switch to Claude Sonnet 4.6 | Everyday coding |
| `/model opus` | Switch to Claude Opus 4.7 | Complex architecture, critical code |
| `/model haiku` | Switch to Claude Haiku 4.5 | Quick edits, boilerplate |
| `/plan` | Forces planning mode — proposes changes before execution | Production-critical files |
| `/fast` | Speed-optimized API settings | Rapid iteration, live debugging |
| `/permissions` | Configure what Claude can auto-approve | Set once, save your attention |
| `/output-style` | Change Claude's response style | Concise / Educational / Reviewer |

### Code & Review Commands

| Command | What It Does | When To Use |
|---------|-------------|-------------|
| `/simplify` | Automated code quality review via parallel agents | Before every PR |
| `/review` | Triggers comprehensive code review | Major feature completion |
| `/diff` | Shows git diff of all changes made this session | Pre-commit verification |
| `/todos` | Persistent task list (survives session close) | Multi-session projects |
| `/agents` | Create and manage specialized sub-agents | Parallel work, delegation |

### Integration Commands

| Command | What It Does | When To Use |
|---------|-------------|-------------|
| `/mcp` | Manages MCP server connections and tool list | Adding/removing external tools |
| `/mcp add <name> <url>` | Connect an MCP server (HTTP transport) | One-time setup |
| `/mcp list` | Show all connected MCP servers | Troubleshooting |
| `/mcp remove <name>` | Disconnect an MCP server | Cleanup |
| `/plugins` | Browse and manage installed plugins | Extending Claude Code |
| `/batch` | Spawn parallel agents for large-scale changes | Refactors across 10+ files |

### Utility Commands

| Command | What It Does |
|---------|-------------|
| `/help` | Lists all available commands (always current) |
| `/vim` | Enables vim keybindings for prompt input |
| `/loop` | Runs recurring scheduled prompts/tasks |
| `/remote-control` | Control local Claude Code from claude.ai on phone |
| `/usage-report` | Monthly usage analytics as HTML report |
| `/btw` | Side question without derailing current task |
| `/buddy` | Easter egg — hatch a terminal pet companion 🐾 |

---

### 🔍 `/compact` Deep Dive

```bash
# Basic — auto-summarize everything
/compact

# Targeted — tell Claude what to remember
/compact retain the auth module patterns and migration strategy

# Rule: compact at 70% context, not 100%
# Check first: /context
```

**`/compact` vs `/clear`:**

| | `/compact` | `/clear` |
|-|-----------|---------|
| **Effect** | Summarizes, keeps thread | Hard wipe |
| **Use when** | Continuing same task | New task entirely |
| **History** | Preserved (compressed) | Gone |

---

## 🟠 Session & Context Commands

> **Manage your Claude Code environment like a pro.**

```bash
# Start fresh session
claude

# Continue last session in this directory
claude -c

# Continue specific session
claude --resume auth-refactor

# Resume from a pull request
claude --from-pr 123

# One-shot query (no interactive session)
claude --print "Explain this error: [paste error]"

# Non-interactive with JSON output
claude --print "List functions in app.js" --output-format json
```

---

## 🟡 Code & Development Commands

> **Community-verified prompt codes for coding workflows.**  
> Type these at the start of your message to activate the mode.

| Command | What It Does |
|---------|-------------|
| `/debug` | Deep root-cause analysis before suggesting fixes |
| `REFACTOR` | Structured refactoring with clean code principles |
| `/shipit` | Review → test → commit → push workflow |
| `ARCHITECT` | Design system architecture before writing code |
| `/convert` | Convert code between languages/frameworks |
| `AUTOMATE` | Identify and automate repetitive code patterns |
| `/testit` | Generate comprehensive test suite for current code |
| `SCAFFOLD` | Create project scaffolding and boilerplate |
| `/optimize` | Performance analysis + optimization suggestions |
| `APIBUILD` | Design and scaffold REST/GraphQL API endpoints |
| `/audit` | Full security + code quality audit |
| `BOTTLENECK` | Identify performance bottlenecks in code |
| `/refactor` | Improve readability while preserving functionality |
| `SCAFFOLD` | Auto-generate file structure and boilerplate |

**Usage:**
```
REFACTOR this authentication module. Focus on separation of concerns.

/testit — generate full Jest test suite for the payment service
```

---

## 🟢 Writing & Style Commands

> **Control Claude's writing voice and output quality.**

| Command | What It Does |
|---------|-------------|
| `/ghost` | Invisible writer mode — matches your voice exactly |
| `/mirror` | Mirrors the tone and style of provided sample text |
| `/raw` | Unfiltered direct output, no softening |
| `/voice` | Adapts to your specified communication style |
| `/punch` | Adds impact — stronger verbs, tighter sentences |
| `/flow` | Improves reading rhythm and sentence variation |
| `/trim` | Removes filler, redundancy, and padding |
| `/hook` | Rewrites opening for maximum attention capture |
| `/rephrase` | Restructures while preserving meaning |
| `/polish` | Final pass — grammar, tone, consistency |

**Usage:**
```
/trim — this section has too much filler

/hook — rewrite this intro, it's too slow
```

---

## 🔵 Thinking & Reasoning Commands

> **Force Claude into different cognitive modes.**  
> These don't just change output — they change *how Claude thinks* before responding.

| Command | Mode | Best For |
|---------|------|---------|
| `L99` | Maximum reasoning depth | Complex architecture decisions |
| `/deepthink` | Extended chain-of-thought | Problems that need multiple steps |
| `OODA` | Observe → Orient → Decide → Act | Tactical decisions under uncertainty |
| `CHAINLOGIC` | Explicit step-by-step logical chain | Debugging complex cause-effect |
| `/blindspots` | Identifies what you're not seeing | Strategy, planning, risk analysis |
| `OVERTHINK` | Exhaustive scenario analysis | High-stakes, irreversible decisions |
| `/unpack` | Breaks complex topics into components | Learning, explanation |
| `INVERT` | Solves by working backward from goal | Creative problem solving |
| `/layered` | Builds understanding layer by layer | Onboarding, documentation |
| `XRAY` | Sees through the surface to root cause | Debugging, analysis |

**Usage:**
```
L99 — What are the second-order consequences of switching to a microservices architecture?

INVERT — What would have to be true for this approach to completely fail?

XRAY — Why is this function returning undefined in production but not locally?
```

> **Pro tip:** `L99` + `XRAY` together is a powerful debug combo.

---

## 🟣 Analysis & Strategy Commands

> **Turn Claude into your strategic thinking partner.**

| Command | What It Does |
|---------|-------------|
| `/redteam` | Attack your own plan — find every weakness |
| `PARETO` | Identify the 20% of work that delivers 80% of value |
| `/swot` | Structured strengths, weaknesses, opportunities, threats |
| `WARGAME` | Simulate how competitors or adversaries respond |
| `/premortem` | Assume failure — work backwards to find causes |
| `LEVERAGE` | Find the highest-leverage action in a situation |
| `/audit` | Systematic audit — code, strategy, or process |
| `BOTTLENECK` | Find what's blocking the most progress |
| `/scenario` | Run multiple future scenarios |
| `BLINDSPOT` | Surface assumptions you haven't examined |

**Usage:**
```
/redteam our go-to-market strategy — punch holes in every assumption

PREMORTEM — it's 6 months from now and this feature failed completely. Why?
```

---

## ⚪ Creative & Content Commands

> **For marketers, writers, and content creators.**

| Command | What It Does |
|---------|-------------|
| `/viral` | Optimize content for shareability |
| `HOOKS10` | Generate 10 different hook variations |
| `/storysell` | Structure content using narrative selling frameworks |
| `REMIX` | Repurpose existing content for new formats/audiences |
| `/angles` | Generate 5–10 unique angles on a topic |
| `CLIFFHANGER` | Add tension and open loops to content |
| `/captionme` | Generate platform-optimized social captions |
| `POLARIZE` | Create takes that generate strong reactions |
| `/banger` | Write an opening line that stops the scroll |
| `THUMBNAIL` | Describe high-CTR thumbnail concepts |

**Usage:**
```
HOOKS10 — give me 10 different hooks for an article about AI coding tools

/storysell — rewrite this product description using a before/after/bridge structure
```

---

## 🔶 Learning & Mastery Commands

> **Transform Claude into a personalized instructor.**

| Command | What It Does |
|---------|-------------|
| `/teachme` | Patient, structured explanation from zero |
| `GAPFINDER` | Identifies what you don't know that you don't know |
| `/eli5` | Explain Like I'm 5 — maximum simplification |
| `MASTERCLASS` | Expert-level deep dive with nuance |
| `/drill` | Practice exercises with immediate feedback |
| `SPEEDRUN` | Fastest path to functional competence |
| `/mentor` | Ongoing coaching mode with corrections |
| `LEVELUP` | Identify and close your next skill gap |
| `CRASHCOURSE` | Compressed curriculum — essentials only |
| `BOOTCAMP` | Intensive, structured learning sprint |

**Usage:**
```
MASTERCLASS on React Server Components — assume I know React well

/drill — give me 5 SQL query exercises, increasing difficulty
```

---

## 🔷 Research & Deep Dive Commands

> **For analysis, fact-finding, and knowledge extraction.**

| Command | What It Does |
|---------|-------------|
| `/deepdive` | Exhaustive research on a topic |
| `FACTCHECK` | Verify claims with sources and confidence levels |
| `/sources` | Find primary sources for a topic |
| `COMPARE` | Side-by-side structured comparison |
| `/investigate` | Investigative analysis — follow the threads |
| `TRENDSCAN` | Identify patterns and directions in a space |
| `/extract` | Pull structured data from unstructured text |
| `TIMELINE` | Build chronological sequence of events |
| `/digest` | Compress long content into key takeaways |
| `DOSSIER` | Compile comprehensive profile on a topic/entity |

**Usage:**
```
COMPARE PostgreSQL vs MongoDB for a high-write SaaS product — be specific

DOSSIER — compile everything relevant about Model Context Protocol
```

---

## ⚫ Power & Hidden Mode Commands

> **Advanced. Use with intention.**

| Command | What It Does |
|---------|-------------|
| `/godmode` | Unlocks extended capabilities, removes conservative defaults |
| `/autoprompt` | Claude improves your prompt before executing it |
| `MEGAPROMPT` | Expands a brief request into a comprehensive prompt |
| `/chain` | Links multiple tasks in a sequential execution chain |
| `/system` | Explicit system-level instruction (use carefully) |
| `PERSONA` | Activates a specific expert persona |
| `/rolelock` | Locks Claude into a defined role for the full session |
| `PROMPTFIX` | Diagnoses and fixes a bad or underperforming prompt |
| `/nofilter` | Maximally direct output, no hedging |
| `BEASTMODE` | Full effort, no shortcuts, maximum output quality |

**Usage:**
```
BEASTMODE — I need the best possible version of this landing page copy

/autoprompt — [paste your rough prompt, Claude optimizes before running]

MEGAPROMPT — "build a SaaS dashboard" [Claude expands into full spec]
```

> ⚠️ **Note:** Some power commands override safety guardrails. Use in sandboxed or development environments.

---

## 🎯 Artifacts & Creation Commands

> **Trigger Claude to build, render, or generate structured outputs.**

| Command | What It Creates |
|---------|----------------|
| `ARTIFACTS` | Rich interactive artifacts (code, UI, data) |
| `/buildme` | Build a working feature end-to-end |
| `DASHBOARD` | Functional data dashboard |
| `PROTOTYPE` | Clickable prototype or proof of concept |
| `CANVAS` | Visual layout or design canvas |
| `/render` | Render output in specified format |
| `BLUEPRINT` | Technical architecture blueprint |
| `WIREFRAME` | Low-fidelity structural wireframe |
| `/livecode` | Live-updating code execution environment |
| `GENERATOR` | Build a content or code generator tool |

**Usage:**
```
DASHBOARD — build a real-time analytics dashboard for these metrics: [list]

BLUEPRINT — design the data architecture for a multi-tenant SaaS application
```

---

## ⌨️ Keyboard Shortcuts

> **Memorize these 8. They're worth more than 20 commands.**

### Essential (Learn First)

| Shortcut | Action |
|----------|--------|
| `Shift+Tab` | Cycle modes: Normal → Auto-Accept → Plan Mode |
| `Ctrl+C` | Cancel current generation |
| `Ctrl+R` | Search command history |
| `Esc Esc` | Open rewind menu |
| `Ctrl+T` | Toggle task list |
| `Tab` | Toggle thinking display |

### Navigation

| Shortcut | Action |
|----------|--------|
| `Alt+P` | Previous message |
| `Alt+N` | Next message |
| `Alt+B` | Back in conversation |
| `Alt+F` | Forward in conversation |

### Input

| Shortcut | Action |
|----------|--------|
| `Shift+Enter` | Multi-line input without sending |
| `Ctrl+L` | Clear screen (visual only) |
| `Ctrl+D` | Exit Claude Code |

### File & Inline Shortcuts

```bash
@src/auth.ts        # Include file in context
@src/components/    # Include entire directory
@https://url.com    # Fetch a URL

! git status        # Run bash command directly
! npm test          # Output feeds into context

# Use JWT for auth  # Quick memory add to CLAUDE.md
```

> **macOS:** Set Option key as Meta in iTerm2 for Alt shortcuts to work.  
> **WSL:** Run `/terminal-setup` to install Shift+Enter keybinding.

---

## 🔧 CLI Flags & Launch Options

```bash
# ── Session Management ──────────────────────────────────────
claude                          # Start new session
claude -c                       # Continue most recent session
claude --resume <session-name>  # Resume named session
claude --from-pr 123            # Resume session linked to PR

# ── One-Shot Mode ────────────────────────────────────────────
claude --print "your question"  # Ask and exit
claude -p "question"            # Short form

# ── Output Format ────────────────────────────────────────────
claude --print "..." --output-format json     # Structured JSON
claude --print "..." --output-format markdown # Markdown

# ── System Prompt ────────────────────────────────────────────
claude --append-system-prompt "Always use TypeScript strict mode"
claude --system-prompt "You are a Python security expert"

# ── Model Selection ──────────────────────────────────────────
claude --model claude-opus-4-7
claude --model claude-sonnet-4-6
claude --model claude-haiku-4-5-20251001
claude --model opusplan          # Opus thinking + Sonnet writing

# ── Agent Configuration ──────────────────────────────────────
claude --agents '{"test-writer": {"role": "Write Jest tests", "model": "claude-sonnet-4-6"}}'

# ── Safety ───────────────────────────────────────────────────
claude --dangerously-skip-permissions  # ⚠️ ONLY in trusted containers
```

---

## 🏗️ Custom Commands (Build Your Own)

The most underused feature in Claude Code. Any `.md` file in `~/.claude/commands/` becomes a slash command.

### Basic Custom Command

```markdown
<!-- ~/.claude/commands/ship.md -->
Review the current diff.
Run tests with `npm test`.
If tests pass, commit with a clear message and push to main.
If tests fail, analyze and fix them first.
```
Now `/ship` runs that entire workflow.

### Advanced Custom Command (with Frontmatter)

```markdown
---
name: security-scan
description: Run OWASP security audit on current codebase
allowed-tools: Read, Grep, Glob, Bash
model: claude-opus-4-7
---

Analyze the codebase for:
- SQL injection vulnerabilities
- XSS attack vectors
- Exposed credentials or API keys
- Insecure configurations
- Dependency vulnerabilities

Return findings as: CRITICAL / HIGH / MEDIUM / LOW
```

### Useful Custom Commands to Build

| Command | What It Should Do |
|---------|-------------------|
| `/standup` | Summarize yesterday's git commits as standup notes |
| `/ship` | Test → commit → push workflow |
| `/onboard` | Generate project summary from codebase for new devs |
| `/changelog` | Generate changelog from recent commits |
| `/postmortem` | Structured incident analysis template |
| `/spec` | Turn a feature brief into a technical spec |

---

## 💡 Pro Workflows

### Workflow 1: Daily Development Session

```bash
claude -c                          # Continue yesterday's session
/context                           # Check where you are
/memory                            # Update CLAUDE.md if needed

# Work on feature
/diff                              # Review all changes
! npm test                         # Run tests
/simplify                          # Code review pass
! git add . && git commit -m "..."
/cost                              # Check session cost
```

### Workflow 2: The L99 + XRAY Debug Stack

```
L99 XRAY — This function returns undefined in production but passes all tests locally.
Here is the relevant code: [paste]
Trace every possible execution path. Find what production has that dev doesn't.
```

### Workflow 3: Context Management

```bash
/context           # 70%+? Time to compact.
/compact retain the auth patterns and current error handling approach
/context           # Verify it dropped
```

### Workflow 4: Parallel Agents

```bash
# Main conversation: high-level architecture
/agents
@agent-create test-writer "Write comprehensive Jest tests for every endpoint"
@agent-create docs-writer "Generate API documentation from code"

# Delegate
@test-writer Generate full test suite for /api/payments
@docs-writer Document the authentication module
```

### Workflow 5: The BEASTMODE + /polish Combo

```
BEASTMODE — Write the technical deep dive section on MCP protocol architecture.
Be comprehensive. Don't hedge. Use concrete examples.
```
Then:
```
/polish — final pass for grammar, flow, and consistency
```

---

## 📊 Command Quick Reference

| Category | Best Commands |
|----------|--------------|
| **Daily drivers** | `/compact`, `/diff`, `/context`, `/cost`, `/memory` |
| **Code quality** | `/simplify`, `/review`, `REFACTOR`, `/testit`, `/audit` |
| **Thinking** | `L99`, `XRAY`, `OODA`, `/blindspots`, `INVERT` |
| **Writing** | `/punch`, `/trim`, `/hook`, `/polish`, `/ghost` |
| **Strategy** | `/redteam`, `PREMORTEM`, `PARETO`, `WARGAME` |
| **Power** | `BEASTMODE`, `MEGAPROMPT`, `/chain`, `/godmode` |
| **Learning** | `MASTERCLASS`, `/eli5`, `SPEEDRUN`, `L99` |

---

## 🔗 Related Resources in This Repo

- [`github-repos.md`](./github-repos.md) — Top GitHub repositories
- [`mcp-servers.md`](./mcp-servers.md) — Best MCP servers
- [`plugins.md`](./plugins.md) — Skills and plugins
- [`memory-systems.md`](./memory-systems.md) — Memory & context management
- [`workflows.md`](./workflows.md) — Production workflow patterns
- [`agent-frameworks.md`](./agent-frameworks.md) — Agent frameworks & orchestration
- [`tools.md`](./tools.md) — Tools & extensions

---

## Contributing

Found a command that works consistently but isn't listed?  
Open a PR. Include: command name, what it does, example usage, and whether it's native or prompt-code.

---

*Part of **Claude OS** — a complete operating layer for Claude.*
*Star the repo if this saved you time.*

---

**Last Updated:** April 2026 · Claude Code v2.1+ · Opus 4.7
