# Device-Cloud Personalization and Collaboration in Recommender Systems: A Systematic Survey in Large Model Era

> **Architecture v2.0**
> OC/OD 各设一章提供完整背景；DC 拆分为 Taxonomy + 三大机制 + 评估共五章，确保主体聚焦 DC。
> 十章节结构，每章/每节附详细写作概要。

---

## Abstract

- **核心要素确认**：
  1. **Problem**: 推荐系统正从纯云架构转向端云协同智能。OC 面临延迟/隐私/成本瓶颈，OD 受限于存储/算力/协同信号。
  2. **Taxonomy**: 提出按协同目标（collaborative objective）的三机制主分类——Computation Allocation / Model Sharing / Task Orchestration——配合明确的归属规则与辅助标签系统。Dual-Loop Personalization 作为贯穿全文的跨切面设计原则统一三种机制。
  3. **Coverage**: 系统综述 200+ 篇文献，覆盖 OC 三代演进、OD 部署与适配技术、以及 DCRec 的三维协同机制。
  4. **Directions**: 展望 LLM 时代的前沿方向（投机解码、Federated PEFT、Agentic 协作、On-Device RAG）。
- **关键词建议**：Device-Cloud Collaborative Recommendation, Federated Recommendation, On-Device Recommendation, Large Language Models, Personalization.

---

## 论文整体结构概览

| 章节 | 定位 | 预计页数占比 |
|:---|:---|:---:|
| Ch1 Introduction | 问题引入与贡献声明 | ~10% |
| Ch2 On-Cloud Recommendation | ★ 背景章（新手友好） | ~10% |
| Ch3 On-Device Recommendation | ★ 背景章（新手友好） | ~12% |
| Ch4 DCRec: Framework & Taxonomy | ★ DC 总纲 | ~12% |
| Ch5 Computation Allocation | ★ DC 机制一 | ~14% |
| Ch6 Model Sharing | ★ DC 机制二 | ~16% |
| Ch7 Task Orchestration | ★ DC 机制三 | ~14% |
| Ch8 Evaluation & Datasets | 实验生态 | ~6% |
| Ch9 Future Directions | 前瞻 | ~6% |
| Ch10 Conclusion | 总结 | ~2% |

> **设计理念**：
>
> 1. OC/OD 各一章保证新手的完整认知路径，但通过篇幅控制和"背景"定位避免喧宾夺主。
> 2. DC 占据 Ch4-Ch7 四章（Taxonomy + 3 Mechanisms），在数量上形成压倒性优势，正文主体明确。
> 3. Introduction 中显式声明 "Chapters 2-3 provide necessary background... Chapters 4-7 form the core contribution" 作为叙事锚点。
> 4. 增设 Ch8 评估章，完善综述的完整性。
> 5. 全文依据 **主分类（Computation / Model / Task）** 组织 Ch5-7，以 **Personalization** 作为跨切面设计原则统一全文。

---

## Chapter 1. Introduction

> **目标**：6-7 页（含 Related Surveys 节），交代推荐系统背景、与已有综述的定位差异、OC/OD 瓶颈、DC 范式动机、本文 2D Taxonomy 概览、Contributions 列表、全文组织结构。

### 1.1 Related Surveys and Positioning

1. **端云协同 AI 综述（非推荐专属）**：
   - Edge-Cloud Polarization and Collaboration (TKDE 2023)：端云协同 AI 宏观视角，不聚焦推荐。
   - End-Edge-Cloud Collaborative Computing (CST 2024)：深度学习计算卸载通论。
   - Edge-Cloud Collaborative Computing on Distributed Intelligence (CST 2026)：分布式智能与模型优化。
   - Collaborative Learning of On-Device Small Model and Cloud-Based Large Model (Arxiv 2025)：云大端小协同学习，涵盖 CV/NLP/Rec 但推荐仅为子议题。
   - Collaborative Mechanisms Between Large and Small Language Models (Arxiv 2025)：LLM 间协同机制，推荐非重点。
2. **推荐相关的分布式/联邦/端侧综述**：
   - Distributed Recommendation Systems (TOIS 2024)：分布式推荐通论，覆盖数据/模型/推理并行，但不以 DC 协同为核心分类轴。
   - On-Device Recommender Systems (DSE 2025)：纯端侧推荐综述，不涉及云端协同。
   - Survey on Federated Recommendation Systems (TNNLS 2024)：联邦推荐综述，隐私优先视角，不覆盖非联邦 DC 协同（如云召回+端重排、LLM 协同推理）。
   - Scenario-Oriented Survey of Federated Recommender Systems (Arxiv 2025)：联邦推荐的场景化视角。
   - Distributed Learning for Next-Gen Recommender Systems (IPCID 2025)：分布式学习通论。
   - Personalized Recommendation Models in Federated Settings (TKDE 2025)：联邦个性化推荐，聚焦模型个性化策略。

- **本文独特定位**（建议附一张 Coverage Map 对比表）：
    1. **To our knowledge, the first survey to treat DCRec as a standalone paradigm**：已有综述要么只覆盖端云协同 AI（不聚焦推荐），要么只覆盖推荐系统的某一片段。本文首次将 On-Cloud + On-Device + Device-Cloud 三重范式统一在单篇综述中。
    2. **提出按协同目标的主分类法**：已有综述多为一维分类。本文按 collaborative objective 将 DCRec 方法分为 Computation Allocation / Model Sharing / Task Orchestration 三个互斥的主分类，并制定明确的归属仲裁规则与辅助标签系统。
    3. **覆盖完整的 DC 协同谱**：不仅包括隐私优先的联邦推荐，也包括性能优先的协同推理、LLM 时代的语义协同等更广泛的 DC 范式。

### 1.2 Recommender Systems: From Centralized to Decentralized

- **写作要点**：
  - 推荐系统的工业价值与核心挑战（信息过载 → 个性化排序）。
  - 工业推荐系统的多阶段级联架构（召回 → 粗排 → 精排 → 重排），每一阶段对延迟、算力、候选规模的不同约束。
  - 传统范式 = 纯云中心化：所有计算、存储、模型部署在云端，端侧仅作为展示界面。
  - 引出核心张力：云端的"全局能力 vs 实时盲区"、端侧的"实时感知 vs 算力瓶颈" → **为什么需要端云协同**。
- **关键概念**：多阶段级联、云中心范式、端侧角色的历史演变。

### 1.3 Evolution and Bottlenecks of On-Cloud Recommendation

- **写作要点**：
  - 简述云端推荐的三代演进：Shallow CF → Deep Representation → Large Semantic Alignment。
  - 云端优势（全局协同信号、强表达能力、复杂模型部署）与三大瓶颈：
    - **实时性瓶颈**：日志回流 + 异步更新 → 秒级/会话级兴趣漂移无法捕捉。
    - **隐私瓶颈**：集中式存储用户行为数据 → 合规风险（GDPR 等）。
    - **成本瓶颈**：大模型推理成本随模型规模与请求量线性增长，尤其 LLM 引入后更加突出。
  - 过渡句：这些瓶颈推动了计算向端侧的迁移。

### 1.4 The Rise and Limitations of On-Device Recommendation

- **写作要点**：
  - 端侧推荐的核心驱动力：隐私法规、低延迟需求、端侧算力提升（NPU/GPU on device）。
  - 端侧优势（零延迟推理、数据不出设备、离线可用）与四大约束：
    - **存储约束**：Embedding 表动辄百 GB，端侧无法承载。
    - **算力约束**：复杂模型（GNN/Transformer/LLM）难以实时推理。
    - **协同信号缺失**：端侧仅见自身行为，缺乏跨用户 CF 信号 → 信息茧房。
    - **候选空间受限**：端侧无法维护全量物品库。
  - 过渡句：纯云与纯端各有致命短板 → 端云协同成为必然。

### 1.5 The Paradigm Shift: Device-Cloud Collaborative Recommendation

- **写作要点**：
  - 给出 DCRec 的**正式定义**：在推荐任务中，端侧与云侧**共同参与**推荐流水线的推理/训练/更新，并通过显式协同机制交换信息（候选集、软标签、表示、参数、梯度、策略等），以在全局协同信号与本地实时个性化之间达成折中。
  - 给出**纳入标准表**：
    - 存在云-端双向信息流动
    - 云端和端侧在推荐流程中承担互补角色
    - 端侧参与最终的推荐推理或排序决策
    - 协同机制对精度、延迟或隐私有可量化贡献
  - 阐明 DCRec 与 Federated Rec 的关系：我们认为 FedRec 可以视作 DCRec 在强隐私约束下的特例——当 $\mathcal{D}^{\text{cloud}}=\emptyset$ 且协同信息仅限于梯度/参数聚合时，DCRec 退化为联邦推荐。DCRec 还覆盖 Performance-first、Cost-first 等更宽松约束下的协作目标（如云召回+端重排、LLM 协同推理）。
  - 过渡句：本文从 Personalization 和 Collaboration 双视角出发，提出统一的分类框架。

### 1.6 Scope, Contributions, and Paper Organization

- **写作要点**：
  - **Contributions**（4-5 条）：
     1. To our knowledge, the first systematic survey that treats device-cloud collaborative recommendation as a standalone paradigm, covering 200+ papers.
     2. A primary classification of DCRec methods into three mechanisms based on collaborative objective (Computation Allocation / Model Sharing / Task Orchestration), with explicit placement rules and auxiliary tags for cross-dimensional methods.
     3. A comprehensive review organized by this classification, covering the full spectrum from infrastructure-level computation distribution to application-level task orchestration.
     4. Summarization of evaluation benchmarks, simulation platforms, and datasets for DCRec research.
     5. Identification of key open directions in the large model era (speculative decoding, federated PEFT, agentic collaboration, on-device RAG).
  - **Paper Organization 段落**：明确声明 Ch2-3 为背景、Ch4-7 为核心、Ch8-9 为支撑，避免审稿人对结构产生"DC 只是三分之一"的误解。

---

## Chapter 2. On-Cloud Recommendation (Background)

> **目标**：8-10 页，为新手提供完整的云端推荐认知链路。按时间/技术代际组织，强调每代的个性化机制与局限性，为后文 DC 章节铺设"云侧能提供什么 / 不能提供什么"的参照系。
>
> **写作原则**：
>
> - 每节结尾设"优势与局限"小结，不堆砌方法名，而是提炼设计思想。
> - 始终从"这对端云协同意味着什么"的视角反观。

