---
name: smart-model-switching
description: "Auto-route tasks to cheapest model. Multi-provider: GLM (Flash→Standard→Plus) + Qwen (Coder→Flash). Routes coding to kiro-cli."
triggers: complex, coding, analysis, security, audit, refactor, implement, debug, architecture, design, plan
---

# Smart Model Switching

Auto-route tasks to cheapest model. Multi-provider support: **GLM** + **Qwen**.

## The Golden Rule

> If a human would need more than 30 seconds of focused thinking, escalate to a stronger model.
> If the task involves architecture, complex tradeoffs, or deep reasoning, use the best model.
> Start cheap, scale only when needed.

---

## Model Reality (Multi-Provider)

### GLM Models (z.ai)

| Tier | Model | Purpose |
|------|-------|---------|
| 💚 Flash | `glm-4-flash` | Fastest & cheapest |
| 💛 Standard | `glm-4.7` | Strong reasoning & code |
| ❤️ Plus/32B | `glm-5` | Heavy reasoning & architecture |

### Qwen Models (qwen-portal)

| Tier | Model | Purpose |
|------|-------|---------|
| 💚 Flash | `qwen-portal/coder-model` | Fast coding, general tasks |
| 💛 Standard | `qwen-portal/coder-model` | Strong coding & reasoning |

**Note**: Qwen currently uses `coder-model` as the primary option. Good for coding tasks!

---

## Provider Selection

| Provider | Best For | Cost |
|----------|----------|------|
| **Qwen** | Coding, general tasks | Free/OAuth |
| **GLM Flash** | Simple Q&A, quick tasks | Cheap |
| **GLM Standard** | Analysis, planning | Medium |
| **GLM Plus** | Architecture, complex reasoning | Expensive |

**Default**: Use Qwen (`qwen-portal/coder-model`) for most tasks. Escalate to GLM-5 only when needed.

---

## 💚 FLASH Tier — Simple Tasks

**GLM**: `glm-4-flash` | **Qwen**: `qwen-portal/coder-model`

Stay on Flash for:
- Factual Q&A — "what is X", "who is Y", "when did Z"
- Quick lookups — definitions, unit conversions, short translations
- Status checks — monitoring, file reads, session state
- Heartbeats — periodic checks, OK responses
- Memory & reminders
- Casual conversation — greetings, acknowledgments
- Simple file ops — read, list, basic writes
- One-liner tasks — anything answerable in 1–2 sentences
- Cron jobs (always Flash by default)

### NEVER do these on Flash
- ❌ Write code longer than 10 lines
- ❌ Create comparison tables
- ❌ Write more than 3 paragraphs
- ❌ Do multi-step analysis
- ❌ Write reports or proposals

---

## 💛 STANDARD Tier — Core Workhorse

**GLM**: `glm-4.7` | **Qwen**: `qwen-portal/coder-model`

Escalate to Standard for:

### Code & Technical
- Code generation — functions, scripts, features
- Debugging — normal bug investigation
- Code review — PRs, refactors
- Documentation — README, comments, guides

### Analysis & Planning
- Comparisons and evaluations
- Planning — roadmaps, task breakdowns
- Research synthesis
- Multi-step reasoning

### Writing & Content
- Long-form writing (>3 paragraphs)
- Summaries of long documents
- Structured output — tables, outlines

Most real user conversations belong here.

---

## ❤️ PLUS / 32B Tier — Complex Reasoning Only

**GLM**: `glm-5` | **Qwen**: (use GLM for complex tasks)

Escalate to Plus/32B for:

### Architecture & Design
- System and service architecture
- Database schema design
- Distributed or multi-tenant systems
- Major refactors across multiple files

### Deep Analysis
- Complex debugging (race conditions, subtle bugs)
- Security reviews
- Performance optimization strategy
- Root cause analysis

### Strategic & Judgment-Based Work
- Strategic planning
- Nuanced judgment and ambiguity
- Deep or multi-source research
- Critical production decisions

---

## 🎯 Quick Decision Tree

```
Is it a greeting, lookup, status check, or 1–2 sentence answer?
  YES → FLASH (glm-4-flash or qwen-portal/coder-model)
  NO ↓

Is it code, analysis, planning, writing, or multi-step?
  YES → STANDARD (glm-4.7 or qwen-portal/coder-model)
  NO ↓

Is it architecture, deep reasoning, or a critical decision?
  YES → PLUS / 32B (glm-5)
  NO → Default to STANDARD, escalate if struggling
```

---

## 🔀 Full Routing Decision Tree

