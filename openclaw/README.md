
<p align="center">
  <img src="https://github.com/user-attachments/assets/2b366fd7-e5d6-4522-8681-a10b83b911da" alt="OpenClaw Logo" width="600">
</p>

<h1 align="center">🦞 OpenClaw: 自主式 AI Agent 巅峰之作</h1>

<p align="center">
  <strong>超越 Linux，全球增长最快的开源自主 AI Agent 框架</strong>
</p>
<p align="center">
  <a href="./README.en.md">English</a>
</p>
<p align="center">
  <img src="https://img.shields.io/github/stars/user/openclaw?style=for-the-badge&logo=github&color=darkverify" alt="GitHub Stars">
  <img src="https://img.shields.io/github/license/user/openclaw?style=for-the-badge&color=darkverify" alt="License">
  <img src="https://img.shields.io/badge/status-Creator_of_History-darkverify?style=for-the-badge" alt="Status">
</p>

---

### 项目背景

2026 年初，**OpenClaw** 在 GitHub 上创造了历史：**两个月内获得超过 247,000 星标**，正式超越 Linux 成为全球增长最快的开源项目。

> **核心定义**：ChatGPT 是顾问，OpenClaw 是执行者。它是一个真正的**自主式 AI Agent**。

---
## 📜 项目传奇背景 (Project History)

这个时间线本身就说明了一个趋势：**2026 年是 AI Agent 元年**。

* **2025.11**：以 `Clawdbot` 名称首次发布。
* **短暂更名**：因 Anthropic 商标问题改为 `Moltbot`。
* **2026.01**：正式定名 `OpenClaw`。
* **2026.02**：病毒式传播，成为 GitHub 历史上增长最快的项目。
* **2026.02.14**：开发者宣布加入 OpenAI，项目整体转移到开源基金会。

---

## 🛡️ 为什么选择 OpenClaw？

与传统的 SaaS AI 助手不同，OpenClaw 的**本地化特性**意味着敏感数据永远不会离开你的设备。

1. **数据主权**：你可以选择使用云端 API（如 Claude、GPT）或完全本地模型（如 Ollama）。
2. **本地记忆**：所有"记忆"存储在本地 Markdown 文件中，可读、可编辑、可迁移。
3. **企业级安全**：字节跳动、阿里云、腾讯云等国内云厂商已提供托管服务，正是看中了这种"可控的 AI 执行力"。企业可以在私有云部署，确保代码、文档、客户数据不外泄。
4. **自主部署**：你可以完全本地运行，或自托管在私有服务器上，处理任何级别的敏感信息。

---

## 🏗️ 核心架构 (Architecture)

OpenClaw 的架构分为严谨的四层设计，既灵活又可控：

1. **消息渠道层 (Channels)**：支持 Telegram、Discord、Slack、CLI、飞书、QQ 等，实现随时随地指令下达。
2. **智能决策核心 (Brain)**：负责 LLM 推理、任务分解与规划、工具选择与调用。
3. **技能插件系统 (Skills)**：提供文件操作、Shell 命令、浏览器控制、API 集成等。
4. **双模记忆系统 (Memory)**：
* `SOUL.md`：定义 Agent 的身份、性格与行为准则。
* `MEMORY.md`：存放长期记忆，记住你的偏好和项目背景。
* **对话历史**：维护短期上下文。



---

## 🤖 多智能体 (Multi-Agent) 优势分析

在处理复杂任务时，OpenClaw 可以衍生出子 Agent（Sub-agent）：

* **并行探索**：研究竞品分析时，主 Agent 派出多个 Sub-agent 分别搜索技术文档、分析市场报告、爬取价格信息，汇总速度远超串行。
* **上下文隔离**：避免"上下文退化"。当主对话积累太多干扰信息时，Sub-agent 在干净的环境中执行，保证决策质量。
* **推理算力扩展**：突破单个 Agent 的 Token 限制（如 200K），每个 Sub-agent 拥有独立空间，总预算大幅提升。

> **代价与权衡**：Token 成本可能从 1x 跃升到 15x。
> **使用建议**：子任务相互独立且只需汇总结果时，推荐拆分；若需要共享大量即时上下文，建议合并。

---

