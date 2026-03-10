# Model Selection Workflow

## Model Hierarchy

| Model/Tool | Role | Use Case |
|------------|------|----------|
| **Qwen Coder** | Primary | Day-to-day conversations, general use |
| **GLM-4.7-FlashX** | Manager | Planning, gives orders to kiro-cli |
| **kiro-cli** | Worker | Implements what FlashX plans |
| **GLM-5** | Advisor | Ultra-complex reasoning (rare) |

**Workflow**: FlashX (plans) → kiro-cli (implements)

## Decision Matrix

### Use Qwen Coder (qwen-portal/coder-model) - PRIMARY
- ✅ Day-to-day tasks
- ✅ Coding and implementation
- ✅ General questions
- ✅ Code review
- ✅ Debugging
- ✅ Your default for everything

### Use kiro-cli (kiro_default agent) - THE WORKER
- ✅ Receives orders from FlashX
- ✅ Implements the plan
- ✅ File creation/editing
- ✅ Script generation
- ✅ Skill development
- **Usage**: `kiro-cli --agent kiro_default chat --trust-all-tools --no-interactive "task"`

### Use GLM-5 (zai/glm-5) - THE ADVISOR
- ✅ Ultra-complex reasoning
- ✅ When FlashX needs deeper thinking
- ✅ Critical security analysis
- ✅ Major architecture decisions

---

## Tool vs Model

| Type | Name | Role |
|------|------|------|
| **Model** | Qwen Coder | Day-to-day conversations (primary) |
| **Model** | GLM-4.7-FlashX | Manager - plans & orders |
| **Tool** | kiro-cli | Worker - implements plans |
| **Model** | GLM-5 | Advisor - deep thinking (rare) |

### Use GLM-4.7-FlashX (zai/glm-4.7-flashx) - THE MANAGER
- ✅ Plans what needs to be built
- ✅ Gives orders to kiro-cli
- ✅ Fast + reasoning (204K context)
- ✅ Breaks down complex tasks
- ✅ Reviews kiro output

## Workflow Pattern

```
┌──────────────────┐
│   Qwen Coder     │ ← You talk to me (PRIMARY)
│  (Conversations) │
└──────────────────┘
       │
       │ (need something built)
       ▼
┌──────────────────────────────────┐
│   GLM-4.7-FlashX (MANAGER)       │
│  - Plans the task                │
│  - Breaks down into steps        │
│  - Gives orders to kiro-cli      │
└──────────────────────────────────┘
       │
       │ (orders)
       ▼
┌──────────────────────────────────┐
│   kiro-cli (WORKER)              │
│  - Receives plan from FlashX     │
│  - Implements the code           │
│  - Creates files                 │
└──────────────────────────────────┘
       │
       │ (if ultra-complex)
       ▼
┌──────────────────────────────────┐
│   GLM-5 (ADVISOR)                │
│  - Deep analysis                 │
│  - Architecture decisions        │
└──────────────────────────────────┘
```

## Command Templates

### Planning with GLM-4.7-FlashX
```bash
# Spawn planner (DEFAULT)
sessions_spawn runtime=subagent model=zai/glm-4.7-flashx task="Plan a skill for..."
```

### Complex Analysis with GLM-5
```bash
# When you need deep thinking
sessions_spawn runtime=subagent model=zai/glm-5 task="Analyze security implications..."
```

### Coding with kiro-cli
```bash
# Implementation
kiro-cli --agent kiro_default chat --trust-all-tools --no-interactive "Create..."
```

### OpenClaw Development
```bash
# Use specialized agents
kiro-cli --agent openclaw-dev chat "Create a skill..."
kiro-cli --agent openclaw-architect chat "Review this skill..."
```

## Fallback Chain

1. **Qwen Coder** - Day-to-day tasks (your primary)
2. **For subagent planning** → GLM-4.7-FlashX (fast + reasoning)
3. **If ultra-complex** → GLM-5
4. **For coding with kiro-cli** → kiro_default agent

## Timeout Thresholds

| Model | Timeout | Notes |
|-------|---------|-------|
| Qwen Coder | 120s | Your day-to-day model |
| GLM-4.7-FlashX | 60s | Fast subagent planning |
| GLM-5 | 120s | Deep thinking |

## Example Session

```bash
# 1. Day-to-day: Just use Qwen (automatic)
# You: "Explain this code"
# Qwen handles it directly

# 2. Need something built? FlashX plans, kiro implements:
# Step 1: FlashX plans
sessions_spawn model=zai/glm-4.7-flashx task="Plan a subdomain scanner skill at /home/akz/.openclaw/workspace/skills/subdomain-scanner/"
  ↓ (FlashX returns detailed plan in 15s)

# Step 2: FlashX orders kiro
# (FlashX's plan includes specific kiro-cli commands)
kiro-cli --agent kiro_default chat --trust-all-tools --no-interactive "Create skill following this plan: [FlashX's plan]"
  ↓ (kiro creates files in 2min)

# 3. Ultra-complex (rare):
sessions_spawn model=zai/glm-5 task="Deep security analysis..."
```

## Manager-Worker Pattern

```
FlashX (Manager)          kiro-cli (Worker)
     │                          │
     ├─ 1. Plan task           │
     ├─ 2. Break into steps    │
     ├─ 3. Generate kiro cmd ──►│
     │                          ├─ 4. Execute plan
     │                          ├─ 5. Create files
     ◄─ 6. Review output ───────┤
     │                          │
     └─ 7. Approve/Direct       │
```

---

*Primary: Qwen Coder | Manager: GLM-4.7-FlashX | Worker: kiro-cli | Advisor: GLM-5*
