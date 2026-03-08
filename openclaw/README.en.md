
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


<br/><br/>

# Appendix

## 🌟 What is CoPaw?

**CoPaw** (Co Personal Agent Workstation) is far more than just a chatbot; it is a **Personal AI Workstation** hosted locally or on your private cloud.

If traditional AI assistants are like a rented "office," CoPaw is your **hand-crafted "Digital Estate."** It deeply integrates model management, multi-channel access, skill extensions, and a long-term memory system, designed to provide a private, powerful, and autonomous AI partner.

### 🎯 Core Scenarios

* **Omni-platform Communication**: Summon your exclusive AI directly within Feishu, QQ, Discord, or Slack.
* **Deep Personalization**: Enable the AI to remember your professional preferences, writing style, and daily habits over the long term.
* **Automated Secretary**: Set up scheduled tasks for daily industry briefings or task reminders.
* **Data Sovereignty**: Supports full localized deployment; sensitive data never passes through third-party platforms, ensuring absolute privacy.

---

## ⚖️ CoPaw vs. OpenClaw: Which to Choose?

Both are excellent self-hosted Agent platforms, but they possess distinct "personalities":

| Feature | **CoPaw (Personal Workstation)** | **OpenClaw (Execution Hub)** |
| --- | --- | --- |
| **Core Positioning** | Focused on "Companionship & Collaboration"; emphasizes the workstation experience | Focused on "Developers & Execution"; emphasizes the toolchain |
| **Control Method** | Elegant **Web Dashboard (Port 8088)** | Powerful **CLI and Gateway** |
| **Tech Stack** | Based on Python Ecosystem (AgentScope) | Node.js / Cross-language plugin system |
| **Typical Features** | Scheduled heartbeats, summary generation, auto-loading Skills | Task execution chains, complex plugin extensions |
| **Target Audience** | Users seeking daily ease-of-use, web control, and long-term memory | Geeks seeking automated workflows and developer-level control |

> **Bottom Line**: If you want a "Digital Butler" with a web-based interface, choose **CoPaw**; if you want a "Swiss Army Knife" via the command line, choose **OpenClaw**.

---

## 🏗️ Core Architecture & Features

1. **Multi-Channel Access**: One-click bridging for Feishu, QQ, Discord, etc., breaking down application silos.
2. **Intelligent Brain**: Supports mainstream cloud models and local inference models based on `llama.cpp` or `MLX`.
3. **Skill Extensions**: A plugin-like system supporting rapid loading of custom tools.
4. **Scheduled Tasks (Cron Jobs)**: Built-in heartbeat checks and periodic summaries, giving the Agent a sense of time.
5. **Flexible Deployment**: Supports local, Docker, Alibaba Cloud ECS, and other environments.

---

## 🚀 Quick Start Guide

### Option A: Python Installation (Recommended)

Suitable for users with an existing Python environment:

```bash
# 1. Install the core package
pip install copaw

# 2. Initialize configuration (using defaults)
copaw init --defaults

# 3. Launch the application
copaw app

```

Once started, access `http://127.0.0.1:8088/` in your browser to enter the Web Dashboard.

### Option B: Docker Deployment

Suitable for users seeking environment isolation or server deployment:

```bash
# Pull the image
docker pull agentscope/copaw:latest

# Run the container (Map port 8088 and mount data volume)
docker run -d \
  --name copaw \
  -p 8088:8088 \
  -v copaw-data:/app/working \
  agentscope/copaw:latest

```

*Configurations, memory, and Skills will be persisted in the `copaw-data` volume.*

### Option C: One-line Install Script

The official documentation provides a one-line install script for a zero-friction experience. Please visit the [Official Quick Start Page](https://www.google.com/search?q=https://docs.agentscope.io/copaw) for the latest script commands.

---

## 🤖 Model Configuration

### Cloud Models

During the `copaw init` process, follow the prompts to select your provider (e.g., OpenAI, Anthropic, DashScope) and enter your **API Key**.

### Local Models (Privacy First)

CoPaw is extremely friendly to local models, supporting `llama.cpp` or `MLX` backends:

* **No API Key Required**: Directly load GGUF format models.
* **Local Download Example**:

```bash
# Example: Download a Qwen model via built-in command
copaw download --model qwen2.5-7b-gguf
copaw app

```

---

## 🛠️ Advanced: Config & Extension

* **Skills Path**: Custom skills can be placed in the `/app/working/skills` folder and will auto-load upon restart.
* **Scheduled Tasks**: Edit task flows directly in the dashboard to set the Agent's "heartbeat" or wake-up frequency.

---