## ⚙️ 安装与验证 (Installation)

### 环境要求

* **Node.js**：22 或更高版本
* **内存**：至少 1GB，推荐 4GB
* **端口**：18789 必须可用

### 安装步骤

```bash
# 全局安装 OpenClaw CLI
npm install -g openclaw@latest

# 验证安装
openclaw --version

```

看到版本号说明安装成功。

### 初始化配置

运行交互式向导：

```bash
openclaw onboard --install-daemon

```

1. **AI 模型选择**：输入对应提供商的 API Key。
2. **渠道配置**：询问是否接入 Slack、Telegram 等（可暂时跳过，后续通过 `openclaw configure` 添加）。
3. **技能安装**：列出可用技能。建议初期跳过，熟悉后再装。
4. **自动启动**：配置完成后，Gateway 守护进程会自动运行。

### 检查与控制

```bash
# 检查 Gateway 状态
openclaw status
# 看到 "Gateway service: running" 说明成功

# 打开 Web 控制面板
openclaw dashboard
# 浏览器将自动打开 http://localhost:18789 进行对话

```

---

## 🛠️ 常见问题：只会聊天不干活？

**现象**：让它执行命令，它却只给出建议和代码块。
**原因**：2026.3.2 版本后，`Tools Profile` 默认被设为安全级别较高的 `messaging`。

**修复方法（命令行推荐）**：

```bash
# 1. 查看当前 profile
openclaw config get tools
# 2. 如果不是 full，切换成 full
openclaw config set tools.profile full
# 3. 重启生效
openclaw gateway restart

```

---

## 📲 全平台移动端控制

无论在通勤、咖啡厅还是出差，手机即是遥控器。

### 1. 飞书接入 (推荐企业)

参看文档：《立省 500！30 分钟把 OpenClaw 在飞书上配到可用》https://larkcommunity.feishu.cn/wiki/LDmXwEVhJitBa5kU0mjc16VKneb。  

<img width="663" height="836" alt="image" src="https://github.com/user-attachments/assets/7d24622a-6922-49eb-8c06-6e9f946416a3" />


### 2. Telegram 接入 (推荐个人)

通过 `openclaw configure` 填入 Bot Token 即可。

### 3. QQ 接入 (国内个人首选)
<img width="637" height="335" alt="image" src="https://github.com/user-attachments/assets/23d338d1-13a0-4bde-b8c6-8536b7f1c87a" />


**步骤 1：Docker 部署 NapCat**

```bash
docker run -d \
  --name napcat \
  -p 3001:3001 \
  -v $(pwd)/napcat/config:/app/napcat/config \
  mlikiowa/napcat-docker:latest

```

* `-d`：后台运行
* `-p 3001:3001`：映射端口
* `-v`：持久化配置

**步骤 2：配置 WebSocket**
在 `config/onebot11_<QQ号>.json` 中启用：

```json
"websocketServers": [
  {
    "enable": true,
    "host": "0.0.0.0",
    "port": 3001
  }
]

```

**步骤 3：安装并对接插件**

```bash
# 安装插件
openclaw plugins install @izhimu/qq

# 方式一：交互式 (输入 NapCat 地址)
openclaw onboard

# 方式二：命令行手动配置
openclaw config set channels.qq.wsUrl "ws://127.0.0.1:3001"
openclaw config set channels.qq.enabled true

# 重启服务
openclaw gateway restart

```

**扫码登录后**，在 QQ 给机器人发"你好"，收到回复即代表接入成功。

---

## 🧰 技能系统 (Skills System)

技能就像手机的 App Store。每个技能包含：**工具定义**、**配置模板**和**提示词指令**。

### 浏览与安装

```bash
# 搜索特定技能
openclaw skill search weather
openclaw skill search email

# 查看详情
openclaw skill info weather

# 安装
openclaw skill install weather

```

### 批量管理

```bash
# 导出配置
openclaw skill export > skills.yaml
# 在新机器导入
openclaw skill import skills.yaml

```

### 常用技能推荐