| 维度 | Shallow CF (1994-2014) | Deep Representation (2015-2022) | Large Semantic (2022-2026) |
|:---|:---|:---|:---|
| 核心建模 | 矩阵分解 / 相似度 | DNN 特征交互 / 序列 / 图 | LLM 语义理解 / 生成 |
| Embedding 规模 | MB 级 | GB 级 | TB 级（FM 参数） |
| 表达能力 | 线性交互 | 非线性 + 高阶结构 | 常识推理 + 零样本 |
| 推理延迟 | <1ms | 1-10ms | 100ms-10s |
| 典型代表 | MF, BPR, FM | Wide&Deep, SASRec, LightGCN | P5, TALLRec, HSTU |
| DC 启示 | Embedding 是核心抽象 | 序列+图需要全局视角 | 催生云大端小范式 |

### 2.1 Shallow Collaborative Filtering Models

#### 2.1.1 Memory-Based Collaborative Filtering

- **写作要点**：
  - 基于 User-Item 交互矩阵的启发式方法（UserKNN, ItemKNN, 基于 Pearson/余弦的相似度）。
  - 核心思想：利用"邻居"的评分预测自身偏好。
  - 优势：直观、可解释、无需训练。
  - 局限：冷启动、稀疏性、扩展性差 → 对大用户/物品基数不可行。
  - 与 DC 的关联：去中心化场景中最简单的协同形式——在网络中通过 Gossip 协议传播评分，无需中心服务器。最早的去中心化推荐实践。

#### 2.1.2 Matrix Factorization and Latent Factor Models

- **写作要点**：
  - MF/PMF/BPR/SVD++：将 User-Item 矩阵分解为低维 Latent Vectors。
  - 核心思想：用 Embedding 捕获用户偏好与物品属性的潜在模式。
  - FM（Factorization Machines）：扩展至任意特征交互，成为 CTR 预估的基础组件。
  - 优势：良好的泛化能力、相对紧凑的表示。
  - 局限：线性交互能力有限、难以建模时序与高阶特征。
  - 与 DC 的关联：MF 的低维 Embedding 天然适合分布式存储与通信。Federated MF 是最早的 DC 实践形态之一。

#### 2.1.3 Summary: What Shallow Models Teach Us

- **写作要点**：
  - Embedding 是推荐系统的核心抽象，也是 DC 场景中的核心传输/存储对象。
  - CF 信号依赖全局数据，这构成了 DC 的核心张力（端侧需要但无法本地获取）。

### 2.2 Deep Representation Learning Models

#### 2.2.1 Deep Collaborative Filtering and Feature Interaction

- **写作要点**：
  - NCF、DeepFM、Wide&Deep、DCN：用神经网络替代 MF 内积，建模非线性交互。
  - 核心创新：从手工特征交叉到自动特征交互学习。
  - 工业里程碑：YouTube DNN（召回）展示了从海量候选到个性化推荐的工业级端到端流程。
  - 与 DC 的关联：深度模型参数规模显著增长，传输/更新/部署成本成为 DC 的实际瓶颈。

#### 2.2.2 Sequential and Session-Based Recommendation

- **写作要点**：
  - SASRec（Self-Attention）、BERT4Rec（双向 Transformer）、SR-GNN（图序列）：建模用户行为的时间动态。
  - 核心创新：从"静态偏好"到"动态意图"的范式转换。
  - 与 DC 的关联：序列建模的实时性刚需天然适合端侧部署（用户行为顺序数据在本地最完整），但模型复杂度与 Embedding 规模需要云端支持。

#### 2.2.3 Graph Neural Networks for Recommendation

- **写作要点**：
  - LightGCN、NGCF、PinSage：利用 User-Item 二分图传播协同信号。
  - 核心创新：高阶邻域信息的显式建模。
  - 优势：超越直接交互的协同信号发现。
  - 局限：图结构依赖全局数据，天然与端侧隔离矛盾。
  - 与 DC 的关联：Decentralized GNN 工作尝试将全局图分解为 Ego-graph 子图在端侧运行，是 DC 中"图分解"思路的起源。

#### 2.2.4 Summary: What Deep Models Enable and Constrain

- **写作要点**：
  - 深度模型极大提升了表达能力，但也大幅增加了存储/通信/算力负载。
  - 行为序列和图结构都需要全局视角，端侧天然受限 → DC 协作的必要性更加明显。

### 2.3 Large Semantic Alignment Models

#### 2.3.1 LLMs as Recommenders

- **写作要点**：
  - P5（Text-to-Recommendation 统一范式）、TALLRec（LLM 微调对齐推荐任务）。
  - 核心思想：将推荐重构为语言/生成任务，利用 LLM 的语义理解与常识推理。
  - 优势：零样本/少样本泛化、可解释性、跨域知识迁移。
  - 局限：推理延迟极高（自回归生成）、训练/推理成本巨大、难以规模化服务于毫秒级推荐请求。
  - 与 DC 的关联：这催生了"云大端小"的 LLM 协同范式 → 云侧 LLM 做复杂推理，端侧 SLM 做高频快速响应（LSC4Rec, CDA4Rec 等）。

#### 2.3.2 Generative and Diffusion Models in Recommendation

- **写作要点**：
  - DiffRec（Diffusion 生成交互）、TIGER/GenRec（生成式检索）。
  - 核心思想：将推荐建模为从用户表示到物品 ID 的生成过程。
  - 与 DC 的关联：生成式模型为端侧"合成数据"和"代理样本"提供了新工具（Privacy-preserving 数据增强）。

#### 2.3.3 LLM Agents for Recommendation

- **写作要点**：
  - RecMind、InteRecAgent、MemRec：LLM 作为可规划/可记忆/可工具调用的推荐智能体。
  - 核心思想：推荐不仅是"排序"，更是"理解→规划→执行"的闭环。
  - 与 DC 的关联：Agentic 范式天然适合端云角色分工——云端 Agent 负责全局规划与知识检索，端侧 Agent 负责实时感知与本地执行。这是 Ch7 和 Ch9 的重要铺垫。

#### 2.3.4 Summary: Lessons from the Cloud Side

- **写作要点**：
  - 云端推荐的能力边界：天然适合"全局理解"与"深度推理"，天然不适合"实时响应"与"隐私敏感"。
  - 这种不对称性就是 DCRec 设计的核心依据。

---

## Chapter 3. On-Device Recommendation (Background)

> **目标**：10-12 页，为新手提供完整的端侧推荐认知链路。按"部署 → 适配"两个阶段组织，对应工业流程中的"先把模型装进去 → 再让它持续更新适应"。
>
> **写作原则**：
>
> - 同样每节小结优势/局限与 DC 关联。
> - 嵌入表（Embedding Table）是贯穿全章的核心瓶颈，需反复强调。

| 维度 | Embedding Slimming | Knowledge Distillation | Model Compression | Privacy Adapt. | Latency Adapt. | Resource Adapt. |
|:---|:---|:---|:---|:---|:---|:---|
| 目标 | 缩减 Embedding 体积 | 保留教师模型质量 | 压缩全模型 | 数据不出设备 | 毫秒级在线更新 | 弹性适配硬件 |
| 技术手段 | 混合维度/组合 Hash/无表生成 | Ranking/Unbiased/LLM→Small | 量化/张量分解/跨层 | 纯本地/去中心化/遗忘 | 端侧重排/异步更新 | 弹性 Embedding/按需通信 |
| 压缩率 | 10-100× | 5-50× (参数) | 4-32× | N/A | N/A | 动态 |
| 精度代价 | 中 | 低-中 | 中-高 | 高（缺协同） | 中 | 中 |
| DC 角色 | 云托管全量表+按需下发 | 云侧教师→端侧学生 | 云指导压缩策略 | 纯端→需去中心化协同 | 端实时+云异步 | 云弹性配置 |

### 3.1 General Lightweight Methods: Deployable Inference

> **章节引言**：核心问题——如何将一个已经训练好的推荐模型"瘦身"到可以在端侧设备上以可接受的延迟和功耗运行？答案分三条路线：压缩 Embedding、知识蒸馏、模型压缩。

#### 3.1.1 Embedding Slimming

- **写作要点**：
  - 问题定义：推荐模型的 Embedding 表通常占据 90%+ 的参数量和存储空间，是端侧部署的第一瓶颈。
  - 技术路线：
    1. **混合维度分配**（Learnable Embedding Sizes, SSEDS）：不统一分配维度，低频特征用低维、高频用高维，自动化搜索。
    2. **组合式压缩**（Compositional Embeddings / Complementary Partitions）：用多个小 Hash 表的组合替代一个大 Embedding 表。
    3. **无表替代**（Deep Hash Embedding / No Embedding Table）：用轻量网络从特征原始值直接生成 Embedding，完全去除查表。
  - 与 DC 的关联：端侧不需要存储全量 Embedding → 云侧可以托管完整表并按需下发子集，或端侧本地生成、云侧提供纠正。

#### 3.1.2 Knowledge Distillation

- **写作要点**：
  - 问题定义：如何在模型大幅缩小的前提下，尽可能保留原始大模型的排序/推荐质量？
  - 技术路线：
    1. **Ranking Distillation**：从大模型（教师）的完整排序列表中学习，而非仅学习 Top-K。
    2. **Unbiased Distillation**：解决蒸馏过程中的曝光偏差问题。
    3. **LLM→Small Model Distillation**（EKD4Rec）：从 LLM 蒸馏知识到传统小推荐模型，利用 LLM 的语义理解弥补小模型的表达能力。
  - 与 DC 的关联：KD 是 DC 中最自然的"知识传输"机制——云侧教师模型持续产出软标签/排序信号，端侧学生模型实时消费并适配。

#### 3.1.3 Model Compression

- **写作要点**：
  - 问题定义：除了 Embedding，推荐模型的其他部分（特征交互层、排序网络）如何压缩？
  - 技术路线：
    1. **量化**（PQ-VAE, Product Quantization）：用离散编码替代浮点 Embedding，大幅缩减存储。
    2. **张量分解**（TT-Rec, Tensor Train）：将高维 Embedding 矩阵分解为低秩张量链。
    3. **跨层压缩**（MLCC）：多层级压缩（Embedding + 特征交互 + 输出层）的协同优化，而非独立压缩各层。
  - 与 DC 的关联：压缩后的小模型是端侧推理的"引擎"，但压缩策略可能需要云端的全局统计指导。

#### 3.1.4 Summary of General Lightweight Methods

