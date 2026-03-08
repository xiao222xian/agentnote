
<p align="center">
  <img src="https://github.com/user-attachments/assets/2b366fd7-e5d6-4522-8681-a10b83b911da" alt="OpenClaw Logo" width="600">
</p>

<h1 align="center">🦞 OpenClaw: The Ultimate Autonomous AI Agent</h1>

<p align="center">
  <strong>Surpassing Linux as the fastest-growing open-source AI Agent framework in history.</strong>
</p>

<p align="center">
  English | <a href="./README.md">简体中文</a>
</p>

<p align="center">
  <img src="https://img.shields.io/github/stars/user/openclaw?style=for-the-badge&logo=github&color=darkverify" alt="GitHub Stars">
  <img src="https://img.shields.io/github/license/user/openclaw?style=for-the-badge&color=darkverify" alt="License">
  <img src="https://img.shields.io/badge/status-Creator_of_History-darkverify?style=for-the-badge" alt="Status">
</p>

---

### Project Background

In early 2026, **OpenClaw** made history on GitHub: reaching over **247,000 stars in just two months**, officially surpassing Linux as the world's fastest-growing open-source project.

> **The Core Essence**: If ChatGPT is the advisor, OpenClaw is the **executor**. It is a true **Autonomous AI Agent**.

---

## 📜 Historical Milestones

The timeline reflects the inevitable trend: **2026 is the Year of the AI Agent**.

* **2025.11**: Initial release under the name `Clawdbot`.
* **Renaming**: Brief change to `Moltbot` due to trademark issues.
* **2026.01**: Officially branded as **OpenClaw**.
* **2026.02**: Viral growth; became the fastest-growing project in GitHub history.
* **2026.02.14**: Developers joined OpenAI; project transitioned to an Open Source Foundation.

---

## 🛡️ Why OpenClaw?

Unlike SaaS AI assistants, OpenClaw’s **Localization** ensures sensitive data never leaves your device.

1.  **Data Sovereignty**: Switch between cloud APIs (Claude, GPT) or fully local models (Ollama).
2.  **Local Memory**: All "memories" are stored in local Markdown files—readable, editable, and portable.
3.  **Enterprise-Grade Security**: Adopted by major tech giants (ByteDance, Alibaba, Tencent) for its "Controllable AI Execution." Deploy on private clouds to prevent leaks of code or customer data.
4.  **Self-Hosting**: Run it entirely on your own server for maximum privacy.

---

## 🏗️ Core Architecture

OpenClaw features a rigorous four-layer design for flexibility and control:

1.  **Channels**: Access via Telegram, Discord, Slack, CLI, Feishu, and QQ for commands anywhere.
2.  **Brain**: The decision-making core responsible for LLM reasoning, task decomposition, and planning.
3.  **Skills**: The plugin system providing file operations, Shell commands, browser control, and API integrations.
4.  **Dual-Memory System**:
    * `SOUL.md`: Defines identity, personality, and behavioral guidelines.
    * `MEMORY.md`: Stores long-term memory and user preferences.
    * **Context**: Manages short-term conversation history.

---

## 🤖 Multi-Agent Advantages

OpenClaw can spawn **Sub-agents** to handle complex tasks:

* **Parallel Exploration**: Sub-agents can simultaneously research docs, market reports, and data, aggregating results much faster than a single agent.
* **Context Isolation**: Prevents "Context Degradation." Sub-agents work in clean environments to avoid repeating failed paths.
* **Scaling Inference Power**: Breaks the 200K token limit; each Sub-agent has its own context window.

> **Trade-off**: Token costs may increase significantly (up to 15x). 
> **Usage Tip**: Use for independent sub-tasks that only require final aggregation.

---

## ⚙️ Installation & Setup

### Requirements
* **Node.js**: v22 or higher
* **RAM**: 1GB minimum (4GB recommended)
* **Port**: 18789 must be available

### Setup Steps
```bash
# Install OpenClaw CLI globally
npm install -g openclaw@latest

# Verify installation
openclaw --version


### Configuration

Run the interactive wizard:

```bash
openclaw onboard --install-daemon

```

1. **Select Model**: Enter your provider's API Key.
2. **Configure Channels**: (Optional) Setup Slack, Telegram, etc.
3. **Install Skills**: Recommendation: skip initially until familiar with basic ops.
4. **Auto-Start**: The Gateway daemon starts automatically after config.

### Monitor & Control

```bash
# Check Gateway status
openclaw status

# Open Web Dashboard
openclaw dashboard

```

*Accessible at http://localhost:18789*

---

## 🛠️ FAQ: "Just chatting, no action?"

**Problem**: The Agent gives advice instead of executing commands.
**Cause**: After v2026.3.2, `Tools Profile` defaults to `messaging` for security.

**Fix (Recommended)**:

```bash
# Check current profile
openclaw config get tools
# Set to full
openclaw config set tools.profile full
# Restart Gateway
openclaw gateway restart

```

---

## 🧰 Skill System

OpenClaw Skills are like an App Store for your Agent. Each skill includes **Tool Definitions**, **Config Templates**, and **Prompt Instructions**.

### Skills Marketplace

The community maintains a repository at [clawdhub.com](https://clawhub.com) with **17,777+** skills.

| Category | Examples | Use Case |
| --- | --- | --- |
| 📧 **Comms** | Gmail, Slack | Email management, notifications |
| 📅 **Productivity** | Google Calendar | Schedule & task tracking |
| 🔍 **Search** | Brave Search, Tavily | Real-time web search |
| 💻 **Dev** | GitHub, Docker | Code management & automation |
| 📝 **Content** | PDF Parser, Markdown | Doc processing & formatting |
| 🌐 **Browser** | Playwright | Web scraping & automation |

### Browse & Install

```bash
# Search for skills
openclaw skill search weather

# Install a skill
openclaw skill install weather

```

---

## 🔒 Security Checklist

1. **Server**: Use SSH keys, disable passwords, and enable `fail2ban`.
2. **API Keys**: Store in `.env`, rotate every 3 months, and set usage limits.
3. **Behavior**: Define a "No-Go" list in `SOUL.md`. Confirm destructive operations (delete/modify).

---

## 🔮 Future Outlook

* **Multimodal Standard**: Real-time camera analysis, natural voice, and robot control.
* **Agent Networks**: A "Butler Agent" coordinating specialized agents (Dev, Data, Comms).
* **First-Mover Advantage**: The earlier you start, the more "Cognitive Accumulation" your Agent builds. It learns your habits over time—there are no shortcuts.

**The best time to start is now.**

```

---
