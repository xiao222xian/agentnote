
# 大模型 1v1 辅导 · 阶段 2（RAG & Agent）

> 本阶段目标：系统掌握 LangChain / LangGraph 基础用法，完成从 0 到 1 的 RAG 搭建，理解 MCP 实战，并能够独立开发一个具备多工具调用、记忆与规划能力的 Agent。

---

## 目录

- [阶段说明](#阶段说明)
- [学习路径](#学习路径)
- [学习时长](#学习时长)
- [学习内容](#学习内容)
  - [1. LangChain / LangGraph 工具学习](#1-langchain--langgraph-工具学习)
  - [2. RAG 从基础到进阶](#2-rag-从基础到进阶)
  - [3. Agent 实战](#3-agent-实战)
- [怎么学（学前必读）](#怎么学学前必读)
  - [一、LangChain 工具学习](#一langchain-工具学习)
  - [二、RAG 从基础到进阶](#二rag-从基础到进阶)
  - [三、从零到一手撕 Agent](#三从零到一手撕-agent)
- [学习重点（必学）](#学习重点必学)
- [学完达到的效果](#学完达到的效果)
- [学习产出](#学习产出)
- [推荐学习顺序](#推荐学习顺序)
- [两周学习计划（打卡版）](#两周学习计划打卡版)
- [实战作业](#实战作业)
- [面试作业](#面试作业)
- [其他注意事项](#其他注意事项)
- [阶段自查清单](#阶段自查清单)
- [阶段目标总结](#阶段目标总结)

---

## 阶段说明

本阶段进入 **RAG 与 Agent 实战阶段**。  
和阶段 1 不同，这一阶段重点不再只是“理解原理”，而是强调：

- 会搭建 LangChain / LangGraph 基础流程
- 会从 0 到 1 实现一个可用的 RAG 系统
- 会理解 MCP 的应用方式
- 会独立实现一个可执行复杂任务的 Agent

本阶段更强调：

- 工程感
- 调试能力
- 框架理解
- 方案选型能力

---

## 学习路径

**LangChain 工具学习 → 从 0 到 1 手撕 RAG → MCP 实战 → 从 0 到 1 手撕 Agent**

---

## 学习时长

- 建议学习周期：**2 周**
- 如果中途有课程、工作或面试冲突，请及时在 **1v1 群** 中反馈，便于调整节奏

---

## 学习内容

## 1. LangChain / LangGraph 工具学习

- 【官方教程】  
  https://larryl93.github.io/dive-into-langgraph-plus/  
  > 只需要掌握基本用法

---

## 2. RAG 从基础到进阶

- 【学习视频】  
  https://www.bilibili.com/video/BV1pZ421g7iv  
  > 基本原理

- 【文档教程】  
  https://github.com/Steven-Luo/MasteringRAG  
  > 图文代码结合

- 【配套代码】  
  https://github.com/langchain-ai/rag-from-scratch  
  > 视频配套源码

---

## 3. Agent 实战

- 【Agent Skills】  
  https://www.bilibili.com/video/BV1gv6eBZErD  
  > 吴恩达新课

- 【Agent 案例实战】  
  https://github.com/datawhalechina/hello-agents  
  > DeepResearch 方向案例

---

# 怎么学（学前必读）

## 一、LangChain 工具学习

LangChain 学习的目标不是“背 API”，而是建立一种 **LLM 应用编排思维**。

### 环境与模型建议

LangChain 先把环境装好，并配置一个可用的大模型 API。

对于国内同学，可以选：

- Qwen
- DeepSeek
- 兼容 OpenAI 协议的国内代理服务

如果你有稳定代理，也可以直接接：

- GPT
- Claude

如果你希望接口尽量统一、少折腾，也可以用兼容 OpenAI 接口的服务。

### 学习要求

对于 LangChain，重点不是把每个 API 记住，而是做到下面几点：

- 知道 LangChain / LangGraph 的核心组件有哪些
- 知道 Prompt、Chain、Tool、Agent、Memory 分别是什么
- 会用即可，不要求死记参数
- 会查文档，知道遇到问题该去哪里找

### 学习建议

建议先过一遍入门教程，跟着里面的代码例子跑一遍，重点理解：

- Prompt 模板
- Output Parser
- Tool 调用
- Agent 的基础流程
- LangGraph 的节点与边
- State 是怎么传递的

对于视频学习，也可以参考：

https://www.bilibili.com/video/BV1pRiWB8EXy

> 可选观看。LangChain 较新、系统又完整的教程不多，这个版本相对适合作为补充。  
> 注意版本变化较快，代码不兼容时要学会降版本或对照新文档修改。

### 备注

- LangChain 更新非常快
- 很多老教程已经过时
- 学习时一定注意：
  - 版本号
  - API 是否已迁移
  - 文档更新时间

---

### 国内接口补充说明

#### 如果你连不上 GPT / GPT-4o / GPT-4o1

可使用兼容 OpenAI 协议的国内服务替代，只要接口兼容即可。

#### 如果使用 DeepSeek 接口

- 官网：  
  https://www.deepseek.com/

- 文档参考：  
  https://blog.csdn.net/wgggfjy/article/details/134169203

> 注意：DeepSeek 不支持 OpenAI Embedding。  
> 如果要做向量化，需要换成其他 embedding 模型或 embedding 服务。

---

## 二、RAG 从基础到进阶

这一部分要求你从“会用 RAG”进一步升级到“知道 RAG 为什么这样做”。

### 推荐学习方式

视频与文档二选一或都看：

- YouTube 播放列表：  
  https://www.youtube.com/playlist?list=PLfalDFEXuae2LXbO1_PkyVJiQ23ZztA0x

- 配套图文仓库：  
  https://github.com/Steven-Luo/MasteringRAG/tree/main?tab=readme-ov-file

建议流程：

1. 先看视频或文字，理解整体流程
2. 跑配套代码
3. 自己再梳理一遍
4. 最后总结常见优化方法

### 重点学习内容

本阶段 RAG 重点不是只会“向量检索 + 问答”，而是必须理解以下高级玩法：

- RAG Fusion
- Decomposition
- HyDE
- Routing

### 为什么推荐文档仓库

当前国内很多 RAG 视频教程实际上是翻译内容，讲得不够清晰，有时甚至会有误差。

如果你更适合看图文，推荐深入看这个仓库，它对 RAG 的不同玩法解释较细，还配了代码：

https://github.com/Steven-Luo/MasteringRAG/tree/main?tab=readme-ov-file

重点看下面三个部分：

- 检索优化
- 文档分片优化
- RAG 评估

---

### 向量数据库部分要不要学？

要学，而且面试经常问。

即使你做的是上层应用，也需要理解向量数据库的底层原理，至少要知道：

- 什么是倒排索引
- 什么是 ANN（近似最近邻）
- 什么是 PQ / HNSW / LSH
- Faiss 是怎么工作的
- 向量数据库和全文搜索的差别在哪里

#### 推荐参考资料

- 向量数据库原理：  
  https://guangzhengli.com/blog/zh/vector-database/?continueFlag=0ab5f417fc4f5e7f838819b4dc745acf

- 各种 Vector DB 比较：  
  https://github.com/Steven-Luo/MasteringRAG/tree/main?tab=readme-ov-file

### 本阶段建议至少了解的向量库

- Faiss（重点）
- Elasticsearch（向量 + 检索结合）
- Chroma
- Milvus
- Qdrant

> 重点掌握 Faiss，并了解 HNSW、LSH 等原理即可。

---

## 三、从零到一手撕 Agent

这一部分要真正开始进入“智能体开发”。

### 第一阶段：Agent Skills 入门

建议先学习吴恩达新课，完成从 0 到 1 的 Agent Skills 开发。

这个资源非常适合之前没有系统做过 Agent 的同学，尤其适合：

- 没开发过 agent skills
- 不熟悉工具调用
- 不清楚 Agent 如何从“对话”变成“执行系统”的同学

特点：

- 基于 Anthropic 的范式
- 不依赖某一个复杂框架
- 很适合建立 Agent 开发第一感觉

---

### 第二阶段：Agent 实战案例

学完前面的技能后，再进入一个完整 Agent 案例项目。

推荐：

https://github.com/datawhalechina/hello-agents

这是一个很好的 Agent 实战系列，建议优先关注下面这些章节：

- 第 4 章
- 第 9 章
- 第 10 章

### 需要重点理解的内容

这里面和 Agent 开发强相关的核心内容包括：

- 三大经典范式：
  - ReAct
  - Plan-and-Solve
  - Reflection
- MCP
- A2A
- DeepResearch 类智能体

### DeepResearch 为什么值得学

因为它是近两年非常热门的方向，强调：

- 自动调用多个工具
- 多轮搜索与信息采集
- 自主规划
- 任务分解
- 报告生成

这是非常典型的“复杂智能体”范式。

### 学习要求

对于这个案例，重点学习后端部分即可，重点看：

- 三个模块
  - 任务规划
  - 任务总结
  - 报告生成

要求你：

- 看懂架构图
- 看懂流程图
- 看懂数据流
- 理解前后端如何协同

前端不做强要求。  
如果有余力，可以尝试把部分代码跑通。

---

# 学习重点（必学）

## 1. LangChain 学习

- [ ] ReAct
- [ ] Memory
- [ ] 上下文工程

## 2. RAG 和 Agent 实战

- [ ] RAG Fusion
- [ ] Decomposition
- [ ] HyDE
- [ ] Routing
- [ ] 向量数据库原理
  - [ ] Faiss
  - [ ] HNSW
  - [ ] LSH

## 3. Agent 实战

- [ ] Agent 三大经典范式
- [ ] MCP 和 A2A 的概念
- [ ] DeepResearch 智能体

---

# 学完达到的效果

## 1. LangChain 学习

你应该能够：

- 掌握 LangChain / LangGraph 的基础用法
- 能基于 LangChain 搭建一个小型 Agent 系统
- 理解 Prompt、Tool、Memory、State 等核心模块

## 2. RAG 和 Agent 实战

你应该能够：

- 掌握 RAG 的基础和进阶玩法
- 对问答型系统的常见方案做到心中有数
- 能从 0 到 1 搭建一个完整的 RAG 流程

## 3. Agent 实战

你应该能够：

- 完成一个复杂智能体的基础开发
- 涉及多次工具调用、反思、规划等过程
- 看懂并解释一个 DeepResearch 风格智能体的核心架构

---

# 学习产出

本阶段必须完成以下学习产出：

- [ ] 用思维导图结构化记录本阶段核心内容，要求能复述
- [ ] 写一份学习周报，上传到【每周学习周报】目录

---

# 推荐学习顺序

1. 先学 **LangChain / LangGraph 基础**
2. 再学 **RAG 从基础到进阶**
3. 接着理解 **向量数据库与检索优化**
4. 最后进入 **Agent Skills 与 Agent 实战**

---

# 两周学习计划（打卡版）

## 第 1 周：LangChain + RAG 基础与进阶

### 目标
建立基于 LangChain 的应用开发认知，并完成 RAG 的系统学习。

### 任务
- [ ] 配置一个可用的大模型 API
- [ ] 跑通 LangChain 基础示例
- [ ] 学会 Prompt / Tool / Memory 基本用法
- [ ] 跑通一个基础 RAG Demo
- [ ] 理解 RAG Fusion、HyDE、Decomposition、Routing
- [ ] 阅读至少一篇向量数据库底层原理文章
- [ ] 理解 Faiss / HNSW / LSH 的基本概念

### 本周产出
- [ ] 一份 LangChain 核心组件笔记
- [ ] 一份 RAG 工作流图
- [ ] 一份向量检索原理总结

---

## 第 2 周：Agent 实战与复杂任务编排

### 目标
完成 Agent 能力搭建，理解多工具调用与复杂智能体工作流。

### 任务
- [ ] 学完 Agent Skills 课程
- [ ] 理解 ReAct / Plan-and-Solve / Reflection
- [ ] 阅读 hello-agents 项目核心章节
- [ ] 搞清 MCP 和 A2A 的区别
- [ ] 看懂 DeepResearch 智能体的后端模块设计
- [ ] 完成实战作业
- [ ] 完成面试作业整理

### 本周产出
- [ ] 一份 Agent 范式对比表
- [ ] 一份 DeepResearch 系统流程图
- [ ] 一份实战作业代码与运行截图
- [ ] 一份面试题回答笔记

---

# 实战作业

## 作业目标

任选一个你熟悉的 Agent 框架，完成一个 **数据探索任务型 Agent**。

可选框架包括但不限于：

- LangChain
- LangGraph
- MetaGPT
- Agently

---

## 数据说明

输入数据是一个 **800 多行的 CSV 表格数据**。

任务要求通过 **自然对话** 的形式完成数据分析与探索。  
本次作业至少要覆盖以下功能：

1. **数据 summary**  
   例如统计：
   - 均值
   - 方差
   - 最大值
   - 最小值
   - 缺失值情况

2. **对数据画图**  
   例如：
   - 柱状图
   - 散点图
   - Survival / 变量分布图

3. **利用 sklearn 完成一个预测任务**  
   使用训练好的模型进行预测，其中 `survive` 那一列为预测变量

---

## 重要要求

### 必须满足

- 以上功能必须尽量通过 **大模型 + Agent 调用工具** 来完成
- 不能完全绕开 LLM，纯脚本硬编码实现不算合格
- 每个功能都可以通过一次和 Agent 的自然对话触发
- 可以使用：
  - function calling
  - 自定义 tools
  - Python REPL Tool

### 数据集

- Titanic cleaned.csv（约 66 KB）
#### 数据预览



[下载完整 Excel 文件](./titanic_cleaned.csv)
### 模型要求

- 模型不限
- 可使用：
  - GPT-4
  - DeepSeek
  - 其他可稳定调用的模型

---

## 建议提交内容

建议最终提交至少包含以下内容：

```text
rag-agent-homework/
├── app.py 或 main.py
├── tools.py
├── prompts.py
├── notebook.ipynb（可选）
├── README.md
├── screenshots/
│   ├── summary.png
│   ├── plot.png
│   └── predict.png
└── data/
    └── Titanic cleaned.csv
````

---

## 建议实现步骤

### Step 1：准备环境

* 安装框架依赖
* 配置模型 API
* 准备 CSV 数据集

### Step 2：设计 Agent 工具

建议至少准备以下工具：

* 数据读取工具
* 数据统计 summary 工具
* 绘图工具
* 训练模型工具
* 预测工具

### Step 3：设计对话接口

让用户可以通过自然语言提出问题，例如：

* “帮我分析这份 Titanic 数据的基本统计信息”
* “给我画一下年龄分布图”
* “训练一个预测 survive 的模型并给出评估结果”

### Step 4：接入 Agent

可选：

* function calling
* Tool Calling
* Python REPL
* LangGraph 节点编排

### Step 5：输出结果

要求保存：

* 代码
* 运行截图
* 必要的说明文档

---

## 作业通过标准（建议）

至少满足以下几点：

* [ ] Agent 能正确读取 CSV
* [ ] Agent 能输出 summary 结果
* [ ] Agent 能生成至少 1 张图
* [ ] Agent 能完成一个 sklearn 预测任务
* [ ] Agent 的核心流程由 LLM 驱动，而不是纯手写脚本

---

# 面试作业

请整理并回答下面问题，要求做到：

* 不只是背定义
* 要能讲清原理
* 最好能结合工程经验
* 对拓展题要有自己的理解

## 题目列表

1. LangChain 有哪些优点和明显的缺点？
2. 什么是检索增强生成（RAG）？
3. 在做知识增强检索时，文本切分有哪些方法？
4. 目前主流的中文向量模型有哪些？
5. 除了基础的向量检索，你还知道哪些可以提升 RAG 检索质量的技术？
6. 什么时使用微调，什么时候使用 RAG？
7. 请详细解释 ReAct 框架。它是如何将思维链和行动结合起来，以完成复杂任务的？
8. 除了 ReAct 范式，Agent 还有什么新架构？
9. Self-RAG 是什么，Self-RAG 如何提升大语言模型的质量和准确性？（拓展题）
10. 什么是 Graph RAG？什么场景下，必须用 GraphRAG？（拓展题）
11. Memory 是 Agent 的一个关键模块。如何为 Agent 设计短期记忆和长期记忆系统？可以借助哪些外部工具或技术？（拓展题）

---

## 面试作业建议输出形式

建议至少准备两份：

### 1. 简答版

* 每题控制在 3 ~ 8 句话
* 适合面试现场快速回答

### 2. 长答版

* 按“定义 → 原理 → 工程实践 → 常见坑 / 边界”展开
* 适合复盘、输出笔记和二次整理

---

# 其他注意事项

1. LangChain 更新很快，很多教程会有版本兼容问题，遇到问题先查版本。
2. RAG 不要只停留在“向量库 + topk 检索”，一定要学会优化与评估。
3. Agent 部分不要只会调 API，要会看流程图、状态流和数据流。
4. 本阶段要有明显工程产出：

   * 跑通 Demo
   * 写出代码
   * 保留截图
   * 记录排障过程
5. 遇到问题要学会借助：

   * DeepSeek
   * GPT
     作为工程辅助排查工具。

---

# 阶段自查清单

在进入下一阶段前，你至少要能回答下面这些问题：

* [ ] LangChain 和 LangGraph 的核心区别是什么？
* [ ] ReAct、Plan-and-Solve、Reflection 的差异是什么？
* [ ] RAG 的完整链路包含哪些步骤？
* [ ] 文本切分为什么会直接影响检索质量？
* [ ] RAG Fusion、HyDE、Routing 分别解决什么问题？
* [ ] 什么是 Faiss？什么是 ANN？
* [ ] HNSW 与 LSH 分别适合什么场景？
* [ ] 什么是 MCP？什么是 A2A？
* [ ] DeepResearch 类智能体和普通聊天机器人最大的区别是什么？
* [ ] 你是否已经完成一个可执行数据探索任务的 Agent？
* [ ] 你是否能独立讲清楚一个 RAG 系统的优化思路？

---

# 阶段目标总结

完成本阶段后，你应该具备下面这些能力：

* 能用 LangChain / LangGraph 搭建一个基础 LLM 应用
* 能从 0 到 1 搭建并优化一个 RAG 系统
* 能理解向量数据库与检索优化的基本原理
* 能开发一个具备多工具调用、记忆、规划能力的 Agent
* 能看懂并解释 DeepResearch 类复杂智能体的工作流
* 能为下一阶段的多智能体、复杂系统设计和生产级 LLM 应用打下基础

---

## 备注

* 本阶段最重要的不是“学了多少框架”，而是“能不能独立做出东西”
* 不要求你把所有 LangChain API 背下来，但要求你能查、能改、能跑
* 不要求你完全掌握所有向量库，但要求你理解核心原理和常见选型
* 不要求你把 Agent 项目复刻到 100%，但要求你看懂核心架构并能复述

```
```
