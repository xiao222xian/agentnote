# 🦞 OpenClaw: 自主式 AI Agent 巅峰之作

2026 年初，一个名为 **OpenClaw** 的开源项目在 GitHub 上创造了历史：**两个月内获得超过 247,000 星标**，超越 Linux 成为全球增长最快的开源项目。

> **核心本质**：ChatGPT 是顾问，OpenClaw 是执行者。它是一个真正的**自主式 AI Agent**。

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