```
Task received
│
├── Is it coding/development?
│   ├── YES → Use kiro-cli (best for file ops)
│   │   kiro-cli --agent kiro_default chat --trust-all-tools --no-interactive "task"
│   │
│   └── NO → Check complexity
│       ├── Simple → glm-4-flash or qwen-portal/coder-model
│       ├── Standard → glm-4.7 or qwen-portal/coder-model
│       └── Complex → glm-5
```

---

## 📋 Quick Reference Card

```
┌─────────────────────────────────────────────────────────────┐
│                  SMART MODEL SWITCHING                       │
│         Qwen (default) → GLM Flash → Standard → Plus         │
├─────────────────────────────────────────────────────────────┤
│  💚 FLASH (cheapest)                                         │
│  • glm-4-flash, qwen-portal/coder-model                      │
│  • Greetings, status checks, quick lookups                   │
│  • Factual Q&A, reminders                                    │
│  • Simple file ops, 1–2 sentence answers                     │
│  • Cron jobs (always Flash)                                  │
├─────────────────────────────────────────────────────────────┤
│  💛 STANDARD (workhorse)                                     │
│  • glm-4.7, qwen-portal/coder-model                          │
│  • Code > 10 lines, debugging                                │
│  • Analysis, comparisons, planning                           │
│  • Reports, long writing                                     │
├─────────────────────────────────────────────────────────────┤
│  ❤️ PLUS / 32B (complex)                                     │
│  • glm-5 (best for complex)                                  │
│  • Architecture decisions                                    │
│  • Complex debugging, multi-file refactoring                 │
│  • Strategic planning, deep research                         │
│  • Security audits, critical decisions                       │
├─────────────────────────────────────────────────────────────┤
│  💡 RULE: >30 sec human thinking → escalate                  │
│  💰 START CHEAP → SCALE ONLY WHEN NEEDED                     │
└─────────────────────────────────────────────────────────────┘
```

---

## Model Switching Commands

```bash
# Switch to GLM-5 (complex tasks)
session_status model="zai/glm-5"

# Switch to GLM-4-Flash (simple tasks)
session_status model="zai/glm-4-flash"

# Switch to GLM-4.7 (standard tasks)
session_status model="zai/glm-4.7"

# Switch to Qwen (coding, general)
session_status model="qwen-portal/coder-model"

# Reset to default
session_status model="default"
```

---

## For Subagents

```javascript
// Routine monitoring (cheap)
sessions_spawn(task="Check backup status", model="GLM-4-Flash")

// Standard code work (Qwen)
sessions_spawn(task="Build the REST API endpoint", model="qwen-portal/coder-model")

// Complex architecture (GLM-5)
sessions_spawn(task="Design the database schema for multi-tenancy", model="GLM-5")
```

---

## For Cron Jobs

```json
{
  "payload": {
    "kind": "agentTurn",
    "model": "GLM-4-Flash"
  }
}
```

Always use Flash for cron unless the task genuinely needs reasoning.

---

## When to Use kiro-cli

- Creating new files
- Modifying existing code
- Debugging code issues
- Multi-file refactoring
- Code analysis and review
- Working with filesystem

```bash
kiro-cli --agent kiro_default chat --trust-all-tools --no-interactive "your coding task here"
```

---

## Example Usage

```
User: "Create a Flask REST API for users"

Me:
"Coding task detected. Routing to kiro-cli..."
[exec: kiro-cli --agent kiro_default chat --trust-all-tools --no-interactive "Create a Flask REST API for users"]

---

User: "What's the capital of France?"

Me:
"Simple question, responding with qwen-portal/coder-model..."
"Paris"

---

User: "Analyze this security vulnerability in detail"

Me:
"Complex analysis task. Switching to GLM-5..."
[session_status model="zai/glm-5"]
[analyzes with GLM-5]
```

---

## Cost Optimization

| Priority | Model | Use Case |
|----------|-------|----------|
| 1 (Default) | `qwen-portal/coder-model` | Coding, general tasks (OAuth, free) |
| 2 (Simple) | `glm-4-flash` | Quick Q&A, status checks |
| 3 (Standard) | `glm-4.7` | Analysis, planning |
| 4 (Complex) | `glm-5` | Architecture, security audits |
| 5 (Coding) | `kiro-cli` | File operations, multi-file projects |

**Strategy**: 
1. Default to Qwen for most tasks
2. Use GLM-Flash for simple/quick tasks
3. Escalate to GLM-5 only for complex reasoning
4. Use kiro-cli for coding with file operations

---

*Built for multi-provider setups (GLM + Qwen). Start cheap, escalate smart.*