- **写作要点**：
  - 三条路线（嵌入瘦身、知识蒸馏、模型压缩）不是互斥的，实际端侧部署通常组合使用。
  - 核心矛盾始终是：**精度 vs 体积 vs 速度** 的三元权衡。
  - 这些方法解决的是"模型如何装进设备"，但没有回答"模型如何在设备上持续进化"→ 引出下一节。

### 3.2 Efficient Adaptation Solutions: On-Device Personalization

> **章节引言**：核心问题——部署到设备后，模型如何利用本地数据持续更新以适应个体用户？答案按优化目标分三条路线：隐私优先、延迟优先、资源优先。

#### 3.2.1 Privacy-Oriented Adaptation

- **写作要点**：
  - 问题定义：数据绝对不能离开设备，如何在纯本地环境下实现个性化更新？
  - 技术路线：
    1. **Local Training**（DeepRec, PocketLens）：纯本地微调，无任何外部依赖。数据绝对安全但缺乏协同信号。
    2. **Decentralized Collaborative Learning**（MAC, DCL4Rec）：设备间直接或通过 Gossip 协议交换模型参数，无需中心服务器。
    3. **Federated Unlearning**：用户退出/被遗忘权场景——从全局模型中删除特定用户的贡献。
  - 与 DC 的关联：纯端侧隐私方案走到极致 = 孤岛模型，需要某种形式的"去中心化协同"来引入外部信号。这也是后文 Ch6 中 Federated Rec 的思想基础。

#### 3.2.2 Latency-Oriented Adaptation

- **写作要点**：
  - 问题定义：如何在保证推理低延迟（<10ms）和更新低延迟（分钟级）的前提下实现个性化？
  - 技术路线：
    1. **EdgeRec**（Mobile Taobao）：端侧实时重排 + 异步模型更新，工业级方案。
    2. **MPDA**（Cloud-Coordinated Domain Adaptation）：云端协调、端侧自主适配的框架，平衡全局一致性与本地个性化。
    3. **Real-time Short Video Rec**：针对短视频特殊场景的端侧实时偏好追踪。
  - 与 DC 的关联：端侧的实时推理与云端的异步更新形成了天然的"双速"协作结构，这是 Ch5（Computation Allocation）中 Slow-Fast Learning 的核心思想。

#### 3.2.3 Resource-Oriented Adaptation

- **写作要点**：
  - 问题定义：不同设备的算力、内存、电量、网络千差万别，模型如何弹性适配？
  - 技术路线：
    1. **弹性 Embedding**（Learning Elastic Embeddings, PEEL）：根据可用资源动态调整 Embedding 维度。
    2. **按需更新**（ODUpdate, IntellectReq）：只在模型对当前请求"不自信"时才触发云端协同更新，节省通信。
    3. **轻量架构**（SNN for Rec, On-Device LLM for Rec）：用脉冲神经网络等极低功耗架构替代传统 DNN，甚至将微型 LLM 搬上端侧。
    4. **自适应剪枝**（Shaver）：在端侧运行时根据实际负载动态剪枝无关参数。
  - 与 DC 的关联：资源异质性是 DC 场景的真实约束，不同设备可能需要不同的协同策略。Ch7 中的动态部署机制直接回应这一问题。

#### 3.2.4 Summary of Efficient Adaptation Solutions

- **写作要点**：
  - 端侧个性化从"纯隐私"到"纯延迟"是一个连续谱，实际方案通常在三点之间折中。
  - 资源约束使得"一刀切"的端侧方案不可行 → 云端需要提供可配置的个性化机制 → DCRec 的动机在此达到顶峰。

### 3.3 Chapter Summary: Why On-Device Alone is Not Enough

- **写作要点**：
  - 端侧的优势和劣势恰好与云端互补，这是 DCRec 存在的根本逻辑：

    | 维度 | 云端优势 / 端侧劣势 | 端侧优势 / 云端劣势 |
    |:---|:---|:---|
    | 协同信号 | 全局 CF / 弱 | 强 / 信息孤岛 |
    | 实时性 | 弱 / 日志回流延迟 | 强 / 零延迟 |
    | 隐私 | 弱 / 集中式存储 | 强 / 数据不出设备 |
    | 算力 | 强 / 无限制 | 弱 / 严格受限 |
    | 候选集 | 大 / 全量物品 | 小 / 受限 |

  - 过渡：这一互补关系是 DCRec 设计的核心矛盾与核心机遇。

---

## Chapter 4. Device-Cloud Collaborative Recommendation: Framework and Taxonomy

> **目标**：10-12 页，DC 部分的总纲。给出形式化定义、三维分类法、三大核心挑战、双循环个性化范式。这一章是全文的"概念骨架"，所有后续章节的方法都应该溯源于此分类框架。
>
> **重要性**：如果 Ch4 写得不够清晰，审稿人会感到 Ch5-7 的机制划分没有逻辑根基。

### 4.1 Problem Formulation

- 统一数学符号：
  - $\mathcal{U} = {u_1, \ldots, u_N}$: 用户集合（设备集合）
  - $\mathcal{I} = {i_1, \ldots, i_M}$: 物品集合
  - $\mathcal{D}_u^{\text{local}}$: 用户 $u$ 的端侧本地数据（交互序列、上下文特征、传感器数据等，只存在于设备上）
  - $\mathcal{D}^{\text{cloud}}$: 云端可访问的全局数据（脱敏交互日志、物品信息、跨用户统计等）
  - $\theta_u$: 端侧模型参数（每个用户可能不同，即个性化模型）
  - $\Phi$: 云端模型参数（全局共享或为每个用户生成）
- 端侧优化目标：
  $$\theta_u^* = \arg\min_{\theta_u} \mathcal{L}_u^{\text{local}}(\theta_u; \mathcal{D}_u^{\text{local}}) + \mathcal{R}(\theta_u, \Phi)$$
  其中 $\mathcal{R}$ 是云端协同引入的正则/先验/约束。
- 云端优化目标：
  $$\Phi^* = \arg\min_{\Phi} \sum_{u \in \mathcal{U}} w_u \cdot \mathcal{L}_u^{\text{global}}(\Phi; \mathcal{D}^{\text{cloud}}, \{\theta_u\})$$
- 协同约束：通信带宽 $B$、延迟预算 $T$、隐私预算 $\epsilon$。
- 强调：DCRec 的本质是**联合优化** ${\theta_u}$ 和 $\Phi$，但受隐私、带宽、延迟约束。
- **安全与隐私作为跨切面约束**：本文不在主分类之外单独设立安全/隐私维度，而是将其视为贯穿所有三个机制的跨切面约束。每个机制章节中会指出该机制的安全脆弱面（如 Ch6.1.3 的投毒/窃取攻击）和隐私加固策略（如 DP、安全聚合），避免安全/隐私与协同机制正交分类时的维度爆炸。
- **形式化在后续章节的延伸**：上述联合优化是 DC 最一般的形式化。在 Ch5-7 中，该框架会实例化为各机制的特殊形式：
  - **Ch5（Computation Allocation）**：聚焦于 $w_u$ 的分配策略与同步协议的约束，$\mathcal{R}(\theta_u,\Phi)$ 退化为参数/候选集下发，自由度在于计算负载与通信拓扑。
  - **Ch6（Model Sharing）**：聚焦于 $\mathcal{R}(\theta_u,\Phi)$ 的具体形式（联邦聚合及其个性化变体），典型约束是 $\mathcal{D}^{\text{cloud}}=\emptyset$。
  - **Ch7（Task Orchestration）**：聚焦于 $\theta_u$ 与 $\Phi$ 之间信息交换的内容与时机，以及推荐流水线各阶段的端云归属。

### 4.2 The Proposed Taxonomy: Primary Classification by Collaborative Objective

> **设计原则**：已有综述多按技术类型或应用场景分类。本文的立场是：DCRec 方法的**本质差异不在于用了什么技术，而在于协作机制主要优化什么目标**。因此我们按每个方法的 **primary optimization objective** 将 Ch5-7 构建为三个互斥的主分类。跨维度的方法以交叉引用和"辅助标签"处理，避免同一文献在多个章节被重复当作代表。

- **分类规则（硬性）**：一个 DCRec 方法被归档到 Ch5、Ch6 还是 Ch7，取决于其**核心贡献针对的系统瓶颈是什么**：

  1. **Ch5 — Computation Allocation（计算分配）**：核心优化目标是 **训练/推理的计算负载在端云之间的格局**。如果论文的主要贡献是设计新的分布式训练拓扑（PS/Gossip）、同步协议（同步/异步/Slow-Fast）、或推理时的算力卸载策略（Early Exit），则归入此类。
  
  2. **Ch6 — Model Sharing（模型共享）**：核心优化目标是 **模型参数/知识在端云之间的流动与聚合**。如果论文的主要贡献是设计新的联邦聚合算法、个性化联邦策略、或跨域/跨模态的知识迁移机制，则归入此类。注意：本类的前提是**端侧运行推荐模型**，如果只有候选集/排序结果在端云之间流转（没有模型同步），则不属于本类。
  
  3. **Ch7 — Task Orchestration（任务编排）**：核心优化目标是 **推荐流水线中具体任务在端云之间的分配与调度**。如果论文的主要贡献是设计任务级的端云分工协议（哪个阶段放云端、哪个放端侧）、动态触发机制（何时请求云端辅助）、或端云协同推理的交互接口（LLM/SLM 之间如何对话），则归入此类。

- **三个类别之间的关系**：从底层到顶层——Ch5 解决"算在哪"（基础设施层），Ch6 解决"知识怎么流"（模型参数层），Ch7 解决"决策谁做"（任务应用层）。这种递进关系意味着越往上的机制越依赖下层的支撑（好的任务编排需要高效的计算分配和合理的模型共享策略作为前提）。

- **如何处理多维度方法（归属仲裁规则）**：

  > 部分方法天然跨多个维度——例如 Meta Controller 既优化推理时的计算卸载（Ch5），又优化请求路由的任务决策（Ch7）。我们按以下规则做仲裁：
  > 1. **以核心创新点所在的维度为准**——Meta Controller 的核心创新在于"一个元控制器判断何时将请求升级到云端"（任务级决策），而非提出新的分布式拓扑或同步协议，因此**归入 Ch7**，在 Ch5 中以交叉引用形式提及。
  > 2. **同类边界案例统一裁定**：
  >    - EdgeRec、DCCL：核心创新是端侧重排系统的流水线设计 → Ch7。在 Ch5 中以 Slow-Fast 模式交叉引用。
  >    - CDA4Rec：核心创新是 LLM Agent 之间的协同协议 → Ch7。在 Ch6 中以 LLM 协同的知识融合交叉引用。
  >    - DUET：核心创新是云端为端侧生成模型参数（参数生成，非参数聚合）→ 本质上是对模型部署策略的编排 → Ch7。在 Ch6 中以异构部署交叉引用。
  >    - DeCoCDR：核心创新是跨域推荐在端云之间的部署协议 → Ch7。

