# 📚 Tesla 车书智能问答系统（RAG）

> 基于 **RAG（检索增强生成）** 架构的垂域问答系统，针对 Tesla Model 3 用户手册（~250页PDF）构建，支持精准问答、答案溯源（引用页码+关联图片）、端到端评估。

---

## 📑 目录

- [项目概述](#项目概述)
- [系统架构](#系统架构)
- [核心技术知识点](#核心技术知识点)
  - [1. PDF 文档解析](#1-pdf-文档解析)
  - [2. LLM 文本清洗](#2-llm-文本清洗)
  - [3. 语义切分与父子文档](#3-语义切分与父子文档)
  - [4. BM25 稀疏检索](#4-bm25-稀疏检索)
  - [5. Milvus 混合检索](#5-milvus-混合检索)
  - [6. FAISS 稠密检索](#6-faiss-稠密检索)
  - [7. 去重与父文档扩展](#7-去重与父文档扩展)
  - [8. Rerank 精排](#8-rerank-精排)
  - [9. LLM 答案生成](#9-llm-答案生成)
  - [10. 后处理（引用溯源）](#10-后处理引用溯源)
  - [11. HyDE 查询扩展](#11-hyde-查询扩展)
  - [12. QA 数据飞轮](#12-qa-数据飞轮)
  - [13. SFT 微调数据生成](#13-sft-微调数据生成)
  - [14. LLM 微调（LoRA + 量化）](#14-llm-微调lora--量化)
  - [15. 评估体系](#15-评估体系)
- [项目目录结构](#项目目录结构)
- [完整请求流程示例](#完整请求流程示例)
- [技术栈总览](#技术栈总览)
- [学习笔记与心得](#学习笔记与心得)

---

## 项目概述

### 背景

用户拿到一本 Tesla Model 3 用户手册（PDF，约250页），希望能用自然语言提问，系统精准回答并给出出处。

| 用户问题 | 期望回答 |
|---------|---------|
| "离车后自动上锁功能怎么用" | 准确回答 + 引用页码(第7页) + 关联图片 |
| "Model 3 支持哪些钥匙类型" | 从手册中检索并综合作答 |
| "怎么调节后视镜" | 检索相关段落，生成回答 |

### 为什么选 RAG？

| 方案 | 优点 | 缺点 |
|------|------|------|
| 整本丢给LLM | 简单 | Token超限(~20万字)、贵、幻觉 |
| 纯微调LLM | 无需在线检索 | 无法溯源、更新手册需重训 |
| **RAG** ✅ | 按需检索+LLM生成，可溯源、可更新 | 需要搭检索管线 |

---

## 系统架构

### 离线建库阶段

```
Tesla_Manual.pdf
    │
    ▼
① PDF解析 (PyMuPDF) → 逐页提取文字+图片 → raw_docs.pkl
    │
    ▼
② LLM文本清洗 (豆包API, 20线程) → 去乱码、整句通顺 → clean_docs.pkl
    │
    ▼
③ 语义切分 + 父子文档构建
    ├─ 语义聚类 (text2vec + AgglomerativeClustering)
    ├─ 父文档 (≤512 token) → 存 MongoDB
    └─ 子文档 (256 token, overlap 50) → 存 MongoDB, 记录 parent_id
    │                                              → split_docs.pkl
    ▼
④ 建索引
    ├─ BM25 (jieba分词 + 停用词) → bm25retriever.pkl
    └─ Milvus (BGE-M3 稠密+稀疏向量) → milvus.db
```

### 在线问答阶段

```
用户问题: "离车后自动上锁功能怎么用"
    │
    ▼
⑤ 多路召回
    ├─ BM25 → Top 10 (关键词匹配)
    └─ Milvus Hybrid → Top 10 (稠密+稀疏+RRF融合)
    │
    ▼
⑥ 去重合并 + 父文档扩展
    │  子文档 → 查 MongoDB parent_id → 替换为父文档
    │
    ▼
⑦ Rerank 精排 (BGE-Reranker-v2-M3, Cross-Encoder) → Top 5
    │
    ▼
⑧ LLM 生成答案 (Qwen3-8B LoRA微调, vLLM部署)
    │  Prompt: 提供编号上下文 → 输出答案+引用编号
    │
    ▼
⑨ 后处理
    │  正则提取引用编号 【1,2,3】 → 映射到页码+图片
    │
    ▼
最终输出: { answer, cite_pages, related_images }
```

---

## 核心技术知识点

### 1. PDF 文档解析

**涉及知识点**：PyMuPDF(fitz)、PDF结构、图片提取、启发式标题检测

#### 1.1 解析逻辑

```python
# src/parser/pdf_parse.py
def load_pdf():
    pdf = fitz.open(file_path)
    for page_num in range(len(pdf)):
        # 过滤封面目录（前4页）和附录（247页后）
        if idx < 4 or idx > 247: continue

        page = pdf.load_page(page_num)
        # 裁剪底部50px（去除页脚噪声）
        crop = fitz.Rect(0, 0, page.rect.width, page.rect.height - 50)
        text = page.get_text(clip=crop)

        # 提取页面图片
        images = page.get_images(full=True)
        # 对每张图片做标题检测
        ...
```

#### 1.2 图片标题检测

```python
# src/parser/image_handler.py
# 启发式打分：字体大小(权重高) + 是否加粗(权重中) + 位置(权重低)
# 过滤小于34px的图标
```

#### 1.3 关键知识点

- **PyMuPDF (fitz)**：Python PDF解析库，支持文字提取、图片提取、页面裁剪
- **PDF 结构**：PDF是由页面(Page)组成，每页包含文本块(TextBlock)和图像(Image)
- **噪声过滤**：页眉页脚、水印等非正文内容需要裁剪去除
- **启发式方法**：基于规则的打分策略，适合处理半结构化文档

---

### 2. LLM 文本清洗

**涉及知识点**：LLM数据清洗、并发处理、Prompt Engineering

#### 2.1 为什么需要清洗？

PDF提取的原始文本通常包含：
- 多余的换行符和空格
- 不通顺的断句（PDF按排版换行，不按语义）
- 无意义的特殊字符

#### 2.2 实现

```python
# src/client/llm_clean_client.py
LLM_CLEAN_PROMPT = """
你是一个专业的文档整理助手，请根据以下要求对文档进行处理：
1. 让句子变得更加通顺：重新整合句子、段落，去除不必要的符号
2. 按标题归类整理：按语义关系，把属于同一标题下的文档做归类合并，标题用markdown###加粗
"""

# 20线程并发清洗
MAX_WORKERS = 20
def request_llm_clean(docs):
    with concurrent.futures.ThreadPoolExecutor(max_workers=MAX_WORKERS) as executor:
        futures = {doc.id: executor.submit(chat, PROMPT.format(doc.page_content))
                   for doc in docs}
```

#### 2.3 关键知识点

- **LLM 做数据清洗**：利用 LLM 的语义理解能力整理非结构化文本
- **`concurrent.futures.ThreadPoolExecutor`**：20线程并发调用API，加速清洗
- **`more_itertools.divide`**：将文档分组后并发处理

---

### 3. 语义切分与父子文档

**涉及知识点**：文本切分、语义聚类、凝聚层次聚类、Parent-Child Document、FastAPI微服务

这是**项目的核心创新点**。

#### 3.1 为什么不用固定长度切分？

| 切分方式 | 优点 | 缺点 |
|---------|------|------|
| 固定长度（每256字切一刀） | 简单 | 会把一个完整概念从中间割裂 |
| 按标点切 | 保证句子完整 | 相关句子可能分到不同chunk |
| **语义聚类切分** ✅ | 语义相关的句子聚在一起 | 需要embedding+聚类，有计算开销 |

#### 3.2 语义切分服务实现

```python
# src/server/semantic_chunk.py (FastAPI 微服务)
@app.post("/v1/semantic-chunks")
def semantic_chunks(sentences, group_size):
    # 1. 计算每个句子的 embedding (text2vec-base-chinese)
    embeddings = model.encode(sentences)

    # 2. 计算最佳聚类数
    n_clusters = max(1, len(sentences) // group_size)

    # 3. 凝聚层次聚类 (cosine距离)
    clustering = AgglomerativeClustering(
        n_clusters=n_clusters,
        metric='cosine',
        linkage='average'
    )
    labels = clustering.fit_predict(embeddings)

    # 4. 按聚类结果合并句子
    # 5. Markdown标题 ### 作为强制分割边界
    # 6. 小于50字符的chunk合并到相邻块
```

#### 3.3 父子文档机制（核心！）

```
✅ 为什么要父子文档？解决"检索精度" vs "上下文完整性"的矛盾

检索需要精确 → 小chunk(256 token)匹配更准
LLM需要上下文 → 大chunk(512 token)信息更完整

解决方案：
┌─────────────────────────────────────┐
│  父文档（512 token）               │ ← 存MongoDB，给LLM用
│  ┌──────────┐  ┌──────────┐       │
│  │ 子文档1   │  │ 子文档2   │      │ ← 存向量索引，给检索用
│  │(256 token)│  │(256 token)│      │
│  │parent_id →│  │parent_id →│──────│ ← 通过parent_id回溯父文档
│  └──────────┘  └──────────┘       │
└─────────────────────────────────────┘

流程：
1. 检索阶段：用子文档检索（粒度细，匹配精度高）
2. 命中后：通过 parent_id 查 MongoDB 取回父文档
3. 送入LLM：用父文档作为上下文（信息完整）
```

#### 3.4 代码实现

```python
# src/parser/pdf_parse.py
def texts_split(raw_docs):
    for doc in raw_docs:
        # 1. 语义聚类切分 → 得到若干语义块
        grouped_chunks = request_semantic_chunk(doc.page_content, group_size=10)

        # 2. 每个语义块 = 父文档
        for group in grouped_chunks:
            parent_id = hashlib.md5(group.encode()).hexdigest()
            parent_doc = Document(page_content=group, metadata={"unique_id": parent_id, ...})
            save_2_mongo(parent_docs)  # 存MongoDB

        # 3. 父文档再切 → 子文档（带 parent_id）
        for parent in parent_docs:
            child_docs = text_splitter.create_documents([parent.page_content])
            for child in child_docs:
                child.metadata["parent_id"] = parent.metadata["unique_id"]
            save_2_mongo(child_docs)  # 也存MongoDB
```

#### 3.5 关键知识点

- **RecursiveCharacterTextSplitter**：LangChain 的递归文本切分器，按 `["\n\n", "\n"]` 优先级递归切
- **chunk_size 与 chunk_overlap**：chunk 太大召回不准、太小信息不全；overlap 保证边界信息不丢失
- **AgglomerativeClustering（凝聚层次聚类）**：自底向上合并最相似的样本，适合文本分组
- **Cosine 距离**：计算向量夹角，常用于文本相似度
- **tiktoken (cl100k_base)**：OpenAI 的 token 计数器，用于精确控制chunk长度
- **MongoDB Upsert**：`update_one(..., upsert=True)`，存在则更新，不存在则插入

---

### 4. BM25 稀疏检索

**涉及知识点**：BM25算法、TF-IDF、jieba分词、停用词、稀疏检索

#### 4.1 BM25 算法原理

$$\text{BM25}(q, d) = \sum_{t \in q} \text{IDF}(t) \cdot \frac{f(t,d) \cdot (k_1 + 1)}{f(t,d) + k_1 \cdot (1 - b + b \cdot \frac{|d|}{avgdl})}$$

其中：
- $f(t,d)$：词 $t$ 在文档 $d$ 中的词频
- $|d|$：文档长度
- $avgdl$：平均文档长度
- $k_1$：词频饱和参数（默认1.2-2.0）
- $b$：文档长度归一化参数（默认0.75）

**BM25 vs TF-IDF**：
- TF-IDF 的 TF 是线性增长的（出现10次 = 10倍权重）
- BM25 的 TF 有**饱和度**（出现10次 ≈ 出现5次，因为 $k_1$ 控制了上限）
- BM25 加入了**文档长度归一化**（长文档不会天然占优）

#### 4.2 实现

```python
# src/retriever/bm25_retriever.py
class BM25:
    def __init__(self, docs):
        self.retriever = BM25Retriever.from_documents(
            docs,
            preprocess_func=self.tokenize  # 自定义中文分词
        )

    def tokenize(self, text):
        tokens = jieba.lcut(text)
        return [t for t in tokens if t not in _stopwords]

    def retrieve_topk(self, query, topk=10):
        self.retriever.k = topk
        return self.retriever.get_relevant_documents(query)
```

#### 4.3 关键知识点

- **BM25**：经典稀疏检索算法，基于词频统计，工业界广泛使用
- **jieba 分词**：中文分词工具，支持精确模式、搜索模式
- **停用词（Stopwords）**：过滤"的""了""在"等高频无意义词
- **稀疏检索 vs 稠密检索**：稀疏=基于词匹配（BM25/TF-IDF），稠密=基于向量相似度（FAISS/Milvus）
- **Pickle 持久化**：用 `pickle.dump/load` 缓存索引，避免重复构建

---

### 5. Milvus 混合检索

**涉及知识点**：向量数据库、BGE-M3、稠密向量、稀疏向量、混合检索、RRF融合排序

#### 5.1 BGE-M3 模型

BGE-M3 是 BAAI 发布的多功能 Embedding 模型，**一次编码同时输出**：
- **稠密向量（Dense）**：768维浮点向量，捕捉语义相似性
- **稀疏向量（Sparse）**：高维稀疏向量，捕捉关键词匹配

#### 5.2 三种检索模式

```python
# src/retriever/milvus_retriever.py

# 1. 纯稠密检索（语义匹配）
def dense_search(self, query_dense_embedding, limit):
    res = self.col.search([query_dense_embedding], anns_field="dense_vector",
                          limit=limit, param={"metric_type": "IP"})

# 2. 纯稀疏检索（关键词匹配）
def sparse_search(self, query_sparse_embedding, limit):
    res = self.col.search([query_sparse_embedding], anns_field="sparse_vector",
                          limit=limit, param={"metric_type": "IP"})

# 3. 混合检索（两路融合）✅ 实际使用的
def hybrid_search(self, query_dense, query_sparse, limit):
    dense_req = AnnSearchRequest([query_dense], "dense_vector", ...)
    sparse_req = AnnSearchRequest([query_sparse], "sparse_vector", ...)
    rerank = RRFRanker()  # RRF 融合排序
    res = self.col.hybrid_search([sparse_req, dense_req], rerank=rerank, limit=limit)
```

#### 5.3 RRF（Reciprocal Rank Fusion）

**多路检索结果融合算法**：

$$\text{RRF}(d) = \sum_{r \in R} \frac{1}{k + r(d)}$$

其中 $r(d)$ 是文档 $d$ 在检索结果 $r$ 中的排名，$k$ 是平滑参数（通常=60）。

**优势**：
- 不需要归一化不同检索器的分数（BM25分数和向量相似度量纲不同）
- 不需要手动调权重
- 比 WeightedRanker 更鲁棒

#### 5.4 索引构建

```python
class MilvusRetriever:
    def __init__(self, docs):
        # Schema定义
        fields = [
            FieldSchema(name="unique_id", dtype=VARCHAR, is_primary=True),
            FieldSchema(name="text", dtype=VARCHAR, max_length=512),
            FieldSchema(name="sparse_vector", dtype=SPARSE_FLOAT_VECTOR),
            FieldSchema(name="dense_vector", dtype=FLOAT_VECTOR, dim=768),
        ]

        # 创建索引
        sparse_index = {"index_type": "SPARSE_INVERTED_INDEX", "metric_type": "IP"}
        dense_index = {"index_type": "AUTOINDEX", "metric_type": "IP"}

    def save_vectorstore(self, docs):
        # BGE-M3 编码
        texts_embeddings = embedding_handler(raw_texts)  # 同时输出dense和sparse
        # 批量插入Milvus
        self.col.insert(batched_entities)
```

#### 5.5 关键知识点

- **Milvus**：开源向量数据库，原生支持稠密/稀疏/混合检索
- **BGE-M3**：BAAI出品，Multi-Functionality（稠密+稀疏+ColBERT三合一）
- **IP（Inner Product）**：内积相似度，向量点积越大越相似
- **Cosine Similarity vs IP**：归一化后的向量，IP = Cosine
- **SPARSE_INVERTED_INDEX**：稀疏向量的倒排索引
- **AUTOINDEX**：Milvus 自动选择最优索引类型
- **RRF（Reciprocal Rank Fusion）**：基于排名倒数的融合算法，不需要分数归一化
- **Batch Embedding**：分批编码（每批50条），避免显存溢出

---

### 6. FAISS 稠密检索

**涉及知识点**：FAISS、BCE Embedding、向量相似度搜索

```python
# src/retriever/faiss_retriever.py
class FaissRetriever:
    def __init__(self, docs):
        self.embeddings = HuggingFaceEmbeddings(model_name=bce_model_path, device="cuda")
        self.vector_store = FAISS.from_documents(docs, self.embeddings)
        self.vector_store.save_local(faiss_db_path)  # 持久化
        del self.embeddings; torch.cuda.empty_cache()  # 释放显存

    def retrieve_topk(self, query, topk):
        return self.vector_store.similarity_search_with_score(query, k=topk)
```

**FAISS vs Milvus**：

| 对比 | FAISS | Milvus |
|------|-------|--------|
| 类型 | 向量检索库 | 向量数据库 |
| 稀疏向量 | ❌ | ✅ |
| 混合检索 | ❌ | ✅ |
| RRF融合 | ❌ | ✅ |
| 持久化 | 文件存储 | 数据库级别 |
| 适用场景 | 单模型原型验证 | 生产环境多模态检索 |

---

### 7. 去重与父文档扩展

**涉及知识点**：检索结果融合、去重策略、MongoDB关联查询

```python
# src/utils.py
def merge_docs(docs1, docs2):
    merged_docs, merged_ids = [], set()
    for doc in docs1 + docs2:
        parent_id = doc.metadata.get("parent_id")
        if parent_id:
            # 子文档 → 查MongoDB取父文档
            parent = manual_collection.find_one({"unique_id": parent_id})
            unique_id = parent["unique_id"]
            if unique_id not in merged_ids:
                merged_ids.add(unique_id)
                merged_docs.append(Document(page_content=parent["page_content"], ...))
        else:
            # 已经是父文档 → 直接去重
            unique_id = doc.metadata.get("unique_id")
            if unique_id not in merged_ids:
                merged_ids.add(unique_id)
                merged_docs.append(doc)
    return merged_docs
```

**关键逻辑**：
- BM25 和 Milvus 的结果可能有重叠 → 按 `unique_id` 去重
- 子文档不直接给LLM → 回溯 `parent_id` 取父文档
- **set 去重**：O(1) 查重效率

---

### 8. Rerank 精排

**涉及知识点**：Cross-Encoder、Bi-Encoder、精排vs粗排、BGE-Reranker、Qwen3-Reranker

#### 8.1 为什么需要 Rerank？

```
粗排阶段（BM25/Milvus）：
  - 方式：Bi-Encoder（query和doc分别编码，计算相似度）
  - 优点：速度快（可以预计算doc向量）
  - 缺点：query和doc独立编码，无法捕捉细粒度交互

精排阶段（Reranker）：
  - 方式：Cross-Encoder（query和doc拼接后联合编码）
  - 优点：直接建模query-doc交互，精度高
  - 缺点：慢（每个(query,doc)对都要过一遍模型）

所以：粗排用Bi-Encoder快速缩小范围 → 精排用Cross-Encoder精确排序
```

#### 8.2 BGE-M3 Reranker 实现

```python
# src/reranker/bge_m3_reranker.py
class BGEM3ReRanker:
    def __init__(self, model_path):
        self.model = AutoModelForSequenceClassification.from_pretrained(model_path)
        self.model.eval().half().cuda()  # FP16 推理加速

    def rank(self, query, candidate_docs, topk=10):
        # 构造 (query, doc) 文本对
        pairs = [(query, doc.page_content) for doc in candidate_docs]

        # Tokenize + 推理
        inputs = self.tokenizer(pairs, padding=True, truncation=True, max_length=4096)
        scores = self.model(**inputs).logits  # 相关性得分

        # 按分数排序，取Top-K
        response = [doc for score, doc in sorted(zip(scores, docs), reverse=True)][:topk]
        return response
```

#### 8.3 Qwen3-Reranker（LLM-based 精排）

```python
# src/reranker/qwen3_reranker.py
# 不同于传统Cross-Encoder，用LLM做精排：
# 输入：query + doc
# 输出：生成 "yes" 或 "no" token 的概率
# P("yes") 越高，文档越相关

# 提取 "yes"/"no" token 的 logits
true_token_id = tokenizer("yes")
false_token_id = tokenizer("no")
score = logits[true_token_id] / (logits[true_token_id] + logits[false_token_id])
```

#### 8.4 关键知识点

- **Cross-Encoder vs Bi-Encoder**：面试高频考点
  - Bi-Encoder：query和doc分别编码，向量点积算相似度，**快但粗**
  - Cross-Encoder：query+doc拼接后联合编码，**慢但精**
- **AutoModelForSequenceClassification**：HuggingFace 的序列分类模型，输出相关性分数
- **FP16 推理**：`model.half()` 将模型参数从 float32 → float16，速度约2倍，精度损失极小
- **Reranker 微调**：在领域数据上微调，标签三级（0=不相关，1=部分相关，2=高度相关）

---

### 9. LLM 答案生成

**涉及知识点**：Qwen3-8B、vLLM、LoRA、INT4量化、Prompt设计、流式输出

#### 9.1 Prompt 设计

```python
# src/client/llm_local_client.py
LLM_CHAT_PROMPT = """
### 信息
{context}

### 任务
你是特斯拉电动汽车Model 3车型的用户手册问答系统，你具备{信息}中的知识。
请回答问题"{query}"，答案需要精准，语句通顺，并严格按照以下格式输出

{答案}【{引用编号1}, {引用编号2}, ...】
如果无法从中得到答案，请说 "无答案" ，不允许在答案中添加编造成分。
"""
```

**Prompt 设计要点**：
- **编号上下文**：每个文档段落带编号 `【1】...【2】...`
- **引用格式**：要求LLM在答案后标注引用编号 `【1,3】`
- **兜底指令**：无法回答时说"无答案"，防止幻觉
- **格式约束**：严格按照指定格式输出，方便后处理解析

#### 9.2 vLLM 推理

```python
# 本地部署 Qwen3-8B，通过 vLLM 暴露 OpenAI 兼容接口
llm_client = OpenAI(api_key="EMPTY", base_url="http://localhost:8000/v1")

completion = llm_client.chat.completions.create(
    model=qwen3_8b_tune_model_name,  # LoRA SFT + INT4 量化
    messages=[...],
    temperature=0.001,    # 接近确定性输出
    top_p=0.95,
    stream=True,           # 流式输出
    extra_body={
        "top_k": 1,
        "chat_template_kwargs": {"enable_thinking": False}  # 关闭思考模式
    }
)
```

#### 9.3 关键知识点

- **vLLM**：高性能LLM推理引擎，核心技术：PagedAttention（分页注意力）、Continuous Batching（连续批处理）
- **OpenAI 兼容接口**：vLLM 提供和 OpenAI 相同的 API 格式，方便切换
- **Temperature**：控制输出随机性。0.001≈确定性输出，适合问答任务
- **enable_thinking=False**：关闭 Qwen3 的思考模式（思考模式会先输出思考过程，不适合问答）

---

### 10. 后处理（引用溯源）

**涉及知识点**：正则表达式、答案溯源、结构化输出

```python
# src/utils.py
def post_processing(response, docs):
    # 1. 提取引用编号：【1, 2, 3】
    all_cites = re.findall("[【](.*?)[】]", response)
    cites = []
    for cite in all_cites:
        cite = [int(k) for k in cite.split("，") if k.isdigit()]
        cites.extend(cite)
    cites = list(set(cites))

    # 2. 清理答案文本（去掉引用标记）
    answer = re.sub("[【](.*?)[】]", "", response)

    # 3. 根据引用编号关联页码和图片
    related_images, pages = [], []
    for index in cites:
        images = docs[index-1].metadata["images_info"]
        pages.append(docs[index-1].metadata["page"])
        for image in images:
            if image["title"]:
                related_images.append(image)

    return {"answer": answer, "cite_pages": sorted(pages), "related_images": related_images}
```

**关键知识点**：
- **答案溯源（Citation）**：RAG 系统的核心价值之一，用户可以验证答案来源
- **正则表达式**：`re.findall("[【](.*?)[】]")` 提取中文方括号中的内容
- **结构化输出**：返回 answer + cite_pages + related_images，前端可做丰富展示

---

### 11. HyDE 查询扩展

**涉及知识点**：HyDE（Hypothetical Document Embeddings）、查询扩展、语义鸿沟

#### 11.1 什么是 HyDE？

**问题**：用户的 query 和文档的表述可能差别很大（语义鸿沟）

```
用户问："下车后车门会一直开着吗"
文档写："离车后自动上锁功能..."

直接检索可能匹配不上！
```

**HyDE 的做法**：先让 LLM 生成一个"假设性回答"，用这个回答去检索

```python
# src/client/llm_hyde_client.py
LLM_HYDE_PROMPT = """
你是一位Tesla汽车专家，请结合Model 3相关知识回答下列问题.
请给出用户问题的使用方法，详细分析问题原因，返回有用的内容。
{query}
最终的回答请尽可能的精简, 不超过100字:
"""

def request_hyde(query):
    result = LLM(HYDE_PROMPT.format(query=query))
    return result  # 假设性回答，用于扩展检索

# 使用：原始query + HyDE回答 拼接后检索
hyde_query = query + "\n" + request_hyde(query)
docs = retriever.retrieve_topk(hyde_query, topk=10)
```

#### 11.2 关键知识点

- **HyDE**：2022年提出的查询扩展技术，用LLM生成的假设文档弥补query-doc语义鸿沟
- **适用场景**：短query（如"自动泊车"）效果好，长query可能引入噪声
- **风险**：如果LLM生成了错误的假设文档，会把检索带偏
- **在本项目中**：通过 `HYDE=0/1` 开关控制，默认关闭

---

### 12. QA 数据飞轮

**涉及知识点**：数据增强、同义句生成、关键词抽取、数据质量评估、LLM数据生成

#### 12.1 数据生成流水线

```
clean_docs.pkl（清洗后文档）
    │
    ▼
Step 1: LLM 生成 QA（每段文档生成5个问答对）
    │  Prompt: "阅读文本，生成5个问题和答案"
    │  产出: qa_pair.json
    │
    ▼
Step 2: 同义句扩展（每题×5种问法）
    │  Prompt: "生成5个意思相近的问题"
    │  例："怎么打开车窗" → "这个车的窗子要怎么才能开启"
    │  产出: expand_qa_pair.json
    │
    ▼
Step 3: 数据切分（90%训练 / 10%测试）
    │  产出: train_qa_pair.json, test_qa_pair.json
    │
    ▼
Step 4: 关键词抽取（汽车领域术语）
    │  Prompt: "提取3-5个汽车领域关键词"
    │  例："行车记录仪,探测功能,辅助驾驶"
    │  产出: test_keywords_pair.json
    │
    ▼
Step 5: 质量评估（LLM打分1-5）
    │  Prompt: "对QA质量打分，好的问题是问事实/观点的..."
    │  过滤低质数据（如"这一段讲了什么"）
```

#### 12.2 关键知识点

- **数据飞轮（Data Flywheel）**：自动从数据生产更多数据，持续提升模型效果
- **同义句扩展**：增加数据多样性，提高模型泛化能力
- **LLM-as-Judge**：用LLM对数据质量打分，自动过滤低质量样本
- **Train/Test Split**：90/10 切分，保证评估的独立性

---

### 13. SFT 微调数据生成

**涉及知识点**：SFT数据格式、Rerank训练数据、三级标签、LLaMA-Factory

#### 13.1 两类训练数据

```python
# generate_sft_data.py

# A类：SFT 数据（用于微调 Qwen3-8B）
{
    "instruction": "### 信息\n1.xxx\n2.xxx\n### 任务\n请回答...",
    "input": "",
    "output": "答案内容【1,3】"
}

# B类：Rerank 数据（用于微调 BGE-Reranker）
{"query": "...", "content": "高度相关的文档", "label": 2}  # 正样本
{"query": "...", "content": "部分相关的文档", "label": 1}  # 中间样本
{"query": "...", "content": "不相关的文档",   "label": 0}  # 负样本
```

#### 13.2 Rerank 标签构造逻辑

| 标签 | 来源 | 含义 |
|------|------|------|
| **2** (高相关) | Reranker Top1 文档 | 和 query 最相关的文档 |
| **1** (部分相关) | Reranker 末位文档 | 被选中但排名靠后 |
| **0** (不相关) | 召回但未入选的文档 | 和 query 无关 |

#### 13.3 关键知识点

- **SFT（Supervised Fine-Tuning）**：在监督标注数据上微调LLM
- **Instruction Tuning 数据格式**：`{instruction, input, output}` 是 LLaMA-Factory 的标准格式
- **Hard Negative Mining**：用"召回但未入选"的文档作为负样本，比随机负样本更有区分度
- **三级标签**：比二分类（相关/不相关）更细致，能让 Reranker 学到排序信息

---

### 14. LLM 微调（LoRA + 量化）

**涉及知识点**：LoRA、INT4量化、LLaMA-Factory、vLLM部署

#### 14.1 LoRA 原理

$$W' = W_0 + \Delta W = W_0 + BA$$

其中 $B \in \mathbb{R}^{d \times r}$, $A \in \mathbb{R}^{r \times d}$, $r \ll d$

- $W_0$：原始预训练权重（冻结，不更新）
- $BA$：低秩矩阵，只训练这部分（参数量极小）
- $r$：秩，通常 4-64，越小参数量越少

**为什么用 LoRA？**
- Qwen3-8B 全参数微调需要 ~16GB+ 显存
- LoRA 只训练 <1% 的参数，单卡即可训练
- 配合 INT4 量化，推理显存需求也大幅降低

#### 14.2 INT4 量化

将模型权重从 FP32（32bit）→ INT4（4bit），模型体积缩小约8倍：

```
FP32: 8B params × 4 bytes = 32GB
INT4: 8B params × 0.5 bytes = 4GB  ← 单张消费级显卡可运行
```

#### 14.3 微调工具链

```
LLaMA-Factory
    │
    ├── 数据准备：summary_data/train.json (SFT格式)
    ├── 模型：Qwen3-8B
    ├── 方法：LoRA SFT
    ├── 量化：INT4 (GPTQ/AWQ)
    │
    ▼
  output/qwen3_lora_sft_int4/  ← 微调后的模型
    │
    ▼
  vLLM 部署 → localhost:8000  ← OpenAI 兼容 API
```

#### 14.4 关键知识点

- **LoRA（Low-Rank Adaptation）**：微调方法，只训练低秩矩阵，参数高效
- **INT4 量化**：将权重压缩到4bit，模型体积约缩小8倍
- **GPTQ/AWQ**：两种常用的量化算法
- **LLaMA-Factory**：开源 LLM 微调框架，支持 LoRA/QLoRA/全参
- **vLLM**：高性能 LLM 推理引擎，PagedAttention + Continuous Batching

---

### 15. 评估体系

**涉及知识点**：语义相似度、Jaccard系数、RAGAS框架、text2vec

#### 15.1 双层评估

**第一层：自研指标**

$$\text{Score} = 0.8 \times \text{SemanticSim} + 0.2 \times \text{KeywordJaccard}$$

```python
# final_score.py
# 语义相似度：text2vec-base-chinese 编码后计算 cosine
semantic_score = semantic_search(simModel.encode([gold]), simModel.encode(pred))[0][0]['score']

# 关键词覆盖率：Jaccard 系数
join_keywords = [word for word in keywords if word in pred]
keyword_score = len(join_keywords) / (len(keywords) + 1e-6)
keyword_score = 1 if keyword_score > 0.3 else 0  # 阈值二值化

# 加权
score = 0.8 * semantic_score + 0.2 * keyword_score
```

**第二层：RAGAS 框架**

```python
# RAGAS（Retrieval-Augmented Generation Assessment）
from ragas.metrics import LLMContextRecall, LLMContextPrecisionWithReference

# Context Recall：检索到的文档是否覆盖了答案需要的信息
# Context Precision：检索到的文档中有多少是真正有用的
result = evaluate(dataset, metrics=[LLMContextRecall(), LLMContextPrecisionWithReference()])
```

#### 15.2 特殊case处理

```python
# "无答案"的处理
if gold == "无答案" and pred != "无答案":
    score = 0.0  # 该回答"无答案"但没回答 → 0分
if gold == "无答案" and pred == "无答案":
    score = 1.0  # 正确拒答 → 满分
```

#### 15.3 关键知识点

- **语义相似度**：用 text2vec 编码后计算余弦相似度，比字面匹配更合理
- **Jaccard 系数**：$J(A,B) = \frac{|A \cap B|}{|A \cup B|}$，衡量集合重叠度
- **RAGAS**：专门为 RAG 系统设计的评估框架
  - Context Recall：答案所需信息是否被检索到
  - Context Precision：检索到的信息有多少是有用的
- **LLM-as-Judge**：用 LLM 评估 Context Recall/Precision（因为需要语义理解）

---

## 项目目录结构

```
车书问答系统源码/
├── build_index.py            # 🔨 离线建库（PDF解析→清洗→切分→索引）
├── infer.py                  # 🚀 在线问答（交互式QA）
├── final_score.py            # 📊 效果评估（语义相似度+RAGAS）
├── generate_sft_data.py      # 📝 生成微调数据（SFT+Rerank训练数据）
├── config.ini                # ⚙️ 服务启动配置
├── requirements.txt          # 📦 依赖列表
│
├── src/
│   ├── constant.py           # 📋 路径常量（模型/数据/索引路径）
│   ├── utils.py              # 🔧 工具函数（merge_docs, post_processing）
│   │
│   ├── parser/               # 📄 文档解析
│   │   ├── pdf_parse.py      #   PDF解析 + 语义切分 + 父子文档
│   │   └── image_handler.py  #   图片提取 + 标题检测
│   │
│   ├── client/               # 📡 外部服务客户端
│   │   ├── llm_local_client.py    # Qwen3-8B 本地推理（vLLM）
│   │   ├── llm_chat_client.py     # 豆包API（生成QA数据用）
│   │   ├── llm_hyde_client.py     # HyDE 查询扩展
│   │   ├── llm_clean_client.py    # LLM 文本清洗（20线程并发）
│   │   ├── mongodb_config.py      # MongoDB 连接管理（单例模式）
│   │   └── semantic_chunk_client.py # 语义切分HTTP客户端
│   │
│   ├── retriever/            # 🔍 检索器
│   │   ├── bm25_retriever.py     # BM25 稀疏检索（jieba分词）
│   │   ├── milvus_retriever.py   # Milvus 混合检索（BGE-M3 + RRF）
│   │   ├── faiss_retriever.py    # FAISS 稠密检索（BCE embedding）
│   │   ├── tfidf_retriever.py    # TF-IDF 稀疏检索
│   │   ├── qwen3_retriever.py    # Qwen3-Embedding 检索
│   │   └── retriever.py          # 抽象基类
│   │
│   ├── reranker/             # 📊 精排模型
│   │   ├── bge_m3_reranker.py    # BGE-Reranker-v2-M3（Cross-Encoder）
│   │   ├── qwen3_reranker.py     # Qwen3-Reranker（LLM-based）
│   │   ├── qwen3_reranker_vllm.py # Qwen3-Reranker-vLLM批量推理
│   │   └── reranker.py           # 抽象基类
│   │
│   ├── gen_qa/               # 📝 QA数据生成
│   │   └── run.py            #   QA生成→同义句扩展→关键词→质量评估
│   │
│   ├── server/               # 🌐 微服务
│   │   └── semantic_chunk.py #   语义切分FastAPI服务（聚类+embedding）
│   │
│   └── fields/               # 📋 数据模型
│       ├── manual_images.py  #   图片元数据（Pydantic）
│       └── manual_info_mongo.py # MongoDB文档模型
│
├── data/                     # 📁 数据
│   ├── Tesla_Manual.pdf      #   源文档
│   ├── stopwords.txt         #   停用词表
│   ├── processed_docs/       #   处理后文档（raw/clean/split .pkl）
│   ├── saved_index/          #   索引文件（bm25/tfidf/faiss/milvus）
│   ├── qa_pairs/             #   QA数据集
│   ├── summary_data/         #   SFT训练数据
│   ├── rerank_data/          #   Rerank训练数据
│   ├── saved_images/         #   提取的PDF图片
│   └── mongodb/              #   MongoDB数据
│
├── models/                   # 🤖 模型文件
│   ├── BAAI/bge-m3           #   BGE-M3 Embedding + Reranker
│   ├── Qwen3-8B              #   Qwen3-8B LLM
│   ├── Qwen3-Reranker-4B    #   Qwen3-Reranker
│   ├── text2vec-base-chinese #   text2vec（评估用）
│   └── AI-ModelScope/m3e-small # M3E Embedding
│
├── LLaMA-Factory-main/       # 🎓 LLM微调框架
│   ├── train.sh              #   训练脚本
│   ├── export.sh             #   模型导出
│   ├── vllm_serve.sh         #   vLLM部署
│   └── ...
│
└── RAG-Retrieval/            # 🎓 Reranker微调框架
    └── ...
```

---

## 完整请求流程示例

### 用户问："离车后自动上锁功能是什么"

```
Step 1: BM25 检索 → Top 10
  命中关键词："离车""自动""上锁"
  结果：[子文档A(score=12.3), 子文档B(score=8.1), ...]

Step 2: Milvus 混合检索 → Top 10
  Dense: 语义理解"锁车功能" ≈ "离车后自动上锁"
  Sparse: 关键词匹配"上锁"
  RRF融合 → [子文档C, 子文档A, 子文档D, ...]

Step 3: 去重合并
  A出现在两路结果中 → 去重
  子文档 → 查parent_id → 取回父文档
  结果：[父文档X, 父文档Y, 父文档Z, ...] (约12个)

Step 4: Rerank 精排
  BGE-Reranker 打分：
  (query, 父文档X) → 0.92  ← Top1
  (query, 父文档Y) → 0.85
  (query, 父文档Z) → 0.78
  ...
  取 Top 5

Step 5: LLM 生成答案
  上下文：
  【1】### 离车后自动上锁 ...
  【2】车门锁闭时 ...
  【3】### 大灯延时照明 ...
  ...

  LLM → "带着手机钥匙或配对的遥控钥匙离开时，车门和行李箱可以自动锁定。
          要打开或关闭此功能，可点击控制 > 车锁 > 离车后自动上锁。【1, 2】"

Step 6: 后处理
  提取引用：【1, 2】 → 页码 [7, 7]
  关联图片：docs[0].metadata["images_info"] → [{"title": "车锁设置", ...}]
  输出：{
    "answer": "带着手机钥匙或配对的遥控钥匙离开时...",
    "cite_pages": [7],
    "related_images": [{"title": "车锁设置", "path": "..."}]
  }
```

---

## 技术栈总览

| 类别 | 技术 | 用途 |
|------|------|------|
| **PDF 解析** | PyMuPDF (fitz) | 逐页提取文字和图片 |
| **文本切分** | LangChain RecursiveCharacterTextSplitter | 递归切分+overlap |
| **语义切分** | text2vec + AgglomerativeClustering | 语义聚类分组 |
| **中文分词** | jieba | BM25 的中文预处理 |
| **Token 计数** | tiktoken (cl100k_base) | 精确控制chunk长度 |
| **Embedding** | BGE-M3, BCE, M3E-small, Qwen3-Embedding | 文档向量化 |
| **稀疏检索** | BM25, TF-IDF | 关键词匹配检索 |
| **稠密检索** | FAISS, text2vec | 向量相似度检索 |
| **混合检索** | Milvus (Dense + Sparse + RRF) | 多路融合检索 |
| **精排** | BGE-Reranker-v2-M3, Qwen3-Reranker | Cross-Encoder 重排序 |
| **LLM** | Qwen3-8B (LoRA SFT + INT4) | 答案生成 |
| **LLM API** | 豆包 (Doubao) | 文本清洗/QA生成 |
| **推理引擎** | vLLM | 高性能LLM推理 |
| **微调框架** | LLaMA-Factory | LoRA SFT 训练 |
| **向量数据库** | Milvus (pymilvus) | 向量存储与检索 |
| **文档数据库** | MongoDB | 父子文档全文存储 |
| **Web框架** | FastAPI | 语义切分微服务 |
| **评估** | RAGAS, text2vec | RAG效果评估 |
| **数据模型** | Pydantic | 结构化数据校验 |

---

## 学习笔记与心得

### 1. RAG 架构设计的核心 trade-off

| Trade-off | 解决方案 |
|-----------|---------|
| 检索精度 vs 上下文完整性 | 父子文档：小chunk检索 + 大chunk给LLM |
| 召回率 vs 准确率 | 多路召回（BM25+Milvus）+ Rerank精排 |
| 速度 vs 效果 | 粗排(Bi-Encoder快) + 精排(Cross-Encoder准) |
| 关键词匹配 vs 语义理解 | 稀疏向量 + 稠密向量 + RRF融合 |

### 2. 多路召回的价值

- **BM25 擅长精确匹配**：用户说"离车后自动上锁"，文档也写"离车后自动上锁" → BM25完美命中
- **向量检索擅长语义理解**：用户说"下车后车门会自动锁吗"，文档写"离车后自动上锁" → 语义相似但词不同，BM25可能漏掉
- **RRF 融合**：不需要归一化不同检索器的分数，按排名倒数融合
- **两者互补 > 任何一路单独使用**

### 3. 数据飞轮的闭环

```
文档 → LLM生成QA → 同义句扩展 → 质量打分
                                    │
                    ┌───────────────┤
                    ▼               ▼
              SFT数据            Rerank数据
                    │               │
                    ▼               ▼
            微调Qwen3-8B    微调BGE-Reranker
                    │               │
                    ▼               ▼
              更好的生成        更好的排序
                    │               │
                    └───────┬───────┘
                            ▼
                      系统效果提升 → 再生成更好的QA...
```

这种"数据→模型→数据"的循环就是**数据飞轮**，是工业界持续提升RAG系统效果的核心方法论。

### 4. 评估的重要性

- **不能只看主观感受**：必须有量化指标（语义相似度、关键词覆盖率、RAGAS）
- **多维度评估**：语义相似度评估"答得对不对"，关键词覆盖率评估"关键信息有没有漏"，RAGAS评估"检索质量好不好"
- **"无答案"也要评估**：手册里没有的问题，正确做法是回答"无答案"而不是瞎编

### 5. 工程实践经验

- **Pickle 缓存**：`raw_docs.pkl`, `clean_docs.pkl`, `split_docs.pkl` 每步缓存，避免重复计算
- **MongoDB 用 Upsert**：`update_one(..., upsert=True)` 幂等操作，重跑不会产生重复数据
- **显存管理**：用完embedding模型后 `del model; torch.cuda.empty_cache()`
- **Reranker FP16**：`model.half().cuda()` 节省一半显存、加速约2倍



## 🚀 面试高频问题与解答 (QA)

### Q1: 为什么要用“语义聚类切分”而不是简单的固定长度切分？

**解答**：

1. **语义完整性**：固定长度切分（如 256 token）极易在句子中间甚至关键词中间“拦腰截断”，导致检索时语义丢失。
2. **聚类优势**：使用 `AgglomerativeClustering`（凝聚层次聚类）配合 `text2vec`，能将语义相关的句子强行“粘”在一起，确保每个 chunk 都是一个完整的话题。
3. **边界处理**：在聚类基础上，我们还引入了 Markdown 标题作为“硬边界”，防止不同章节的内容被混在一起。

### Q2: 既然有了 BGE-M3 的稠密向量检索，为什么还要保留 BM25？

**解答**：

1. **关键词敏感度**：向量检索擅长“语义”，但对“专有名词”和“特定型号”不敏感。例如“Model 3”和“Model Y”在语义空间极近，但业务上不能弄混。BM25 靠词频匹配，能精确锁定这些硬核关键词。
2. **长尾分布**：对于手册中出现的冷门零件名称，Embedding 模型可能未充分训练，此时 BM25 的表现更稳健。
3. **互补效应**：实验证明，BM25 与稠密向量通过 **RRF (Reciprocal Rank Fusion)** 融合后，NDCG@10 指标通常比单路提升 5%-10%。

### Q3: 详细解释一下“父子文档”机制是如何解决 RAG 痛点的？

**解答**：

* **痛点**：检索需要小粒度（颗粒度越细，余弦相似度计算越准）；生成需要大粒度（LLM 需要足够的上下文才能回答通顺）。
* **解决**：我们建立了两级映射。
* **子文档 (Child)**：256 token，带 overlap，存入向量库。
* **父文档 (Parent)**：512-1024 token，存入 MongoDB。


* **流程**：Query 命中子文档 $\rightarrow$ 获取其 `parent_id` $\rightarrow$ 实时从 MongoDB 拉取对应的父文档作为 Context。这样既保证了召回的灵敏度，又给了 LLM 充足的“阅读素材”。

### Q4: Reranker（精排）模型在系统中的角色是什么？不用它行不行？

**解答**：

* **角色**：它是典型的 **Cross-Encoder** 架构。比起粗排用的 Bi-Encoder（分别算向量），Reranker 会将 Query 和 Doc 拼接后一起输入，捕捉它们之间深层的语义交互。
* **必要性**：向量搜索只能保证“长得像”，不能保证“一定是”。在 Tesla 手册这种专业文档中，很多段落长得像但功能完全不同。Reranker 能在 Top 20 的候选集中踢除那些语义干扰项，将最正确的文档排到 Top 1，显著提升 LLM 的回答准确度。

### Q5: 在评估 RAG 系统时，为什么语义相似度（Semantic Sim）不能作为唯一指标？

**解答**：

1. **幻觉检测**：语义相似度高的答案，逻辑可能是反的（比如把“支持”答成“不支持”）。
2. **关键信息遗漏**：答案可能语气很像，但漏掉了关键页码或具体的操作步骤。
3. **综合指标**：所以我们引入了 **Keyword Jaccard (关键词覆盖率)** 确保硬核信息在场，并引入 **RAGAS** 框架评估“忠实度 (Faithfulness)”和“相关性 (Relevance)”，从而构建多维度的评估雷达图。