* **效率类**：`weather` (全球天气), `calendar` (日程提醒), `translator` (百语翻译), `currency` (实时汇率)。
* **开发类**：`github` (仓库管理), `docker` (镜像控制), `database` (SQL执行), `api-test` (接口测试)。
* **创作类**：`image-gen` (DALL-E/SD), `video-edit` (基础剪辑), `markdown` (格式排版), `ocr` (文字识别)。
* **数据类**：`excel` (清洗统计), `csv` (筛选转换), `json` (校验格式), `web-scraper` (数据抓取)。

---

## 🏗️ 创建自定义技能 (Custom Skill)

### 1. 技能结构

```text
my-skill/
├── SKILL.md          # 描述与配置元数据
├── tools.yaml        # 定义工具逻辑
└── config.yaml       # 用户配置项

```

### 2. 编写 SKILL.md

```markdown
---
name: my-skill
version: 1.0.0
author: Your Name
description: 简短描述这个技能的功能
---
# My Skill
说明技能用途。
## 配置
- API_KEY: 你的 API 密钥

```

### 3. 定义工具 (tools.yaml)

```yaml
tools:
  - name: my_tool
    description: 工具描述
    parameters:
      - name: query
        type: string
        required: true
    command: |
      curl -X GET "${BASE_URL}/api?q=${query}" -H "Authorization: Bearer ${API_KEY}"

```

### 4. 安装本地技能

```bash
openclaw skill install ./my-skill

```

---

## 🚀 实战案例 (Showcase)

* **邮件自动分类**：安装 Gmail 技能后，指令：“每天早上 9 点检查收件箱，来自客户的标为'重要'，营销邮件归档，需回复的生成待办清单发给我。”
* **代码审查助手**：结合 GitHub 和分析技能：“新 PR 时自动检查规范、Bug 和性能，生成报告并评论在 PR 上。”
* **数据报告生成**：使用 Excel 技能：“每周一从数据库提取数据，生成含趋势图的 Excel 报告并发送。”
* **多平台内容发布**：组合社交媒体技能：“发布文章至公众号、知乎、小红书，自动根据平台调优配图。”


---

## 🛒 技能市场 (Skill Marketplace)