### 4.3 Three Distinctive Challenges of DCRec

> 从 Introduction 1.0 中提出的三大挑战在这里展开详述，构建后续章节的"问题意识"。

#### 4.3.1 Natural Data Heterogeneity

- **写作要点**：
  - 推荐系统中的"每个用户就是一个分布"，行为模式、偏好强度、活跃度高度异质。
  - 在 DC 场景中，这种异质性从"数据集偏差"升级为"系统级约束"：
    - **统计异质性**（Non-IID）：不同用户的本地数据分布差异巨大，简单的模型平均会破坏个性化。在联邦推荐中表现为 Client Drift。
    - **行为异质性**：不同用户的交互密度、序列长度、兴趣漂移速度截然不同。端侧的自适应策略不能一刀切。
    - **场景异质性**：同一用户在通勤/休息/工作场景下表现出不同偏好，端侧有上下文感知能力，云端缺乏实时上下文。
  - 对分类框架的映射：Computation Allocation 要适应不同用户的计算需求；Model Sharing 要解决 Non-IID 下的全局聚合退化；Task Orchestration 要对不同用户动态调度不同策略。

#### 4.3.2 Embedding-Centric Model Structures

- **写作要点**：
  - 推荐模型与 CV/NLP 模型最本质的差异：高基数稀疏特征 → 超大 Embedding 表。
  - Embedding 表在 DC 场景中的三重身份：
    1. **存储瓶颈**：端侧无法存储完整 Embedding 表 → 需要云侧托管 + 按需下发 / 端侧本地生成 / 压缩替代。
    2. **通信瓶颈**：在联邦学习场景中，整个模型的同步带宽可能被 Embedding 表主导。
    3. **隐私载体**：Item Embedding 间接编码了用户行为偏好，传输 Embedding 可能泄露用户隐私。
  - 对分类框架的映射：Computation Allocation 要解决"Embedding 表放在哪里、如何高效访问"；Model Sharing 要解决"Embedding 如何在端云之间安全同步"；Task Orchestration 要解决"候选物品 Embedding 何时下发给端侧做重排"。

#### 4.3.3 Massively Distributed User Devices

- **写作要点**：
  - 工业推荐服务规模：十亿级用户 → 十亿级潜在计算节点。
  - 规模带来的独特挑战：
    - **参与率不可控**：并非所有设备都愿意/能够参与协同训练（电池、网络、用户授权）。
    - **设备异质性**：从旗舰手机到 IoT 设备、机顶盒，算力跨越 2-3 个数量级。
    - **通信不可靠**：移动网络下设备可能随时离线，同步式协作不可行。
    - **安全攻击面扩大**：海量端侧节点 = 海量潜在攻击入口（投毒攻击、模型窃取）。
  - 对分类框架的映射：Computation Allocation 要应对动态的设备可用性；Model Sharing 要应对部分参与、通信失败的问题；Task Orchestration 要为不同设备分配不同粒度的任务。

#### 4.3.4 Challenge × Mechanism Cross-Matrix

- **写作要点**：
  - 用一张 3×3 交叉矩阵表，显式展示每个挑战在哪个机制中是"主要应对对象"(●) vs "次要波及对象"(○)：

    |                         | Ch5 Computation Allocation | Ch6 Model Sharing | Ch7 Task Orchestration |
    |:------------------------|:--------------------------:|:-----------------:|:----------------------:|
    | Data Heterogeneity      | ● (Non-IID → Straggler / 异步策略) | ● (Client Drift → 个性化聚合) | ○ (同一用户跨场景调度) |
    | Embedding-Centric       | ● (Embedding 表放置与访问) | ● (Embedding 安全同步) | ○ (候选 Embedding 下发时机) |
    | Massively Distributed   | ● (参与率/设备可用性) | ● (部分参与/通信失败) | ● (设备粒度的任务分配) |
  
  - **作用**：
    1. 这张表成为 Ch4 到 Ch5-7 的导航图——每章开头回顾该章列中的 ● 项，建立"本机制主要解决什么挑战"的问题意识。
    2. TKDE 审稿人通过此表可快速确认：你的三大挑战不只是 Introduction 里的摆设，而是后续章节的结构化组织依据。
    3. 同时也显示出 DCRec 需要三维联动才能完整应对所有挑战（没有哪个单一机制能同时覆盖全部挑战的三列全 ●）。

### 4.4 Dual-Loop Personalization: A Cross-Cutting Design Principle

> 几乎所有 DCRec 方法都不同程度地采用了双循环模式，因此它适合作为统一理解 DC 个性化机制的概念透镜，但不适合作为可操作的分类标准。

- **写作要点**：
  - **慢循环（Slow Loop / Cloud-Stide）**：基于全局数据，小时至天级更新周期。提供：
    - 全局协同信号的初始模型参数（如 FedAvg 聚合结果）
    - 海量候选集的粗筛结果
    - 跨用户的泛化知识与探索策略
  - **快循环（Fast Loop / Device-Side）**：基于本地实时数据，秒至分钟级更新周期。提供：
    - 本地微调 / 测试时训练做个性化适应
    - 实时上下文感知的端侧重排
    - 隐私敏感特征的本地消费
  - **双循环在三种机制中的不同表现形式**：
    - 在 Computation Allocation（Ch5）中，双循环表现为 Slow-Fast 的计算架构（云端大模型慢更新 + 端侧小模型快适应）。
    - 在 Model Sharing（Ch6）中，双循环表现为全局聚合（Slow）与本地适应（Fast）的迭代——FedAvg 聚合 → 本地 Fine-Tune → 再聚合。
    - 在 Task Orchestration（Ch7）中，双循环表现为推荐流水线的两级调度——云端召回 / 策略生成（Slow）+ 端侧重排 / 实时决策（Fast）。
  - **个性化强度的可调性**：设端侧自适应深度为 $d_{\text{local}}$，云端干预频率为 $f_{\text{cloud}}$，则个性化强度 $P = f(d_{\text{local}}, f_{\text{cloud}}^{-1})$。DCRec 相比纯 OC 或纯 OD 的核心优势在于 $P$ 可动态调节——兴趣稳定时增大 $f_{\text{cloud}}$ 依赖全局知识，兴趣漂移时增大 $d_{\text{local}}$ 强化实时适应。
  - **配图建议**：双循环示意图，标注 Slow Loop 与 Fast Loop 在三种机制中的不同信息流（Ch5=参数+候选集下发，Ch6=梯度/Adapter 同步，Ch7=置信度信号+语义表示）。

---

## Chapter 5. Computation Allocation Mechanisms

> **目标**：10-12 页。"谁来计算"是 DC 的基础层问题。从分布式推荐架构到协同推理的算力卸载。
>
> **对应文献** "Computation Allocation → Distributed Recommendation" 部分。
>
> **与 Ch4 的衔接**：本机制对应 Ch4.2 主分类的第一类——核心优化目标是**计算负载在端云之间的分配格局**。Ch5 主要应对 Ch4.3.4 交叉矩阵第 1 列中的挑战：数据异质性（Non-IID → Straggler/异步）、Embedding 表放置、及分布式规模（参与率/设备可用性）。对于跨维度方法（如 EdgeRec、Meta Controller 等主要归入 Ch7 的工作），本机制仅在相关小节中作交叉引用。
>
> **机制形式化**：在 Ch4.1 的统一框架中，Computation Allocation 的特化形式为——
>
> - 协作变量：$w_u$（用户权重）、$\mathcal{T}_{\text{sync}}$（同步拓扑）、$\tau_u$（端侧本地更新频率）。
> - 约束空间：$\mathcal{C}_{\text{comp}} = \{\text{带宽} \leq B,\ \text{延迟} \leq T_{\text{round}}\}$。
> - 优化退化为：$\min \sum_u w_u \mathcal{L}_u^{\text{local}}(\cdot)$，满足 $w_u$ 与 $\mathcal{T}_{\text{sync}}$ 的联合约束。当 $\mathcal{T}_{\text{sync}}$ 为全连接同步时退化为经典 PS；当 $w_u$ 不均等时引入异步/Staleness 权衡。
>

| 方法 | 架构类型 | 同步模式 | 通信负载 | Straggler 处理 | 适应移动场景 | 代表工作 |
|:---|:---|:---|:---|:---|:---|:---|
| Classical PS | 中心化 | 同步 | 高（全参数） | BSP 等待最慢者 | 弱 | OpenEmbedding, PGLucid |
| Gossip-Based | 去中心化 | 异步 | 中（邻居交换） | 天然适应 | 强 | PEPPER, GDSRec |
| Bounded Async | 中心化 | 半同步 | 中 | Staleness bound | 中 | Ekko |
| Slow-Fast | 混合 | 双层 | 低（选择性同步） | Fast loop 独立 | 强 | MC2-SF, Hybrid Parallel |
| Comm-Efficient | 中心化/混合 | 多种 | 低（压缩/量化） | 依赖底层 | 中 | CoLR-FedRec, DEST |
| Early Exit | 混合 | 按需 | 极低（仅疑难请求） | N/A | 强 | Meta Controller |

### 5.1 Distributed Training Paradigms

#### 5.1.1 Centralized Parameter Server Architectures

- **写作要点**：
  - 经典 PS 架构：Worker（端侧/GPU 节点）负责梯度计算，Server（云端）负责参数存储与聚合。
  - 在推荐场景的特殊性：Embedding 表通常放在 Server 侧（因其巨大），Dense 参数可在 Worker 侧。这是推荐系统参数服务器的典型"混合放置"策略。
  - 代表性工作：OpenEmbedding（Persistent Memory PS）、PGLucid（GPU PS for Ads）、Herald（Embedding Scheduling）。
  - 局限性：纯 PS 架构假设所有节点同时在线、通信可靠，不符合端侧移动场景。

#### 5.1.2 Decentralized and Gossip-Based Architectures

