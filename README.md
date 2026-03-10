# OpenClaw Skills Collection 🦞

A collection of custom skills for OpenClaw agents.

---

## 📦 Installed Skills

### 🧠 Smart Model Switching

**Path:** `smart-model-switching/`

Auto-route tasks to the cheapest model based on complexity. Supports multi-provider routing (GLM + Qwen).

#### Features

- **Three-tier routing**: Flash → Standard → Plus/32B
- **Multi-provider support**: Z.AI (GLM) + Qwen
- **kiro-cli routing**: Automatic routing for coding tasks
- **Cost optimization**: Start cheap, escalate only when needed
- **Quick decision tree**: Visual flowchart for model selection

#### Models

| Tier | Model | Use Case |
|------|-------|----------|
| 💚 Flash | `glm-4-flash` | Simple Q&A, greetings, cron jobs |
| 💛 Standard | `qwen-portal/coder-model`, `glm-4.7` | Coding, analysis, planning |
| ❤️ Plus | `glm-5` | Architecture, complex debugging, security audits |

#### Usage

The skill automatically routes tasks based on complexity:

```
Task received
│
├── Coding? → kiro-cli
│
├── Simple? → glm-4-flash
│
├── Standard? → qwen-portal/coder-model
│
└── Complex? → glm-5
```

#### Installation

```bash
# Clone this repo
git clone https://github.com/akzedevops/openclaw-skills.git

# Copy skill to OpenClaw
cp -r openclaw-skills/smart-model-switching ~/.openclaw/workspace/skills/
```

---

### 🔍 Shodan Recon Scanner

**Path:** `shodan-recon-scanner/`

Shodan-powered reconnaissance and vulnerability scanner. Search for exposed services, CVE vulnerabilities, and perform security recon on IPs/domains.

#### Features

- **Device Discovery**: Find exposed services and devices
- **CVE Scanning**: Search for vulnerable hosts by CVE
- **Host Analysis**: Detailed IP/domain reconnaissance
- **Honeypot Detection**: Identify honeypots before engagement
- **Data Export**: Download and parse scan results

#### CVE Quick Reference

| CVE | Name | Search Query |
|-----|------|--------------|
| CVE-2021-44228 | Log4Shell | `vuln:CVE-2021-44228` |
| CVE-2019-0708 | BlueKeep | `vuln:CVE-2019-0708 port:3389` |
| CVE-2017-0144 | EternalBlue | `vuln:CVE-2017-0144 port:445` |
| CVE-2022-22965 | Spring4Shell | `vuln:CVE-2022-22965` |

#### Quick Commands

```bash
# Check account info
shodan info

# Search for Apache servers
shodan search "apache country:US"

# Get host info
shodan host 8.8.8.8

# Check honeypot score
shodan honeyscore 8.8.8.8

# Scan for Log4Shell
shodan search "vuln:CVE-2021-44228"
```

#### Triggers

`shodan`, `recon`, `cve-scan`, `vuln-scan`, `exposed-services`, `device-search`, `ip-recon`

---

## 🔧 Available Skills

| Skill | Description | Status |
|-------|-------------|--------|
| `smart-model-switching` | Multi-provider model routing | ✅ Active |
| `shodan-recon-scanner` | Shodan recon & CVE scanning | ✅ Active |
| `security-auditor` | OWASP security audits | ✅ Active |
| `vector-memory` | Qdrant semantic memory | ✅ Active |

---

## 🚀 Quick Start

### 1. Clone the Repo

```bash
git clone https://github.com/akzedevops/openclaw-skills.git
cd openclaw-skills
```

### 2. Install Skills

```bash
# Copy all skills
cp -r * ~/.openclaw/workspace/skills/

# Or copy specific skill
cp -r smart-model-switching ~/.openclaw/workspace/skills/
```

### 3. Restart Gateway

```bash
openclaw gateway restart
```

### 4. Verify Installation

```bash
ls ~/.openclaw/workspace/skills/
```

---

## 📚 Skill Structure

Each skill follows the OpenClaw skill format:

```
skill-name/
├── SKILL.md          # Skill definition and instructions
├── script.sh         # Optional: Helper scripts
└── config.json       # Optional: Configuration files
```

### SKILL.md Frontmatter

```yaml
---
name: skill-name
description: What the skill does
triggers: keyword1, keyword2, keyword3
---
```

---

## 🤝 Contributing

1. Fork the repo
2. Create a new skill folder
3. Add `SKILL.md` with frontmatter
4. Test in your OpenClaw instance
5. Submit a PR

---

## 📝 License

MIT License - Feel free to use and modify!

---

## 🔗 Links

- **OpenClaw Docs**: https://docs.openclaw.ai
- **ClawHub**: https://clawhub.com
- **OpenClaw GitHub**: https://github.com/openclaw/openclaw

---

*Built with 🦞 for the OpenClaw community*
