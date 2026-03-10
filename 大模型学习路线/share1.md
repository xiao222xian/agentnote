

# 🚀 大模型 1v1 辅导 · 阶段 1（基础夯实）

> **本阶段目标：**
> 打牢大模型基础，建立完整知识地图，能从 **原理、代码、实战** 三个层面真正理解 Transformer、GPT、ChatGPT 与 RLHF。

---

## 📍 快速索引

* [⚓ 阶段说明](https://www.google.com/search?q=%23%E9%98%B6%E6%AE%B5%E8%AF%B4%E6%98%8E)
* [🗺️ 学习路径](https://www.google.com/search?q=%23%E5%AD%A6%E4%B9%A0%E8%B7%AF%E5%BE%84)
* [🕒 学习时长](https://www.google.com/search?q=%23%E5%AD%A6%E4%B9%A0%E6%97%B6%E9%95%BF)
* [📚 学习内容](https://www.google.com/search?q=%23%E5%AD%A6%E4%B9%A0%E5%86%85%E5%AE%B9)
* [💡 怎么学（必读）](https://www.google.com/search?q=%23%E6%80%8E%E4%B9%88%E5%AD%A6)
* [🎯 学习重点](https://www.google.com/search?q=%23%E5%AD%A6%E4%B9%A0%E9%87%8D%E7%82%B9)
* [🌟 学后效果](https://www.google.com/search?q=%23%E5%AD%A6%E5%AE%8C%E6%95%88%E6%9E%9C)
* [📅 三周打卡计划](https://www.google.com/search?q=%23%E4%B8%89%E5%91%A8%E8%AE%A1%E5%88%92)
* [📝 实战作业](https://www.google.com/search?q=%23%E5%AE%9E%E6%88%98%E4%BD%9C%E4%B8%9A)
* [🎤 面试作业](https://www.google.com/search?q=%23%E9%9D%A2%E8%AF%95%E4%BD%9C%E4%B8%9A)
* [✅ 阶段自查](https://www.google.com/search?q=%23%E9%98%B6%E6%AE%B5%E8%87%AA%E6%9F%A5)

---

## ⚓ 阶段说明

本阶段属于 **大模型基础夯实阶段**。
⚠️ **注意：** 请严格按照既定路径学习，**不建议跳跃式学习**。

这一阶段的重点不在于收集“名词”，而在于真正落地三件事：

1. **建立完整** 的大模型知识图谱。
2. **真正看懂** Transformer / GPT / ChatGPT 的底层逻辑。
3. **串联闭环**：把论文、代码实现和工程实践有机结合。

---

## 🗺️ 学习路径

**大模型原理** ➡️ **手撕 Transformer / GPT 底层原理** ➡️ **ChatGPT 的前世今生** ➡️ **大模型实战**

---

## 🕒 学习时长

* **建议周期：** 2 ~ 3 周
* **进度反馈：** 如遇时间冲突（校招、大厂项目、面试等），请务必及时在 **1v1 私享群** 内反馈，以便调整进度。

---

## 📚 学习内容精选

### 1. 大模型核心原理

* **【Transformer 的前世今生】**
🔗 [点击观看 B站视频](https://www.bilibili.com/video/BV11v4y137sN)

### 2. 手撕 Transformer / GPT

* **【图解 + 手撕底层原理】**
🔗 [知乎深度专栏](https://zhuanlan.zhihu.com/p/648127076)
> *Transformer 手撕级教程*


* **【手写 LLAMA3】**
🔗 [GitHub 仓库 (PyTorch 版)](https://github.com/naklecha/llama3-from-scratch)

### 3. ChatGPT 的核心技术

* **【论文研读】** *InstructGPT: Training language models to follow instructions with human feedback*
* **【论文配套视频 · 李沐】**
🔗 [B站通俗讲解版](https://www.bilibili.com/video/BV1hd4y187CR)
* **【论文讲解及扩展】**
🔗 [知乎专栏：论文之外的细节](https://zhuanlan.zhihu.com/p/677607581)
* **【DeepSeek 解读】**
🔗 [DeepSeek 技术拆解视频](https://www.bilibili.com/video/BV1RtNLeqEeu)
* **【GRPO 精讲】**
🔗 [LLM + RL 核心原理视频](https://www.bilibili.com/video/BV1LYQWYFE9S)

### 4. 大模型实战

* **【Transformer 案例实战教程】**
🔗 [Transformers 快速上手指南](https://transformers.run/)

---

# 💡 怎么学（学前必读）

## 一、大模型核心原理 🧱

**重点：** 在脑中建立完整的大模型全景图。

* **学习目标：** 优先学习 **Transformer 的前世今生**。从语言模型、word2vec、注意力机制一路串联到 Transformer 和 GPT。
* **建议：**
* 零基础同学只看 **Transformer & GPT**。
* 分类实战/BERT 可暂缓。
* **目标是：** 理解框架、认知实战知识、理清术语脉络。不懂的先跳过，后面刷题补。


* **建议耗时：** 约 1 周。

## 二、手撕 Transformer / GPT ✍️

**目标：彻底搞懂底层。** 推荐通过 PyTorch 手撕核心模块。

* **核心模块：** 位置编码、多头注意力、Add & Norm、FFN。
* **特别强调：** **必须手写 MultiHeadAttention 类**。这是本阶段最重要的代码能力。
* **补充教程：** 注意力机制看不懂？看这个视频的前两个：[B站链接](https://www.bilibili.com/video/BV19YbFeHEtz)
* **进阶：理解 Llama 架构。** 关注 GQA、ROPE、SwiGLU、FFN、MoE 的演化逻辑。

> **💡 资源说明：**
> * **运行 Llama3-8B：** 需 18G 显存。
> * **算力推荐：** 算力不足可用 **AutoDL** 租 3090（用完即关）。
> * **模型下载：** 官网受限可去 **ModelScope** 下 `original` 核心文件。
> 
> 

## 三、ChatGPT 的前世今生 🤖

**目标：掌握“对齐”底层原理。**

* **核心论文：** **InstructGPT**（ChatGPT 路线的“母论文”）。
* **阅读工具：** 推荐使用 **步达读论文** 或 **alphaxiv** (例如将 `arxiv.org` 替换为 `alphaxiv.org`) 提高效率。
* **精读重点：** Introduction, Methods, Experimental details。
* **推荐视频：** 李沐讲解版 & **Kaparthy 讲解版** [B站链接](https://www.bilibili.com/video/BV1ts4y1T7UH)。
* **RLHF 必考点：** PPO 模型协作、奖励模型数据构建、RLHF 步骤分解。
* **代码实践：** 推荐 [Simple_RLHF](https://github.com/lansinuote/Simple_RLHF)，单卡 4090 即可跑通。

> **扩展阅读 (可选)：**
> 1. LLaMA3 技术报告 [论文](https://arxiv.org/pdf/2407.21783) / [解读](https://zhuanlan.zhihu.com/p/712251536)
> 2. Qwen3 技术报告 [论文](https://arxiv.org/pdf/2505.09388) / [解读](https://zhuanlan.zhihu.com/p/1905945819108079268)
> 3. [大模型架构对比](https://sebastianraschka.com/blog/2025/the-big-llm-architecture-comparison.html#6-qwen3)
> 
> 

## 四、大模型实战 🛠️

**目标：熟悉工程落地的“手感”。**

* **必学章节：** 微调预训练模型、文本摘要任务、抽取式问答任务。
* **基础薄弱补课：** [Transformers 库基础教程视频](https://www.bilibili.com/video/BV1ma4y1g791/)。

---

# 🎯 学习重点（必学清单）

| 模块 | 核心考核点 |
| --- | --- |
| **核心原理** | LLM 基础、RLHF 基本过程 |
| **ChatGPT 技术** | InstructGPT 论文、Reward 模型构建、PPO/GRPO 逻辑 |
| **手撕系列** | **Multi-head Attention (重中之重)**、ROPE、FFN、残差标准化 |
| **实战任务** | 模型微调、文本摘要、抽取式问答 |

---

# 🌟 学完达到的效果

* **理论：** 讲清生成式模型演进，掌握 RLHF 与 PPO 整体流程。
* **代码：** **盲写** Multi-Head Attention 核心代码，根据公式快速还原逻辑。
* **实战：** 独立完成 NLP 经典任务微调，代码可灵活迁移至新业务。

---

# 📅 三周学习计划（打卡版）

### 第 1 周：打基础 🏗️

* **目标：** 搞懂 Transformer / GPT 主脉络。
* **任务：** 看完原理视频，理解注意力机制。
* **产出：** 知识结构脑图 + 疑问清单。

### 第 2 周：手撕 + 吃透 ChatGPT 🧠

* **目标：** 掌握核心代码与 InstructGPT 流程。
* **任务：** 手撕 Multi-Head Attention，读懂 RLHF 协作逻辑。
* **产出：** 个人版 MHA 实现代码 + InstructGPT 笔记 + RLHF 流程图。

### 第 3 周：进入实战 💻

* **目标：** 跑通 NLP 任务，形成工程感。
* **任务：** 完成微调、摘要、QA 三大实战。
* **产出：** 3 份实战记录，解决所有残留知识盲区。

---

# 📝 实战作业：生成式问答模型训练

### 🏁 作业目标

基于 **Google T5-Base** 构建一个问答模型。要求输入“文章+问题”，AI 生成对应答案。

### 📋 作业规格

* **模型 (Backbone):** T5
* **评价指标:** BLEU-1, 2, 3, 4
* **数据格式示例:**
```json
{
  "context": "淘宝网每年12月31日24:00点会对符合条件的扣分做清零处理...",
  "answer": "12月31日24:00",
  "question": "淘宝扣分什么时候清零",
  "id": 203
}

```



### ✅ 必做项

1. 完成训练代码。
2. 绘制 **Loss 收敛曲线图**。
3. 完成预测脚本，支持任意 Context + Query 推理。

### 🔗 资源链接

* **模型:** [ModelScope 萌子 T5](https://modelscope.cn/models/langboat/mengzi-t5-base)
* **数据集:** [夸克网盘](https://pan.quark.cn/s/6d4a98cd65f2) (码: `bzne`)

---

# 🎤 面试作业：深度复述

*要求：不背定义，结合代码和工程实践用自己的话讲清楚。*

1. Transformer 基本流程。
2. 为什么需要多头注意力？
3. ROPE 旋转位置编码详解。
4. 为什么 LLM 偏爱 Decoder-only 架构？
5. ChatGPT 训练的具体步骤。
6. 位置编码的必要性。
7. LLM 损失函数计算逻辑（10w 词表如何算 Loss）。
8. DeepSeek MLA 注意力与 KV-Cache 优化。
9. GRPO 算法改进点。
10. 如何通过 Loss 曲线看强化学习效果？

---

# ⚠️ 其他注意事项

1. **拒绝死磕公式：** 实在不懂先跳过，重点看工程细节。
2. **必须动手：** 理论看十遍不如代码写一遍。
3. **善用工具：** 配置好 DeepSeek 或 GPT 作为你的助教。
4. **记录 Bug：** 所有的排错记录都是你的“护城河”。

---

# ✅ 阶段自查清单

* [ ] 为什么 Transformer 比 RNN 更适合并行？
* [ ] Self-Attention 的核心公式是什么？
* [ ] ROPE 和传统位置编码的区别？
* [ ] ChatGPT 四阶段训练流程是什么？
* [ ] 你能手写 Multi-Head Attention 吗？
* [ ] 问答任务跑通了吗？

---

# 🏆 阶段目标总结

学完本阶段，你将拥有：

* 讲清 Transformer/GPT/ChatGPT 关系的**深度底气**。
* 手撕核心代码的**工程能力**。
* 独立完成生成式任务的**实战经验**。
* 为下一阶段 **RAG、Agent、多模态** 打下的**坚实地基**。

---

**备注：**

* 学得透比学得多更重要。
* 遇到“硬骨头”及时在群里 Call 老师。
* **加油，大模型之门已经开启！**


# 🎓 面试通关锦囊：10 道核心题参考答案

### 1. 请简述下 Transformer 基本流程

**核心逻辑：**
Transformer 采用 Encoder-Decoder 架构（虽然后来的 GPT 偏爱 Decoder-only）。

* **输入层：** Token Embedding + Position Encoding（相加得到输入向量）。
* **处理层（多个 Block 堆叠）：** * **Self-Attention：** 计算 $Q, K, V$，通过 $Attention(Q,K,V) = \text{softmax}(\frac{QK^T}{\sqrt{d_k}})V$ 捕获全局上下文。
* **Add & Norm：** 残差连接防止梯度消失，Layer Norm 保证数据分布。
* **FFN：** 两层线性映射 + 激活函数，增加非线性表达。


* **输出层：** 通过 Linear + Softmax 映射到词表概率空间。

### 2. 为什么基于 Transformer 的架构需要多头注意力机制？

**核心逻辑：**

* **多空间特征提取：** 类似于 CNN 的多个卷积核，不同的“头”可以关注序列中不同位置的依赖关系（比如头 A 关注指代关系，头 B 关注语法结构）。
* **增强鲁棒性：** 防止模型过度关注某一个单一位置，通过集成学习的思想提高模型的泛化能力。
* **抑制注意力退化：** 多个子空间投影可以有效避免注意力权重过度集中在个别 Token 上。

### 3. 什么是旋转位置编码 (RoPE)？

**核心逻辑：**

* **绝对位置与相对位置的统一：** RoPE 通过旋转矩阵将绝对位置信息注入 $Q, K$ 中。
* **相对位置敏感：** 经过旋转后，$Q$ 和 $K$ 的内积只取决于它们的相对距离 $(m-n)$。
* **外推性优越：** 相比于原始 Transformer 的绝对位置编码，RoPE 在处理超出训练长度的长文本时表现更稳定。

### 4. 为什么现在的大模型大多是 decoder-only 的架构？

**核心逻辑：**

* **训练效率与规模：** Decoder-only 采用 Causal Mask（因果掩码），非常适合自回归预训练（预测下一个词），在超大规模参数下更容易收敛。
* **零样本泛化：** 研究发现，Decoder-only 在 In-context Learning（上下文学习）上的表现优于 Encoder-Decoder 或 Prefix-LM。
* **参数效率：** 统一的掩码机制让模型在相同计算量下能学习到更强的逻辑推理能力。

### 5. ChatGPT 的训练步骤有哪些？

**核心逻辑（三/四阶段法）：**

1. **SFT（有监督微调）：** 专家标注高质量问答数据，对预训练模型进行初步对齐。
2. **RM（奖励建模）：** 让模型对多个回答进行排序，利用排序数据训练一个打分器。
3. **RLHF（强化学习对齐）：** 使用 PPO 或 DPO 算法，根据奖励模型的反馈，不断迭代策略模型。
4. **迭代优化：** 循环往复，提升模型的安全性与连贯性。

### 6. 为什么 Transformers 需要位置编码？

**核心逻辑：**

* **打破排列不变性：** Self-Attention 是全对称的，无论 Token 的顺序怎么换，计算结果都一样。
* **捕捉语序语义：** 在 NLP 中，“狗咬人”和“人咬狗”语义完全不同。位置编码赋予了模型分辨“谁在谁前面”的能力，从而理解句法结构。

### 7. LLM 的损失函数是什么？给你一个 10w 的词表，怎么计算出损失值？

**核心逻辑：**

* **函数：** 交叉熵损失函数 (Cross-Entropy Loss)。
* **计算过程：** 1.  模型的末端 Logits 输出是一个 $1 \times 100,000$ 的向量。
2.  通过 **Softmax** 将其转换为概率分布 $P$。
3.  取目标词索引处的概率 $P_{target}$。
4.  计算 $Loss = -\log(P_{target})$。
* **工程细节：** 为了节省显存和加速，通常在训练时使用 `CrossEntropyLoss` 内置的优化，只针对目标 Label 的索引进行梯度回传。

### 8. 介绍一下 DeepSeek 的 MLA 注意力及 KV-Cache 优化

**核心逻辑：**

* **Multi-head Latent Attention (MLA)：** 它是对 MHA 的一种低秩压缩改进。
* **压缩 KV-Cache：** 传统的 KV-Cache 随 Context 线性增长。MLA 通过将 KV 投影到一个低维的潜在空间（Latent Space）进行存储，大幅减少显存占用。
* **吸收 RoPE：** MLA 将不参与 RoPE 的 KV 与参与 RoPE 的部分分开处理，既保留了长距离感知，又实现了极致的压缩。

### 9. 讲一下 GRPO 算法，相比 PPO 做了哪几点改进？

**核心逻辑：**

* **Group Relative Policy Optimization：** 它的核心是不再需要单独的 Critic（评论家）模型。
* **改进点：** 1.  **组内打分：** 针对一个 Prompt 生成一组样本（Group），通过这一组样本的平均奖励值来计算 Advantage。
2.  **架构极简：** 省去了 Critic 模型，大幅降低了显存消耗和计算复杂度。
3.  **更强的探索：** 组内对比鼓励模型在多个候选答案中寻找更优解。

### 10. 通过训练的 loss 曲线能看出对 GRPO 或者 PPO 的效果吗？

**核心逻辑：**

* **答案：很难通过单一 Loss 曲线判断。**
* **原因：** 强化学习的 Loss 往往抖动巨大，且策略模型的 Loss 下降并不一定代表效果好（可能发生了模式崩溃）。
* **判断标准：** 我们更多观察 **Reward 均值曲线**（是否平稳上升）和 **KL 散度**（模型是否偏离原分布太远）。如果 Reward 上升且模型输出多样性尚存，才是真正的“效果好”。

---

**💡 排版小提示：**
在公众号发布时，建议将这些答案放在**折叠面板**里，或者作为**回复关键词**可见，这样能引导读者互动，增加账号粘性。