- **写作要点**：
  - 去中心化架构的动机：无单点故障、无中心信任依赖、适合 P2P/移动边缘网络。
  - Gossip 协议基础：节点随机选择邻居交换模型参数，通过流行病传播达到渐进一致性。
  - 推荐场景实践：PEPPER（Gossip-based 用户中心推荐）、GDSRec（图去中心化社交推荐）、Decentralized POI Rec（DCL4Rec / DecKG）。
  - 挑战：收敛速度较 PS 架构慢、通信效率受 Gossip 拓扑影响大。
  - 与 DC 的关系：去中心化架构 = 极端弱云端依赖的 DCRec，云端可能仅作为目录/协调者而非直接计算节点。

#### 5.1.3 Synchronous vs. Asynchronous Collaboration

- **写作要点**：
  - 同步范式：所有端侧同时完成本地训练 → 云端聚合 → 新一轮下发。简单但受制于最慢设备（Straggler problem）。
  - 异步范式：端侧随时上传、云端即时聚合下发。适合移动设备不可靠的通信环境，但可能引入 Staleness。
  - 折中方案：Bounded Staleness / Semi-synchronous。Ekko（低延迟异步更新 OS）。
  - 推荐场景的特别考量：模型更新的"新鲜度"直接影响推荐质量——陈旧模型可能推荐已过时的内容。

### 5.2 Slow-Fast Learning Architectures

> **写作要点**：
>
> - Slow-Fast Learning 是 DC 场景下自然的计算分配策略：云端的 Slow Learner 在大量历史数据上训练全局能力；端侧的 Fast Learner 在本地实时数据上快速适应。
> - 形式化：云端模型 $\Phi$ 以慢频率（如每天）更新；端侧模型 $\theta_u$ 在 $\Phi$ 的初始化/正则化下以快频率（如每次交互）更新。
> - 代表性工作：MC2-SF（Slow-Fast for Mobile-Cloud Collab）、Hybrid Parallel Training（KDD 2021, 通信高效的模型+数据并行）。
> - 核心挑战：Slow-Fast 之间的**同步时机**（何时端侧需要云端的全局信息、何时云端需要端侧的局部反馈）和**知识粒度**（传递完整模型 vs 部分参数 vs 软标签）。

### 5.3 Communication-Efficient Strategies

- **写作要点**：
  - 为什么推荐场景通信效率特别重要：Embedding 表参数巨大 + 移动网络带宽有限 + 海量设备。
  - 策略分类：
    1. **梯度压缩**：稀疏化（Top-K 梯度）、量化（1-bit SGD, SignSGD）。
    2. **低秩分解**（CoLR-FedRec）：通过低秩矩阵分解减少通信参数量。
    3. **等价替代训练**（DEST）：用本地等价操作替代全局同步通信的部分步骤。
    4. **嵌套流水线**（NestPipe）：训练阶段的流水线并行优化。
  - 通信-精度权衡：每一个通信优化都伴随精度损失，需要用统一框架评估。

### 5.4 Collaborative Inference Strategies

#### 5.4.1 Cloud Retrieval + Device Reranking

- **写作要点**：
  - 工业界事实标准：云端负责在海量候选库中粗筛（召回+粗排），产出 Top-K 候选下发；端侧结合实时上下文物特征和最新交互行为进行细粒度重排。
  - 代表性工作：EdgeRec（Mobile Taobao 端侧重排）、On-Device Integrated Re-ranking（KDD 2023, 异质行为建模的端侧重排）。
  - 通信开销分析：每次请求下发 K 个候选的 Embedding/特征 ≈ K × D × 4 bytes。K 通常数十到数百，D 通常 64-256。在 4G/5G 网络下可接受。

#### 5.4.2 Dynamic Early Exit and Computation Offloading

- **写作要点**：
  - 动机：并非所有请求都需要云端大模型处理。简单请求端侧处理、复杂请求升级到云端。
  - 代表机制：Meta Controller（KDD 2022, 云端 Meta Controller 判断哪些请求需要升级到云端）。
  - 提前退出策略：端侧模型附带 Confidence Estimator，当模型对当前推荐的置信度低于阈值时，向云端发起辅助推理请求。
  - 挑战：何时退出/升级的决策本身也有开销，且可能影响用户体验的延迟感知。

### 5.5 Chapter Summary

- **写作要点**：
  - 计算分配机制解决了 DCRec 的基础设施层问题："如何高效地在端与云之间分配推荐计算"。
  - 关键趋势：从集中式 → 分布式 → 智能调度的演进路径。
  - 与下一章的衔接：计算分配解决了"在哪里算"，但如果端和云的模型不是同一套、或者模型需要持续同步，就需要模型共享机制（Ch6）。

---

## Chapter 6. Model Sharing Mechanisms

> **目标**：14-16 页，DC 论文中通常篇幅最长的章节。"如何共享模型知识与参数"是 DC 的核心要件，联邦推荐（Federated Recommendation）是主要技术载体。
>
> **对应文献**："Model Sharing → Federated Recommendation" 部分。
>
> **写作原则**：
>
> - 按技术演进而非时间线组织：Foundational → Personalization → Advanced Integration。这种"从基础到前沿"的叙事适合读者逐步深入。
> - 联邦学习是手段，推荐质量与隐私才是目标。每节都需回答：这种方法带来了多少精度提升？付出了多少隐私/通信代价？
>
> **机制形式化**：在 Ch4.1 的统一框架中，Model Sharing 的特化形式为——
>
> - 核心约束：$\mathcal{D}^{\text{cloud}} = \emptyset$（云端无原始数据访问权）。
> - $\mathcal{R}(\theta_u, \Phi)$ 的具体形式：$R(\theta_u, \Phi) = \lambda \cdot d(\theta_u, \Phi)$，即端侧模型与云侧全局模型之间的某种距离度量。FedAvg 中 $d(\cdot)$ 为 $\ell_2$ 加权平均；个性化联邦中 $d(\cdot)$ 为带用户特定残差或聚类约束的距离。
> - 与 Ch4 的对接：$\theta_u$ 的参数空间分解为共享部分 $\theta_u^{\text{shared}} \subset \Phi$ 与私有部分 $\theta_u^{\text{private}}$。隐私-精度-通信的三元权衡通过 $\dim(\theta_u^{\text{shared}})$ 的大小和 $\lambda$ 的强度来调节。
>
> **与 Ch4 的衔接**：本机制对应 Ch4.2 主分类的第二类——核心优化目标是**模型参数/知识在端云之间的流动与聚合**。Ch6 主要应对 Ch4.3.4 交叉矩阵第 2 列中的挑战：数据异质性（Client Drift → 个性化聚合）、Embedding 安全同步、及分布式规模（部分参与/通信失败）。与 Ch5 的计算分配不同，本机制的前提是端侧运行推荐模型且存在显式的端云参数同步。

### 6.1 Foundational Exploration: The Basics of Federated Recommendation

> **章节引言**：第一阶段的联邦推荐工作主要回答"能不能做"——在用户数据不出设备的前提下，能否训练出与集中式可比的推荐模型？

- 基础联邦推荐方法对比

  | 方法 | 模型类型 | 隐私机制 | 安全攻击面 | 聚合策略 | 精度 vs 集中式 | 代表工作 |
  |:---|:---|:---|:---|:---|:---|:---|
  | FedMF | MF | DP / SecAgg | 梯度泄露 | 加权平均 | -3~8% | FedMF, MetaMF, LightFR |
  | FedNCF | MLP | DP | 梯度+Embedding | FedAvg | -5~12% | FedNCF |
  | FedGNN | GNN | 匿名子图扩展 | 图结构推断 | FedAvg | -8~15% | FedGNN, FedPerGNN |
  | DP-Secure | 多种 | DP + SecAgg | 聚合结果推断 | 安全聚合 | -10~20% | PrivateRec, PrivFR |
  | Attack-Resilient | 多种 | Byzantine 鲁棒 | 投毒攻击 | 鲁棒聚合 | -3~10% | Byzantine Robust FedRec |

#### 6.1.1 Federated Matrix Factorization

- **写作要点**：
  - 这是最早也是最基本的 FedRec 方案，因为 MF 模型结构简单、隐私分析清晰。
  - 核心机制：客户端持有 User Embedding $\mathbf{p}_u$，服务端持有 Item Embedding $\mathbf{q}_i$。客户端在本地用隐式/显式反馈更新 $\mathbf{p}_u$ 并计算 $\mathbf{q}_i$ 的梯度上传；服务端聚合梯度更新 $\mathbf{q}_i$。
  - 代表工作：FedMF (Secure Fed MF)、MetaMF (Meta-learned MF for cold users)、LightFR (轻量级联邦 MF)。
  - 隐私增强：差分隐私（DP）噪声在梯度上传前注入；安全聚合（Secure Aggregation）确保服务端只看到聚合结果而非个体梯度。
  - 局限：MF 表达能力有限，且区分 User/Item Embedding 的存储方式在深度模型中不再适用。

#### 6.1.2 Federated Deep Recommendation Models

- **写作要点**：
  - 扩展到更深的推荐模型：FedNCF（联邦 NCF）、FedGNN（联邦图神经网络）。
  - 核心挑战：Item Embedding 和网络参数（Dense layers）都需要同步，通信开销显著增长。
  - 训练策略：FedFast（智能客户端选择加速收敛）、FedRec++（显式反馈的无损联邦）、Hierarchical FedRec（层次化聚合）。
  - 图联邦的特殊性：FedGNN 中每个客户端缺少全局 User-Item 图结构 → 需要用隐私保护的子图扩展（如通过服务器匿名传递高阶邻居）。

#### 6.1.3 Privacy and Security in Foundational FedRec

- **写作要点**：
  - 隐私机制：
    - 差分隐私（DP）：在梯度/模型更新中注入校准噪声，提供数学可证的隐私保障。
    - 安全聚合（SecAgg / Secure Multi-party Computation）：云端只能看到聚合结果。
    - Hash Embedding 替代（PrivFR）：用哈希嵌入替代直接 ID 查表，减少隐私泄露面。
  - 安全攻防：
    - 投毒攻击（PipAttack, PoisonFedDLRS）：恶意客户端提交伪造梯度，推广特定物品或降低全局模型质量。
    - 拜占庭鲁棒性（Byzantine Robust FedRec, AAAI 2025）：在不信任部分客户端的前提下保证聚合质量。
    - 模型窃取与无参数传输方案（Hide Your Model, ICDE 2024）：如何在完全不传输模型参数的情况下实现协同？

#### 6.1.4 Summary of Foundational Exploration