OpenClaw 社区维护着一个不断增长的技能库：[clawdhub.com](https://clawhub.com)
目前已经拥有超过 **17,777** 条技能。

### 按类别浏览

| 类别 | 示例技能 | 解决的问题 |
| --- | --- | --- |
| 📧 **通讯接入** | Gmail, Outlook, Slack | 邮件管理、消息通知推送 |
| 📅 **效率工具** | Google Calendar, Todoist | 日程管理、任务追踪 |
| 🔍 **深度搜索** | Brave Search, Tavily | 实时网页搜索、信息检索 |
| 💻 **开发辅助** | GitHub, VS Code, Docker | 代码管理、环境调试与协助 |
| 📊 **数据分析** | GA4, GSC, Ahrefs | 流量分析、SEO 优化 |
| 📝 **内容处理** | Markdown, PDF Parser | 文档处理、格式转换 |
| 🌐 **浏览器控制** | Playwright, Puppeteer | 网页自动化浏览、数据抓取 |
| 🏠 **智能家居** | HomeAssistant | 控制灯光、温度及家用设备 |

---

## 🛠️ 进阶阶段一：编写自定义技能

如果社区技能无法满足需求，你可以轻松编写自己的技能。本质上，这只是创建一个 Markdown 文件，告诉 AI：“你现在具备了这项新能力，方法如下。”

### 最简技能示例

在路径 `~/clawd/skills/weather/SKILL.md` 创建文件：

```markdown
# 天气查询技能

## 能力说明
你可以查询任何城市的天气信息。

## 使用方法
通过调用 wttr.in API 获取数据：
curl "wttr.in/城市名?format=3"

示例：
curl "wttr.in/Beijing?format=3"

## 输出格式
用简洁的语言告知用户当前天气，包括温度和天气状况。

```

**特点：** 无需复杂的 SDK，无需注册流程，一个 Markdown 文件就是一个技能。保存后尝试问它：“今天北京天气怎么样？”——它会立即学会调用 API 并返回结果。

### 技能开发原则

* **SKILL.md 是核心**：清晰写出“能做什么”、“怎么做”和“输出格式”。
* **保持简单**：一个技能只专注做好一件事。
* **错误处理**：在文件中注明“如果失败该怎么办”。
* **安全说明**：涉及敏感操作的技能必须要求确认。

> 🐱 **小莫的感想**：孟健给我写的第一个自定义技能是“催他睡觉”——如果检测到他 23:00 后还在发消息，就用越来越严厉的语言提醒。现在这个技能已经在社区传开了。你看，有用的东西自然会传播。

---

## 🌐 进阶阶段二：多设备协作 (Nodes)

你的助理不应只运行在服务器里。通过 **Nodes（节点）** 系统，它可以同时“看到”手机摄像头、“控制”电脑浏览器、“访问”家庭智能设备。

### 什么是节点？

Node 是安装在其他设备上的轻量级客户端，将这些设备连接到你的主 OpenClaw 服务器：

* **手机节点**：拍照、获取位置、发送系统通知。
* **电脑节点**：截图、屏幕录制、控制浏览器。
* **树莓派节点**：控制智能家居硬件。

### 应用场景

1. **远程观察**：出差时问助理：“给我看看办公室电脑屏幕”，它会自动截图并发送给你。
2. **手机协同**：助理在手机弹出通知：“你 3 点有会，需要帮你打开链接吗？”，你点确认，手机便自动开启会议。
3. **硬件控制**：“关闭客厅灯” → 助理通过树莓派节点控制 HomeAssistant。

### 如何设置

1. **安装客户端**（在目标设备运行）：
```bash
# 电脑端安装
curl -fsSL https://openclaw.ai/install.sh | bash

```


*手机端请在 App Store 搜索 "OpenClaw"（目前支持 iOS）。*
2. **主服务器批准配对**：
```bash
openclaw nodes approve <设备名称>

```



---

## 🔒 进阶阶段三：安全检查清单

能力越大，责任越大。请务必核对以下安全事项：

### 服务器安全

* ✅ SSH 使用密钥认证，禁用密码登录
* ✅ 防火墙仅暴露必要端口 (22, 443)
* ✅ 系统定期更新：`sudo apt update && sudo apt upgrade`
* ✅ 以非 root 用户身份运行 OpenClaw
* ✅ 启用 `fail2ban` 防止暴力破解

### API 密钥安全

* ✅ 所有密钥存储在环境变量或 `.env` 文件中
* ✅ `.env` 已添加到 `.gitignore`
* ✅ 密钥定期更换（建议每 3 个月一次）
* ✅ 设置 API 使用限额，防止产生失控成本

### 行为与数据安全

* ✅ **SOUL.md** 设定“绝对禁止操作”清单
* ✅ 破坏性操作（删除、修改配置）必须人工确认
* ✅ 使用 `trash` 代替 `rm`（可恢复 > 不可恢复）
* ✅ 定期备份工作目录 `~/clawd/`

---

## 🌍 社区资源

* **GitHub 主页**：[github.com/openclaw/openclaw](https://github.com/openclaw/openclaw)（2 天突破 10 万星，提交 Bug 或贡献代码）。
* **Discord 社区**：官方讨论区 [discord.com/invite/clawd](https://discord.com/invite/clawd)。
* **ClawHub 技能市场**：[clawhub.com](https://clawhub.com)。
* **即刻/小红书**：搜索 OpenClaw 相关话题。



---

## 🔮 未来展望

2026 年初仅仅是个开始，未来的 OpenClaw 将：

1. **模型更强**：随着 Claude、GPT 迭代，助理将自动变得更聪明、理解力更高。
2. **成本更低**：API 费用预计会从每月 $30 降至 $3 左右，实现全民普惠。
3. **多模态标配**：实时视觉分析、自然语音对话、甚至控制机器人执行物理任务。
4. **代理协作网络**：你将拥有一支“特工团队”，由“管家 Agent”协调专业 Agent（代码、分析、邮件）为你工作。

### 你的先发优势

**越早开始，优势越大。**
你今天打造的助理每天都在积累关于你的认知。一个用了 6 个月的助理和刚建成的助理，差距不是时间，而是**认知积累**。它知道你的习惯、偏好和处理问题的方法，这些只能靠时间沉淀。

**不要等待完美版本，现在就是开始的最佳时机。**

---

