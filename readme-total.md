# RAG 核心知识点与项目实操整理
## 2.2 RAG 核心模块与实操要点
### 01-03  准备工作
### 04 DataLoad
数据加载模块（无额外说明，附相关示意图）
### 05 Text Chunking
文本分块核心内容：**讲解分块的核心方法与实操步骤**
### 06 Vector Embedding
向量嵌入核心要求：**实现文本的向量嵌入转换**
### 07 Multimodel Embedding
多模态向量嵌入：**支持多模态数据的向量嵌入处理**
### 08 Vector DB
向量数据库模块：**列举主流向量数据库类型**（附相关示意图）
### 09 Milvus
专项讲解：**Milvus 向量数据库的使用与实操**
### 10 Index Optimization
索引优化核心：**基于上下文拓展实现索引优化**
### 11 Hybrid Search
混合检索（Hybrid Search）：结合**稀疏向量（Sparse Vectors）** 和**密集向量（Dense Vectors）** 优势的先进搜索技术
- 核心组成：BM25 检索（关键词检索）+ 向量检索
### 12 Query Construction
查询构建实操步骤：
1. 配置元数据字段
2. 创建自查询检索器
3. 执行目标查询
### 13 Text2SQL
功能说明：利用大语言模型（LLM）将用户自然语言问题，直接转换为可在数据库执行的SQL查询语句。
### 14 Query Rewriting
查询重构与分发：核心包含**查询翻译**与**查询路由**两大能力
### 15 Advanced Retrieval Technique
高级检索技术，包含**重排、压缩、校正**三大核心方向（重排模块附相关示意图）
#### 重排
（附相关示意图，无额外实操步骤说明）
#### 压缩
压缩检索实操流程（基于FAISS+LangChain）：
1. 创建基础组件：搭建标准 FAISS 向量存储，创建基础检索器 `base_retriever`，从向量库初步召回20个候选相关文档。
2. 准备处理单元：实例化两个核心处理模块
   - `reranker`：自定义 ColBERTReranker 实例
   - `compressor`：LangChain 内置 LLMChainExtractor，提取文档中与查询相关的句子
3. 构建处理管道：创建 `DocumentCompressorPipeline` 实例，将 `reranker` 和 `compressor` 按顺序加入 `transformers` 列表，文档先重排、后压缩（按管道源码依次执行处理器）。
4. 组装最终检索器：基于处理管道封装可直接调用的检索器。
#### 校正
校正检索（Corrective-RAG, C-RAG）：核心工作流程为**检索-评估-行动** 三阶段
### 16 Formatted Generation
格式化生成：**实现检索结果的标准化、结构化生成输出**
### 18 System Evaluation
系统评估：**RAG 整体系统的评估方法与介绍**
### 19 CommonTool
通用工具模块（附相关示意图，无额外说明）
### 20 KG RAG
基于知识图谱的RAG：**知识图谱与RAG融合的实操与应用**

## 实战项目：howtocook 项目
### 核心实施流程
1. 项目结构设计
2. 数据准备工作
3. 索引构建与检索优化
4. 生成集成与系统整合

## 拓展实操：图RAG 实现方案
### 第二种图结构做法 - 图RAG 全流程实操
1. 图RAG系统架构设计与环境配置
2. 图数据建模与Neo4j 集成
3. Milvus 索引构建
4. 图检索与RAG融合实操