- **写作要点**：
  - 基础阶段解决了"可行性"问题，但遗留了三个关键不足：个性化不足（用户被均质化）、冷启动用户/物品处理不佳、跨场景泛化弱。这些推动了下一阶段的发展。

### 6.2 Personalization Enhancement: Global Aggregation Meets Local Adaptation

> **章节引言**：第二阶段的核心认识——"一刀切"的全局聚合会抹杀用户个性。如何在联邦框架内实现"千人千模"？

| 策略 | 个性化方式 | 冷启动支持 | 异构模型 | 额外通信 | 代表工作 |
|:---|:---|:---|:---|:---|:---|
| 加性个性化 | $\theta_u = \Phi + \Delta_u$ | 弱（依赖 $\Phi$ 质量） | 否 | $\|\Delta_u\|$ | FedRAP |
| 聚类个性化 | 簇内共享/簇间不同 | 中（簇级先验） | 否 | 簇分配信号 | CoFedRec, GPFedRec |
| 层次个性化 | 用户-组-全局三层 | 中（组级先验） | 否 | 组级参数 | Hierarchical PFL |
| 图引导 | 局部图指导聚合权重 | 强（图结构传递） | 否 | 子图描述 | GPFedRec, LPSFed |
| 双个性化 | User + Item 双端个性化 | 强（Item 属性分离） | 否 | 双端参数 | PFedRec, IFedRec |
| 异构聚合 | 允许不同架构 | 中 | 是 | 知识蒸馏代替参数同步 | HeteFedRec, FedEM, FedCA |

#### 6.2.1 Personalized Federated Recommendation Architectures

- **写作要点**：
  - 个性化策略的分类：
    1. **加性个性化**（FedRAP, ICLR 2024）：$\theta_u = \Phi_{\text{global}} + \Delta_u$，学习一个全局基底加用户特定残差。
    2. **聚类个性化**（CoFedRec, WWW 2024; GPFedRec, KDD 2024）：将相似用户聚类，簇内共享一个模型，簇间不同。
    3. **层次个性化**（Hierarchical PFL, WWW 2021）：用户-组-全局三层结构，组层捕获细粒度共性。
    4. **图引导个性化**（GPFedRec, KDD 2024; LPSFed, ICLR 2026）：用局部/全局图结构指导个性化聚合权重。
    5. **双个性化**（PFedRec, IJCAI 2023）：同时在用户端和物品端做个性化（User-specific + Item-specific）。
  - 核心权衡：个性化越强 → 越偏离全局协同信号 → 越接近纯端侧的"信息孤岛"。
  - 形式化与 Ch4 中个性化强度 $P = f(d_{\text{local}}, f_{\text{cloud}}^{-1})$ 的联系。

#### 6.2.2 Addressing Cold-Start and Data Sparsity

- **写作要点**：
  - 冷启动是 FedRec 中的核心痛点：新用户没有本地数据、新物品没有交互记录。
  - 策略：
    1. **属性分离**（IFedRec, WWW 2024）：将物品属性特征（对所有人都可见）与交互行为（隐私）分离处理。
    2. **知识融合**（WWW 2025, Cold-Start Users）：对冷启动用户自适应融合全局知识和相似用户知识。
    3. **联邦持续学习**（F3CRec, CIKM 2025）：在联邦场景中持续学习新物品/新用户，避免灾难性遗忘。
  - 与 DC 的关联：冷启动用户恰恰最需要云端的全局先验知识——这体现了 DC 中"云侧冷启动 + 端侧热适配"的自然分工。

#### 6.2.3 Heterogeneous Model Aggregation

- **写作要点**：
  - 此前假设所有端侧运行相同架构的模型。但在现实 DC 中，不同设备可能运行不同架构、不同大小的模型 → 异构联邦。
  - 代表工作：
    - HeteFedRec（ICDE 2024）：允许不同设备运行不同结构的推荐模型，云端通过知识蒸馏聚合异构模型的知识。
    - FedEM（AAAI 2026）：个性化的模型合并（Model Merging）策略，超越简单的加权平均。
    - FedCA（TOIS 2026）：复合聚合策略，分组件采用不同聚合方式。
  - 这一方向直接呼应 Ch4.3.3 的设备异质性挑战。

#### 6.2.4 Summary of Personalization Enhancement

- **写作要点**：
  - 个性化增强阶段将 FedRec 从"能用"推进到"好用"。
  - 核心趋势：从均质化聚合 → 结构化分层 → 自适应个性化。
  - 遗留问题：迄今讨论基本限于单一域、单一模态的协同过滤 → 下一节扩展到跨域、多模态、基础模型。

### 6.3 Advanced Integration: Cross-Domain, Multi-Modal and Foundation Models

> **章节引言**：第三阶段将联邦推荐的边界从"单域协同过滤"扩展到"跨域 + 多模态 + 基础模型"的广阔天地。

| 方法 | 集成维度 | FM 使用方式 | 通信粒度 | 隐私考量 | 代表工作 |
|:---|:---|:---|:---|:---|:---|
| FedCDR 系列 | 跨域 | N/A | 域间+端云双层 | 域间隐私隔离 | FedCDR, Win-Win, PFCR |
| FedVLR 系列 | 多模态 | VLM 特征提取 | 模态表示+个性化融合 | 多模态公共空间 | FedVLR, GFMFR |
| FedPA 系列 | FM 集成 | Adapter/Prompt 联邦 | 仅 Adapter (<1% 参数) | Adapter 隐私分析 | FedPA, TransFR, MRFF |
| FELLAS | LLM 作为服务 | LLM 外部服务 | 语义增强信号 | LLM API 侧隐私 | FELLAS |

#### 6.3.1 Cross-Domain Federated Recommendation

- **写作要点**：
  - 动机：不同业务域（电商 vs 视频 vs 新闻）拥有互补的用户行为信息，但因隐私/商业壁垒无法直接共享数据。
  - 方案：
    - FedCDR（CIKM 2022）：基础 FedCDR，隐私保护下共享跨域知识。
    - Win-Win（AAAI 2023）：双目标跨域联邦，同时提升两个域的效果。
    - PFCR（WWW 2024）：用 Prompt 增强的跨域联邦内容表示学习。
    - FFMSR（TOIS 2025）：联邦语义学习用于隐私保护跨域推荐。
    - FedCIA（SIGIR 2025）：联邦协同信息聚合。
  - 与 DC 的关联：跨域联邦本质上是将"端云协同"扩展为"域间协同 + 端云协同"的复合架构。

#### 6.3.2 Multi-Modal and Vision-Language Federated Recommendation

- **写作要点**：
  - 动机：物品的图片、文本描述、视频特征可以变成（部分）公共信息，绕过隐私限制。
  - 方案：
    - Manipulating Visually-aware FedRec (TOIS 2023)：多模态联邦推荐的攻击与防御。
    - FedVLR（AAAI 2026）：视觉-语言联邦推荐 + 个性化融合。
    - GFMFR（WWW 2026）：多模态增强的群组式联邦融合。
  - 关键洞察：多模态特征提供了一种"端云共享公共空间"——云端可以公开 Item 的多模态表示，端侧在此基础上做个性化。

#### 6.3.3 Foundation Model Integration in Federated Recommendation

- **写作要点**：
  - 动机：LLM/VLM 等基础模型提供了强大的语义理解能力，但其体积巨大无法在端侧运行。
  - 范式：
    1. **云端 LLM 服务 + 联邦适配**（FELLAS, TOIS 2025）：LLM 作为外部服务提供语义增强，FedRec 框架负责个性化聚合。
    2. **联邦 Prompt/Adapter 学习**（FedPA, IJCAI 2024; MRFF, AAAI 2025; TransFR, AAAI 2026）：冻结云端基础模型，端侧仅微调 Adapter/Prompt，云端聚合这些轻量模块。
    3. **联邦多模态基础模型**（GFMFR, WWW 2026）：在联邦框架内整合多模态基础模型。
  - 关键趋势：在 LLM 时代，FedRec 从"同步全部参数"转变为"同步轻量适配器"，从根本上改变了通信效率的格局。

#### 6.3.4 Summary of Advanced Integration

- **写作要点**：
  - 联邦推荐已从单一域协同过滤扩展到"跨域 + 多模态 + FM"的综合框架。
  - 基础模型的引入是最大变数：它同时解决了"表达能力瓶颈"（FM 提供强大语义）和"通信瓶颈"（PEFT 大幅减少通信量）。
  - 向下一章过渡：模型共享机制解决了参数/知识如何在端云之间流动。但推荐是一个多阶段流水线任务，模型参数只是其中一环——召回、排序、重排、冷启动、多场景切换等具体任务如何编排？这正是 Ch7 要回答的问题。

---

## Chapter 7. Task Orchestration Mechanisms

> **目标**：12-14 页。"如何编排推荐任务在端云之间的执行"是 DC 的最顶层设计——直接面向推荐效果与用户体验。
>
> **对应文献**："Task Orchestration → Device-Cloud Recommendation" 部分。
>
> **写作原则**：
>
> - 这是 DCRec 中最新、最活跃的研究方向（大部分工作集中在 2022-2026），需要突出时间新鲜度。
> - 每节需要关联 Ch4 的 Slow-Fast 双循环，说明该机制如何实例化双循环。
>
> **机制形式化**：在 Ch4.1 的统一框架中，Task Orchestration 的特化形式为——
>
> - 信息交换对象：端云之间传递的不再主要是梯度/参数，而是 **候选集 $\mathcal{C}_u$**、**置信度信号 $\text{conf}_u$**、**语义表示 $\mathbf{h}_u^{\text{sem}}$** 等任务级对象。
> - 优化扩展：目标函数扩展为多阶段级联损失 $\sum_{s \in \text{stages}} \alpha_s \mathcal{L}_u^{(s)}(\theta_u^{(s)})$，其中各阶段的 $\theta_u^{(s)}$ 可能由端或云独立管理，协作机制决定阶段间的信息流转协议。
> - 调度策略：定义决策函数 $\pi: (\mathcal{C}_u, \text{conf}_u, \text{context}_u) \to \{\text{local}, \text{cloud}, \text{hybrid}\}$，决定当前请求的处理路径。这是 Meta Controller、IntellectReq 等方法的核心抽象。
>
> **与 Ch4 的衔接**：本机制对应 Ch4.2 主分类的第三类——核心优化目标是**推荐流水线子任务在端云之间的分配与调度**。Ch7 主要应对 Ch4.3.4 交叉矩阵第 3 列中的挑战：Massively Distributed Devices（设备粒度的异构任务分配），同时也涉及跨场景调度的数据异质性和 Embedding 下发时机问题。本机制是 Ch4.2 中基础设施层（Ch5）和知识层（Ch6）之上的应用层，直接面向推荐效果与用户体验。

| 方法 | 流水线设计 | 触发策略 | 端侧延迟 | 云端开销 | 适配设备异质性 | 代表工作 |
|:---|:---|:---|:---|:---|:---|:---|
| Cloud-Retrieval + Device-Reranking | 云召回→端重排 | 每次请求 | <10ms | 高（全量召回） | 弱 | EdgeRec, DCCL |
| Dynamic Candidate Augmentation | 云按需补充 | 兴趣漂移触发 | <50ms | 中（仅补充） | 中 | Meta Controller, IntellectReq |
| Cloud-Coordinated Adapt. | 云协调+端自主 | 云定期+端按需 | <5ms | 中（策略下发） | 强 | MPDA, DUET |
| Compatibility-Aware | 端模型按需升级 | 版本不兼容触发 | <10ms | 低（超网维护） | 强 | DIET, CHORD, Forward Once |
| LLM+SLM Co-Inference | 端优先→云增强 | 不确定性触发 | <20ms (端) | 高（LLM推理） | 中 | LSC4Rec, CDA4Rec |
| Agentic Collaboration | 端云 Agent 对话 | 任务驱动 | 可变 | 极高（LLM多轮） | 强 | CDA4Rec, SCaLRec |

### 7.1 Co-Inference Pipeline Design

> **章节引言**：推荐的多阶段级联架构天然提供了端云分工的空间。问题是：每个阶段放在哪里？阶段之间如何协作？

#### 7.1.1 Cloud-Side Retrieval and Device-Side Reranking

- **写作要点**：
  - 工业标准模式：召回+粗排在云端（海量候选 → 数百候选），精排+重排在端侧（结合实时上下文 → 最终展示）。
  - 代表工作深度分析：
    - **DCCL**（KDD 2021）：最早的端云协同学习框架，云端学习全局知识，端侧本地适应。提出"千人千模"的概念，云侧吸收海量端侧个性化模型的知识。
    - **EdgeRec**（CIKM 2020）：淘宝移动端的端侧重排系统，展示工业级方案的延迟、功耗、效果统计。
    - **On-Device Integrated Re-ranking**（KDD 2023）：端侧重排中的异质行为建模（点击、购买、停留时间等不同行为信号的综合利用）。
  - 核心设计问题：
    - 候选集大小 K 的选择（K 越大精度越高但通信开销越大）。
    - 云端下发的信息粒度（仅 Item ID vs Item Embedding vs 完整 Item 特征）。
    - 端侧重排的延迟预算（通常 <10ms）。

#### 7.1.2 Dynamic Candidate Augmentation

- **写作要点**：
  - 动机：当端侧缓存候选不能满足当前用户兴趣时（兴趣漂移），需要云端干预扩大候选空间。
  - 代表工作：
    - **Meta Controller**（KDD 2022）：云端部署 Meta Controller 持续监控端侧推荐质量，当检测到端侧候选不足或用户兴趣变化时，触发云端候选补充。
    - **IntellectReq**（WWW 2024）：智能模型更新请求策略——只在模型"不确定"时才请求云端协同，避免不必要的通信。
  - 核心挑战：漂移检测的灵敏度 vs 虚警率权衡；云端干预的延迟（需控制在用户感知阈值内）。

### 7.2 Dynamic Model Deployment and Update

> **章节引言**：推荐模型不是部署一次就完事的——用户的兴趣在变、新物品在涌现、端侧环境在变。如何动态地部署和更新模型？

#### 7.2.1 Cloud-Coordinated Model Personalization

- **写作要点**：
  - **MPDA**（KDD 2022）：大规模云端协调的域自适应，端侧利用云端提供的域适应策略进行本地个性化。
  - **DUET**（WWW 2023）：免调参的端云协同参数生成——云端根据端侧需要生成模型参数，端侧直接使用无需本地训练。
  - 核心理念：云端不仅是"参数服务器"，更是"策略生成器"——告诉端侧如何个性化而不仅仅是给什么参数。

#### 7.2.2 Compatibility-Aware Model Evolution

- **写作要点**：
  - 现实约束：云端模型会持续迭代升级，但端侧模型可能由于版本管理、网络条件等原因无法同步更新。
  - 代表工作：
    - **DIET**（KDD 2024）：定制化瘦身处理不兼容网络——新云模型与旧端模型结构不同时，如何实现知识的有效迁移？
    - **CHORD**（MM 2025）：定制化混合精度端侧模型——云端为每个端侧设备定制适合其硬件能力的模型精度配置。
    - **Forward Once for All**（KDD 2025）：结构参数化适配——一次前向传播同时适配所有端侧设备，消除每设备单独训练的成本。
  - 核心设计模式：云端维护一个"超网"（Supernet），端侧设备通过结构参数从中提取适合自身能力的子网。

### 7.3 LLM-Era Orchestration: Cloud LLM + Device SLM

> **章节引言**：大模型时代的端云任务编排发生了质变——从"两端共享同一套参数"到"两端运行不同能力的模型，通过语言/语义接口协作"。这是当前 DCRec 最前沿的方向。

#### 7.3.1 Cloud LLM + Device Small Model Architecture

- **写作要点**：
  - 核心范式：云端部署强大 LLM（如 LLaMA-8B/70B）负责复杂的语义理解、跨域推理、冷启动；端侧部署小型专用模型（如 LLaMA-1B 或传统推荐模型）负责高频、低延迟的常规推荐。
  - 代表工作：
    - **LSC4Rec**（KDD 2025）：LLM 与小型推荐模型的协同框架，端侧 SLM 先尝试处理，不确定时调用云端 LLM。关键创新在主-从协作策略和请求调度。
    - **CDA4Rec**（Arxiv 2025）：端云协同智能体框架，端侧 Agent 和云侧 Agent 通过标准化协议交互任务请求和结果。
  - 技术要素：协作触发策略（何时调用 LLM）、提示构造（如何将端侧状态转化为 LLM 可理解的输入）、结果融合（LLM 的输出如何与端侧模型输出整合）。
  - 与 Ch4 Slow-Fast 双循环的映射：LLM = Ultra-Slow Loop（语义理解/天级更新），端侧 SLM = Fast Loop（实时响应/秒级更新）。

#### 7.3.2 Semantic Calibration and Knowledge Alignment

- **写作要点**：
  - 核心问题：云端 LLM 和端侧推荐模型处于不同的表示空间（自然语言 vs 协同过滤 Embedding），直接协作存在语义鸿沟。
  - 代表工作：**SCaLRec**（Arxiv 2026）：提出语义校准框架解决这一鸿沟。建立统一的语义表示空间，实现"LLM 通用知识 ↔ 推荐领域知识"的双向映射。
  - 对齐方法分类：
    1. 对比对齐：用正负样本对比学习拉近/推远表示。
    2. 蒸馏对齐：LLM 的语义表示作为教师信号蒸馏到端侧模型的 Embedding 空间。
    3. 共享投影：两端通过共享的投影层映射到统一空间。
  - 这一问题的解决是 LLM 时代 DC 从"可行"到"好用"的关键瓶颈。

#### 7.3.3 Backpropagation-Free and Relay Learning

- **写作要点**：
  - 端侧反向传播（Backpropagation）的计算和内存开销对于大型模型来说可能仍然很大。
  - **无反向传播适配**（ACM TOMM 2025）：云端通过正向传播即可为端侧模型生成个性化方案，端侧无需本地训练。
  - **RelayRec**（IPSN 2024）：云-端中继学习，云端大模型通过中继机制向端侧传递知识，端侧解耦训练与推理。
  - 这一路径指向了一个极端高效的 DC 未来：端侧几乎不做训练，完全依赖云端"投喂"个性化方案。

#### 7.3.4 Agentic Device-Cloud Collaboration

- **写作要点**：
  - 这是 LLM 时代最宏大的愿景：端云不再是"服务器-客户端"的关系，而是"两个 Agent 在对话"。
  - 核心思想：云侧 Agent（Global Orchestrator）掌握全局物品图谱和跨用户知识；端侧 Agent（Personal Assistant）完全掌握用户隐私偏好。两者通过自然语言/Prompt 接口协商推荐列表。
  - 当前进展（CDA4Rec 等）还处于早期，Agent 间的交互协议、意图理解、任务分解都远未成熟。
  - 这是 Ch9（Future Directions）中 Agentic Collaboration 的技术基础。

### 7.4 Production Systems and Industrial Practice

- **写作要点**：
  - 将前面讨论的方法落地到工业系统的总结视角。
  - 代表系统：
    - **淘宝**：EdgeRec → Meta Controller → IntellectReq → LSC4Rec 的技术演进线。
    - **快手/抖音**：端侧重排 + 实时更新（Real-time Short Video Rec, CIKM 2022）。
    - **TiVo**（2004）：最早的去中心化工业推荐系统，历史视角。
    - **NestPipe**（2026）：1500+ 加速器的大规模推荐训练，工业规模下的流水线并行。
  - 关键经验：
    - 工业系统追求的是"在给定的延迟/成本约束下最大化推荐质量"，而非理论最优。
    - 端云协同在工业界的采纳是渐进式的：先从端侧重排开始，逐步扩展到端侧训练、端云协同推理。
  - 与 Ch8 的衔接：工业系统的评估需要仿真平台和标准数据集。

### 7.5 Chapter Summary

- **写作要点**：
  - 任务编排是 DCRec "从可用到好用"的关键层。
  - 与 Ch5（计算）和 Ch6（模型）协同：好的任务编排需要高效的计算分配和合理的模型共享策略作为基础设施。
  - LLM 时代正在重塑任务编排的基本范式——从"参数同步"到"语义协作"。

---

## Chapter 8. Evaluation, Platforms, and Datasets

> **目标**：5-6 页。综述 DCRec 研究的实验生态，为后续研究者提供可复现的基础设施参考。
>
> **背景**：增补此章使综述体系完整。

| 数据集/平台 | 类型 | 规模 | 时间戳 | DC 适配方案 | 不足 |
|:---|:---|:---|:---|:---|:---|
| MovieLens-25M | 评分 | 162K users, 62K items | 有 | 按 User ID 分割 | 规模小，无设备异构 |
| Amazon Reviews | 评论 | 百万级 | 有 | 按 User ID 分割 | 缺乏会话结构 |
| Yelp | POI+评论 | 百万级 | 有+地理 | 按 User ID + 时间分割 | 单域 |
| Taobao/Alimama | 工业 | 亿级 | 有 | 已有 DC 实验设置 | 获取门槛高 |
| Flower | FL 仿真平台 | 可配置 | N/A | 原生支持 FedRec | 缺推荐专用 DSL |
| FedScale | FL 基准 | 可配置 | N/A | 含推荐任务 | 缺端侧推理模拟 |

### 8.1 Evaluation Metrics

- **写作要点**：
  - 传统推荐指标：Precision@K, Recall@K, NDCG@K, MAP, MRR, AUC（排序质量）。
  - DC 特有指标：
    - **通信效率**：通信轮数、每轮传输数据量（KB/MB）、总通信时间。
    - **设备资源**：端侧推理延迟（ms）、模型存储大小（MB）、峰值内存、电量消耗。
    - **隐私度量**：差分隐私预算 $\epsilon$、成员推理攻击成功率、属性推理攻击成功率。
    - **系统吞吐**：支持的最大并发端侧连接数、云端 QPS。
  - 隐性指标：模型更新频率、冷启动恢复速度、用户兴趣漂移跟踪能力。
  - 建议构建一个统一的评估矩阵：每个 DC 方法至少应报告排序质量 + 通信开销 + 端侧资源消耗 + 隐私保障四维指标。

### 8.2 Simulation Platforms and Frameworks

- **写作要点**：
  - 通用 FL 仿真平台：
    - **Flower**：最广泛使用的 FL 框架，支持多种推荐模型的联邦训练。
    - **FedScale**：面向大规模 FL 的基准平台，包含推荐任务。
    - **TensorFlow Federated / PySyft**：学术界常用的 FL 仿真工具。
  - 推荐专用仿真器：
    - **EdgeRecSim**（非公开，工业内部）：淘宝端侧推荐仿真。
    - **DCRec-Bench**（identified need）：目前缺乏统一的 DC 推荐基准；本文将该需求识别为重要的社区方向（见 Ch9.5），而非已交付的 artifact。
  - 仿真 vs 真实部署的差异：用户行为模拟的逼真度、网络条件的简化、设备异质性的建模不足。
  - 建议：在已有的 FL 平台基础上，增加推荐特有的仿真组件（用户行为序列生成器、物品冷启动模拟器、Embedding 存储模拟器）。

### 8.3 Datasets

- **写作要点**：
  - 构造 DC 实验数据集的要求：用户维度 > 物品维度，带时间戳，最好有设备/会话级别的分割。
  - 常用公开数据集与 DC 适配方法：
    - **MovieLens-1M/10M/25M**：经典但用户数少（≈6K-160K），常用于 Proof-of-concept。
    - **Amazon Reviews**：按用户 ID 分割模拟端侧本地数据（每个用户 = 一个设备）。
    - **Yelp**：带地理和时间戳，适合 POI 推荐的 DC 仿真。
    - **Steam / LastFM**：适合序列推荐的 DC 仿真。
    - **Taobao / Alimama**：工业级数据集，规模大但获取门槛高。
  - 数据分割策略：如何从公开数据集中构建"端侧本地数据 + 云端全局数据"的实验设置。
    - 按用户 ID 分割：每个用户的行为序列 = 该设备的本地数据。
    - 按时序分割：旧数据放云端、新数据放端侧（模拟日志回流延迟）。
  - 当前数据集的核心不足：缺乏真实的设备异构信息（设备型号、网络类型、电量状态等），导致仿真结果与真实部署差距显著。

---

## Chapter 9. Challenges and Future Directions

> **目标**：5-6 页。分为两类方向：(1) 对现有方法的改进方向 (2) LLM 时代催生的全新范式。
>
> **写作原则**：每条方向需回答"为什么是未来"（动机）、"现在做到什么程度了"（现状）、"需要什么突破"（挑战）。

### 9.1 Speculative Decoding for On-Device LLM Inference

- **写作要点**：
  - 动机：LLM 自回归生成的延迟（百毫秒到秒级）无法满足在线推荐（<50ms）。投机解码是端云协作加速 LLM 推理的自然方案。
  - 技术构想：端侧部署微型 LLM（0.1-0.5B）作为 Draft Model 快速生成候选物品序列；云端 LLM（8-70B）进行批处理验证。
  - 推荐场景的独特优势：推荐中的"物品"是有限词汇表（Item Vocab），可以在 Draft Model 中专门优化 Item 令牌的预测准确性。
  - 当前状态：投机解码在通用文本生成中已有成功实践，在推荐场景中尚属空白。
  - 挑战：Draft-Target 的接受率在推荐场景中的特性、KV Cache 的端云共享与卸载、串行验证的延迟上限。

### 9.2 Parameter-Efficient Federated Fine-Tuning for Recommendation

- **写作要点**：
  - 动机：端侧无法对数十亿参数的基础模型进行全参微调。PEFT（LoRA/Adapter/Prompt Tuning）只更新 <1% 的参数，天然适合端侧联邦学习。
  - 技术构想：云端冻结 FM，端侧仅微调轻量 Adapter 模块。云端定期聚合个性化 Adapter 进行全局知识融合 → Federated LoRA。
  - 当前进展：FedPA (IJCAI 2024)、TransFR (AAAI 2026) 已初步探索，但尚未系统化。
  - 挑战：
    - LoRA rank 与通信效率/表达能力的最优权衡。
    - 不同用户/域的 LoRA 模块如何有效聚合（LoRA 空间是非欧的，简单加权平均可能不合理）。
    - 端侧推理时 Adapter 与基础模型的融合延迟。

### 9.3 Agentic Collaboration: Multi-Agent Cloud-Device Recommendation

- **写作要点**：
  - 这是未来 3-5 年最宏大的方向：端云协同从"张量交换"升级为"智能体对话"。
  - 技术构想：
    - 云侧 Global Orchestrator Agent：掌握全域物品图谱、趋势检测、探索策略规划。
    - 端侧 Personal Assistant Agent：掌握用户隐私偏好、本地上下文、设备状态。
    - 交互方式：自然语言/结构化 Prompt（而非梯度/参数）。
  - 优势：模型架构完全解耦（云端 LLM 可以是任意厂商的）、天然可解释（交互日志 = 自然语言日志）、隐私更优（不传输模型参数）。
  - 当前进展：CDA4Rec (2025)、MemRec (2026) 已展示 Agent 在推荐中的潜力，但距离端云 Agentic 协作还有显著差距。
  - 挑战：Agent 间通信协议的标准化、意图理解的可靠性、多轮对话延迟的可控性、Agent 幻觉的抑制。

### 9.4 Synchronized On-Device RAG

- **写作要点**：
  - 动机：RAG 是 LLM 推荐中缓解幻觉的关键技术，但纯云端 RAG 需要检索千万级 Item Vector，成本高且无法感知端侧最新状态。
  - 技术构想：Cloud-Device Distributed RAG。
    - 云端维护全量物品向量数据库（Vector DB）。
    - 端侧基于用户近期画像，动态预加载一个微型个性化子索引（Sub-Index），本地缓存近期热门且匹配用户兴趣的物品 Embedding。
    - 推理时优先本地 RAG 检索；Cache Miss 时向云端发起检索。
  - 端侧的优势：本地 RAG 索引天然包含了用户最新交互（无需日志回流）。解决了 LLM 推荐的"新鲜度 + 幻觉"双重问题。
  - 挑战：子索引的更新策略（何时刷新、刷新多少）、同步的通信成本、端侧向量检索的延迟（近似近邻搜索在资源受限设备上的性能）。

### 9.5 Unified Evaluation and Benchmark for DCRec

- **写作要点**：
  - 动机：当前 DCRec 研究各自采用不同的数据集、模拟器、指标，实验可复现性和方法间的公平比较严重不足。
  - 建议：构建 DCRec-Bench —— 统一的 DC 推荐评估基准。
    - 标准化数据集分割方案（User-split + Time-split 的复合策略）。
    - 统一的仿真环境配置（网络带宽/延迟/丢包率、设备算力/内存档位）。
    - 多维度评估体系（推荐精度 + 通信成本 + 端侧资源 + 隐私保护的联合报告）。
  - 预期影响：加速领域收敛、降低新方法进入的门槛、促进学术界与工业界的对齐。

### 9.6 Beyond Recommendation: General Cloud-Device Collaborative Intelligence

- **写作要点**：
  - DCRec 的许多技术贡献（PEFT Fed、Slow-Fast Learning、端云 Agent 协作）可以溢出到更广泛的端云 AI 场景。
  - 潜在迁移场景：
    - 端云协同的个性化教育（数学题的难度和类型因人而异）。
    - 端云协同的健康监测（个人生理数据隐私 + 全局医学知识）。
    - 端云协同的自动驾驶（本地实时决策 + 云端全局地图/交通规划）。
  - 呼吁：DCRec 社区的研究者为更广泛的端云协作智能提供方法论。

---

## Chapter 10. Conclusion

> **目标**：1-2 页，简洁有力。

### 10.1 Summary of Contributions

- **写作要点**：
  - 重申本文对端云协同推荐领域的系统性梳理：从 OC 和 OD 的背景出发 → 提出 DCRec 统一框架 → 按 Computation-Model-Task 三维机制分类 → 系统综述 200+ 篇文献 → 指出 LLM 时代的前沿方向。
  - 强调"双循环个性化"作为全文的概念主线。

### 10.2 Key Takeaways

- **写作要点**（每点 1-2 句话）：
  1. DCRec 是推荐系统的必然演进方向——OC 的隐私与延迟瓶颈 + OD 的协同信号与候选空间瓶颈，使得端云结构化协同成为系统性最优方案。
  2. 端云协同的核心是分工，不是简单的"同时存在"——每种机制都定义了明确的分工逻辑。
  3. LLM 不仅增强了推荐能力，更重构了协同范式——从参数共享到语义协作。
  4. 工业实践领先于学术理论——工业界（淘宝、快手等）已有成熟 DC 系统，学术界需要建立统一的理论和评价体系。

### 10.3 Closing Remarks

- **写作要点**：
  - 对未来 5 年的展望：端云协同推荐将从"专用系统"演变为"通用智能基础设施"。
  - 呼吁学术界与工业界建立更紧密的合作，推动 DCRec 从"经验驱动"走向"原理驱动"。

## References
