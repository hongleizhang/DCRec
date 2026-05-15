# Device-Cloud Personalization and Collaboration in Recommender Systems: A Systematic Survey in the Large-Model Era

> **Architecture v4.0** | **Target Venue: IEEE TKDE** | **Page Budget: ≤20 pages total (≤16 pages double-column body + ~4 pages references)**
> OC / OD / DC 三章并列；Preliminaries 紧跟 Introduction，为三章提供统一的形式化语言与挑战分析；DCRec 不再拆分为多章，与 OC、OD 平行构成三大主体内容。
> 八章节结构：Introduction → Preliminaries → OC → OD → DC → Evaluation → Future → Conclusion。
> **个性化框架更新**：三种推荐范式对应三种差异化个性化——OCRec（Contextual Personalization，基于全局上下文和协同信号的个性化）、ODRec（Real-Time Personalization，基于本地实时数据的毫秒级个性化）、DCRec（Utility-Privacy-Efficiency Collaborative Personalization，在端云协同中平衡推荐效用、隐私保护与系统效率的个性化）。旧版 Dual-Loop Personalization 已替换为此三维个性化框架。
>
> **三条编辑规则（此版新增，所有章节修改必须遵守）**：
>
> 1. **单一主归属**：一篇论文只能在一个章节被作为"代表性方法"展开分析。跨维度方法通过辅助标签（如 `[Computation: Early Exit]`）在其他章节交叉引用，但正文分析不重复展开。
> 2. **Ch4 纯 OD / 以端为主**：Ch4 只保留纯端侧或"以端为主、无显式云协同"的方法。只要存在显式云侧协同角色（云端参与训练/推理/策略下发），正文主分析就放 Ch5。
> 3. **Ch6 必选，但高度压缩**：按 systematic survey 的方法学标准，Evaluation & Datasets 为必选章。TKDE 版压缩至约 1 页双栏（三段式：指标讨论 + 数据集表格 + Benchmark Gap），不做 optional 预留。

---

## Abstract

- **核心要素确认**：
  1. **Problem**: 推荐系统正从纯云架构转向端云协同智能。OC 面临延迟/隐私/成本瓶颈，OD 受限于存储/算力/协同信号。
  2. **Taxonomy**: 提出按协同目标（collaborative objective）的三机制主分类——Computation Allocation / Model Sharing / Task Orchestration——配合明确的归属规则与辅助标签系统。三种推荐范式对应三种差异化个性化：OCRec 的 Contextual Personalization、ODRec 的 Real-Time Personalization、DCRec 的 Utility-Privacy-Efficiency Collaborative Personalization。
  3. **Coverage**: 系统综述 200+ 篇文献（检索窗口 2004-2026.04），覆盖 OC 三代演进、OD 部署与适配技术、以及 DCRec 的三维协同机制。
  4. **Directions**: 展望 LLM 时代的前沿方向（投机解码、Federated PEFT、Agentic 协作、On-Device RAG）。
- **关键词建议**：Device-Cloud Collaborative Recommendation, Federated Recommendation, On-Device Recommendation, Large Language Models, Personalization.

---

## 论文整体结构概览

| 章节 | 定位 | 页数 |
|:---|:---|:---:|
| Ch1 Introduction | 问题引入、方法学、贡献声明 | ~2.5 |
| Ch2 Preliminaries | 统一形式化语言、推荐系统独特挑战、三种个性化范式 | ~1.5 |
| Ch3 On-Cloud Recommendation | ★ 主体贡献：OC 三代演进系统化重组 | ~2.5 |
| Ch4 On-Device Recommendation | ★ 主体贡献：OD 部署+适配分类体系 | ~2.0 |
| Ch5 Device-Cloud Collaborative Recommendation | ★ 主体贡献：DCRec 范式定义、划界与三机制分类 | ~3.5 |
| Ch6 Evaluation & Datasets | 实验生态 | ~1.0 |
| Ch7 Challenges and Future Directions | 前瞻 | ~1.5 |
| Ch8 Conclusion | 总结 | ~0.5 |
| **Total** | | **~15.0 页正文 + ~4 页参考文献** |

> **设计理念**：
>
> 1. **三章并列贡献**：OC、OD、DCRec 各占一主力章节。OC 和 OD 虽为相对成熟领域，但本文投入大量工作进行了系统化的重分类与重新整理，为新人提供完整的入门路径，这本身就是论文的重要贡献。DCRec 作为新兴领域占据最大篇幅，承担定义、划界、分类与展望的功能。
> 2. **Preliminaries 紧接引言**：不再将形式化定义和挑战分析局限于 DC 范畴。Preliminaries 提供贯穿全文的统一符号体系、推荐系统独有的三大挑战分析，以及对 OC / OD / DC 均适用的底层设计原则。OC、OD 两章的行文也会在相应位置呼应这些 Preliminaries 中的概念。
> 3. **DCRec 不再拆分**：旧架构将 DCRec 拆为 Taxonomy + 三大机制 + Evaluation + Future 共六章，导致 DC 在章节数量上"压倒"OC/OD，但正文内容分散。新架构将 DCRec 的主体内容（定义、划界、三机制分类、代表性方法、工业实践）整合为一章，与 OC、OD 并列，突出三者同为本文核心贡献的平等地位。
> 4. **OC 与 OD 不再是"背景"**：旧架构将 OC/OD 标记为 Background，暗示其仅为基础铺垫。新架构将这两章定位为高质量系统化重组（systematic reorganization），为新人提供完整的入门路径。最强的原创性（范式定义、划界、机制分类）保留给 Ch5。
> 5. **Ch6 为必选章，但高度精简**：按 systematic survey 的方法学要求，评估生态综述为必选。TKDE 版压缩至 1 页双栏（数据集表格 + 指标讨论 + Benchmark Gap 三段式），不做 appendix 预留。
> 6. **Ch3/Ch4 的定位**：不是弱原创，而是"高质量系统化重组"（systematic reorganization）。措辞用 `new organizing lens` / `systematic taxonomy`，但最强的原创性（范式定义、划界、机制分类）保留给 Ch5。鉴于 TKDE 页数限制，这两章每节约写 0.7-0.8 页双栏，以代表性方法抓主干、3-4 篇 per subsection 展开。
> 7. **Introduction 中显式声明** "Chapters 3-5 form the core contributions, with Ch3-4 providing systematic reorganizations of established paradigms and Ch5 formally defining the emerging DCRec paradigm" 作为叙事锚点。
> 8. **TKDE 页数策略**：正文 15 页双栏 + 参考文献 4 页 = 19 页（留 1 页 buffer）。正文中图表占用约 3-4 页，纯文字 11-12 页约 9000-10000 词。每章写作务必控制在该页数预算内，超出部分移入 Appendix（arXiv 版本保留完整细节，TKDE 版本精简）。

---

## Chapter 1. Introduction

> **目标**：~2.5 页双栏（约 2000 词 + 1 图），交代推荐系统背景、与已有综述的定位差异、OC/OD 瓶颈、DC 范式动机、本文三元并列的贡献概览、Contributions 列表、全文组织结构。注：Coverage Map 对比表占用约 1/4 页，需预留空间。

### 1.1 Scope, Related Surveys, and Survey Methodology

> **TKDE 要求**：Systematic Survey 必须交代文献检索与筛选的方法学骨架，包括检索库、检索式、时间范围、筛选流程与排除标准。

#### 1.1.1 Survey Protocol and Paper Selection Criteria

- **检索库与检索策略**：
  - 检索数据库：DBLP, Google Scholar, Semantic Scholar, ACM Digital Library, IEEE Xplore。
  - 核心检索词组合：("device-cloud" OR "cloud-edge" OR "cloud-device") AND ("recommendation" OR "recommender system")；单独检索的辅助方向："federated recommendation", "on-device recommendation", "distributed recommendation", "decentralized recommendation"。
  - 检索时间窗口：**2004 年 1 月 – 2026 年 4 月**（TiVo 为最早可追溯的去中心化推荐工业系统，2026 年 4 月为本文检索截止日）。
  - 去重后初始命中：约 800–1000 篇。

- **筛选流程**（三层过滤）：
  1. **Title-level screening**：排除明显不相关的文献（如纯边缘计算调度无推荐、纯推荐无端云协作元素）。通过率约 60%。
  2. **Abstract-level screening**：排除仅将"端"作为数据采集界面而无实质性协同机制的工作。通过率约 50%。
  3. **Full-text review**：按纳入标准确认每篇论文是否符合本文 scope，并标注其归属章节。最终纳入 **200+ 篇**。

- **纳入 / 排除标注协议**：
  - 每篇论文记录：归属章（OC / OD / DC）、DC 论文的主分类（Computation / Model / Task）与辅助标签、方法名、年份、主要优化目标、模型结构、评估数据集、关键指标。
  - 多作者独立标注 + 交叉验证。

- **可复现性**：
  - 附录或 arXiv 版本中提供最终纳入的完整文献列表（按 README.md 的三级分类组织）及相应的检索窗口声明，供审稿人核查覆盖完整性。

- **局限性声明**：
  - 检索以英文文献为主，可能遗漏部分中文/非英语社区的 DC 推荐工作。
  - 部分 2025–2026 的 arXiv preprint 截止日前尚未经过同行评审，纳入时以标题/摘要判断其 topic fit，不代表 endorsement。
  - 工业界工作多以系统设计报告而非开源复现形式存在，实验对比受限于已公开信息。

#### 1.1.2 Related Surveys

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
    1. **三维全覆盖**：已有综述要么只覆盖端云协同 AI（不聚焦推荐），要么只覆盖推荐系统的某一片段。本文将 On-Cloud + On-Device + Device-Cloud 三重范式统一在一篇综述中，且每个方向都提供了系统性的分类整理。
    2. **OC 与 OD 的重新分类**：已有综述对 OC 多以时间线/技术代际简单切分，本文进一步在每个代际内部按技术范式做子分类（如 Shallow CF 内的 Memory/Model/Content-based 三分、Deep 内的 Feature/Sequential/Graph 三分、Large Semantic 内的 Instruction/Generative/Agentic 三分）；对 OD 以"部署轻量化 → 在线适配"的两阶段框架重新组织，在适配阶段进一步按优化目标（Privacy/Latency/Resource）做细粒度分类。这些分类工作本身就是对领域知识的系统化贡献。
    3. **DCRec 的定义与划界**：已有综述尚未将 DCRec 作为独立范式进行定义。本文首次提出按 collaborative objective 的三机制主分类，并制定明确的归属仲裁规则与辅助标签系统，为后续研究者提供了可操作的分类框架。
    4. **LLM 时代的整合视角**：不仅包括隐私优先的联邦推荐，也包括性能优先的协同推理、LLM 时代的语义协同等更广泛的 DC 范式。

### 1.2 Recommender Systems: From Centralized to Collaborative

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
    - 存在云-端双向信息流动（参数、梯度、候选集、软标签、置信度信号等）
    - 云端和端侧在推荐流程中承担互补角色
    - 端侧在推荐流程中承担实质性角色：模型训练（如 FedRec 中的本地 SGD）、推荐推理（如端侧重排）、或排序决策（如最终展示列表的选择）
    - 协同机制对精度、延迟或隐私有可量化贡献
  - **与 FedRec 的兼容性说明**：上述第三条中"模型训练"的纳入明确覆盖了那些核心贡献在训练与聚合、而非端侧最终 serving 的联邦推荐工作。这些工作在本文 Ch5.4（Model Sharing）中被视为联邦推荐的主体文献群——它们的端侧角色是"在本地数据上训练模型并将参数/梯度贡献给全局聚合"，这满足"端侧承担实质性角色"的标准。
  - 阐明 DCRec 与 Federated Rec 的关系：我们认为 FedRec 可以视作 DCRec 在强隐私约束下的特例——当 $\mathcal{D}^{\text{cloud}}=\emptyset$ 且协同信息仅限于梯度/参数聚合时，DCRec 退化为联邦推荐。DCRec 还覆盖 Performance-first、Cost-first 等更宽松约束下的协作目标（如云召回+端重排、LLM 协同推理）。
  - 过渡句：本文从 Personalization 和 Collaboration 双视角出发，提出统一的分类框架，覆盖 OC、OD 与 DCRec 三大范式。

### 1.6 Scope, Contributions, and Paper Organization

- **写作要点**：
  - **Contributions**（4-5 条）：
      1. To the best of our knowledge and within the defined search scope (2004–2026.04), the first survey that covers the full spectrum from on-cloud to on-device to device-cloud collaborative recommendation under a unified framework, covering 200+ papers.
      2. A systematic reorganization of OC and OD: a three-generation taxonomy for OC with fine-grained sub-categorization within each generation (Memory/Model/Content → Feature/Sequential/Graph → Instruction/Generative/Agentic); a two-stage deployment-adaptation framework for OD with adaptation broken down by optimization objective (Privacy/Latency/Resource). These provide a structured on-ramp for newcomers to both established fields.
      3. The formal definition and boundary delineation of DCRec as a distinct paradigm, and a primary classification of DCRec methods into three mechanisms based on collaborative objective (Computation Allocation / Model Sharing / Task Orchestration), with explicit placement rules and auxiliary tags for cross-dimensional methods.
      4. Summarization of evaluation benchmarks, simulation platforms, and datasets for DCRec research.
      5. Identification of key open directions in the large-model era, structured as evidence-backed trends vs. speculative outlook.
  - **Paper Organization 段落**：
    - Ch2 (Preliminaries)：建立统一的形式化框架与推荐系统的三大独特挑战，为后续三章提供共同的概念基础（注：Ch3-5 需在相应位置显式回钩 Ch2 的概念与符号）。
    - Ch3 (On-Cloud Recommendation)：对 OC 三代演进进行系统化重组，每代内部按技术范式做子分类。
    - Ch4 (On-Device Recommendation)：以"部署轻量化 → 在线适配"两阶段框架重组 OD 技术，适配阶段按优化目标三分。
    - Ch5 (Device-Cloud Collaborative Recommendation)：正式定义 DCRec 范式，提出三机制主分类，系统综述各机制的代表性方法与协同策略。
    - Ch6 (Evaluation & Datasets)：实验生态与基础设施现状。
    - Ch7 (Challenges and Future Directions)：LLM 时代的前沿趋势与远期愿景。
    - Ch8 总结全文。

---

## Chapter 2. Preliminaries

> **目标**：~1.5 页双栏（约 1200 词 + 1 矩阵表）。为全文建立统一的形式化语言、定义推荐系统区别于 CV/NLP 的独特挑战，并引入 OC/OD/DC 三种个性化范式。本章是所有后续章节的概念基础——Ch3-5 将反复引用此处的符号、定义与挑战。
>
> **设计理念**：
>
> - 与旧架构不同，Preliminaries 不再是"DC 专有的预备知识"，而是面向推荐系统整体的通用框架。
> - Ch3 (OC) 在讨论各代模型时会提及这些挑战在云侧的体现形式（如 Embedding 规模增长驱动的技术演进）；Ch4 (OD) 会将三大挑战映射到端侧的约束空间（存储/算力/协同信号缺失）；Ch5 (DC) 则将三大挑战作为 DCRec 机制设计的核心优化对象。

### 2.1 Problem Formulation

- 统一数学符号：
  - $\mathcal{U} = \{u_1, \ldots, u_N\}$: 用户集合
  - $\mathcal{I} = \{i_1, \ldots, i_M\}$: 物品集合
  - $\mathcal{D}^{\text{global}}$: 所有可用的交互数据（在 OC 场景中集中存放于云端；在 OD 场景中分散于各设备且不可直接聚合；在 DC 场景中端云各有副本或互补子集）
  - $\mathcal{D}_u^{\text{local}}$: 用户 $u$ 的端侧本地数据
  - $\mathcal{D}^{\text{cloud}}$: 云端可访问的数据（脱敏交互日志、物品信息、跨用户统计等）
  - $\theta_u$: 用户 $u$ 的个性化模型参数
  - $\Phi$: 全局/云端模型参数
- 联合学习目标（DC 最一般形式）：
  $$\min_{\{\theta_u\}, \Phi} \sum_{u \in \mathcal{U}} w_u \cdot \mathcal{L}_u(\theta_u, \Phi; \mathcal{D}_u^{\text{local}}, \mathcal{D}^{\text{cloud}}) + \mathcal{R}(\{\theta_u\}, \Phi)$$
  其中 $\mathcal{R}$ 是端云协同引入的正则/先验/约束。
- 协同约束：通信带宽 $B$、延迟预算 $T$、隐私预算 $\epsilon$。

- **形式化在不同范式中的特化**：
  - **OC（纯云）**：$\mathcal{D}_u^{\text{local}} = \emptyset$，所有数据在 $\mathcal{D}^{\text{cloud}}$，$\theta_u$ 退化为从 $\Phi$ 派生的表示。
  - **OD（纯端）**：$\mathcal{D}^{\text{cloud}} = \emptyset$，每个 $\theta_u$ 仅从 $\mathcal{D}_u^{\text{local}}$ 学习，$\Phi$ 可能为空或作为不可训练的静态初始化。
  - **DC（端云协同）**：端云各有数据子集且互为补充，联合优化同时受隐私与通信约束。
  - 这一统一形式化明确了三种范式的本质差异在于数据分布与协同约束的不同配置。

- **安全与隐私作为跨切面约束**：本文不在主分类之外单独设立安全/隐私维度，而是将其视为贯穿所有范式的跨切面约束。在 OC 中隐私约束表现为数据脱敏与合规存储；在 OD 中表现为数据不离开设备；在 DC 中表现为差分隐私、安全聚合等端云通信隐私保障。

### 2.2 Three Distinctive Challenges of Recommender Systems

> 推荐系统相较于 CV/NLP 具有三项独特特征。这些特征并非仅存在于 DC 场景，而是在 OC、OD 和 DC 中均有不同的表现形式与影响程度。理解这些挑战在三种范式中的差异化表现，是理解本文分类逻辑与机制设计的关键。

#### 2.2.1 Natural Data Heterogeneity

- **写作要点**：
  - 推荐系统中的"每个用户就是一个分布"，行为模式、偏好强度、活跃度高度异质。
  - 在三种范式中的表现：
    - **OC 中**：数据异质性表现为推荐长尾效应——头部用户数据充足、尾部用户数据稀疏。常规的全局模型训练可能过度拟合活跃用户而忽视沉默用户。
    - **OD 中**：异质性是最核心的优势——端侧模型天然适配单用户分布，不存在"被平均"的问题。但这也意味着端侧无法从相似用户的数据中获益。
    - **DC 中**：异质性从"数据集偏差"升级为"系统级约束"——统计异质性（Non-IID）导致联邦聚合退化（Client Drift），行为异质性要求端侧自适应策略不能一刀切，场景异质性使得云侧缺乏的实时上下文恰好是端侧的优势。
  - 对后续章节的映射：Ch3 中异质性表现为模型从粗粒度到细粒度的个性化演变趋势；Ch4 中表现为为什么需要端侧适配；Ch5 中表现为 DC 各机制的个性化策略设计依据。

#### 2.2.2 Embedding-Centric Model Structures

- **写作要点**：
  - 推荐模型与 CV/NLP 模型最本质的差异：高基数稀疏特征 → 超大 Embedding 表。
  - Embedding 表在三种范式中的不同角色：
    - **OC 中**：Embedding 表规模随模型代际呈指数增长（MB → GB → TB），推动存储架构和分布式训练从可选变为刚需。OC 的三代演进中，Embedding 从简单的查表操作升级为需要分布式 KV 存储、GPU PS 和流水线并行的核心基础设施挑战。
    - **OD 中**：Embedding 表是端侧部署的**第一瓶颈**——90%+ 参数存储占比 → 催生了 Embedding Slimming 这一核心研究方向。端侧无法存储全量 Item Embedding → 需要云侧托管或本地压缩替代方案。
    - **DC 中**：Embedding 承担三重身份——存储瓶颈（端侧存不下）、通信瓶颈（联邦同步时 Embedding 梯度占通信量的主导）、隐私载体（Item Embedding 间接编码用户偏好 → 传输时需隐私保护）。
  - 对后续章节的映射：Ch3 中 Embedding 增长是技术代际跃迁的核心驱动力之一；Ch4 中 Embedding 压缩是轻量化的首要目标；Ch5 中 Embedding 放置、同步与隐私保护是贯穿三机制的关键议题。

#### 2.2.3 Massively Distributed User Devices

- **写作要点**：
  - 工业推荐服务规模：十亿级用户 → 十亿级潜在计算节点。
  - 在三种范式中的表现：
    - **OC 中**：规模挑战体现为海量并发请求 → 云侧需要千卡/万卡集群的分布式推理与弹性扩容。用户设备数量只影响服务端的 QPS 压力，不改变集中式架构。
    - **OD 中**：每个设备独立运行，不存在设备间协调问题。但设备异质性（算力、内存、电量、网络条件差异巨大）使得"一刀切"的端侧方案不可行 → 需要弹性适配策略。
    - **DC 中**：规模带来三个独特挑战：(1) 参与率不可控（用户授权、电池、网络）；(2) 设备异质性跨越 2-3 个数量级；(3) 通信不可靠（移动网络下设备随时离线）；(4) 安全攻击面随端侧节点数线性扩大。
  - 对后续章节的映射：Ch5 各机制的分布式架构设计（Ch5.3）、联邦聚合策略（Ch5.4）、任务动态调度（Ch5.5）均需应对此挑战。

#### 2.2.4 Challenge × Paradigm Cross-Matrix

- **写作要点**：
  - 用一张 3×3 交叉矩阵表展示每个挑战在三种范式中的核心程度：

    |                         | OC (Ch3) | OD (Ch4) | DC (Ch5) |
    |:------------------------|:--------:|:--------:|:--------:|
    | Data Heterogeneity      | ○ (长尾/稀疏 → 个性化建模驱动力) | ● (核心优势：天然适配单用户) | ● (Client Drift → 个性化聚合核心挑战) |
    | Embedding-Centric       | ● (驱动分布式存储架构演进) | ● (端侧部署第一瓶颈) | ● (存储+通信+隐私三重挑战) |
    | Massively Distributed   | ○ (并发服务压力) | ○ (设备异构 → 弹性适配需求) | ● (参与率+可用性+攻击面) |

  - **作用**：
    1. 这张表展示了三种范式面对同一组挑战时的不同侧重，是全文组织逻辑的导航图。
    2. 它解释了为什么 OD 需要轻量化技术（因为 Embedding 瓶颈最突出），为什么 DC 需要三维联动（因为没有单一机制能同时覆盖全部挑战的三列全 ●），以及为什么 OC 的演进史实质上是不断升级基础设施以应对 Embedding 规模与用户并发量的历史。
    3. DC 是唯一在三个挑战维度都面临系统级约束的范式——这正是它需要 Computation + Model + Task 三维机制的根本原因。

### 2.3 Personalization Paradigms Across OC, OD, and DC

> 个性化是推荐系统的核心目标，但在三种范式中的实现方式有本质差异。我们按个性化所依赖的核心资源将其区分为三种类型——Contextual、Real-Time、Collaborative——分别对应 OC、OD、DC 三大范式。

- **写作要点**：

  - **Contextual Personalization（OCRec，上下文个性化）**：
    - 核心资源：全局交互历史 + 跨用户协同信号 + 物品内容/语义知识。
    - 个性化方式：在全局数据上训练统一模型，用户的个性化表示通过其历史交互在全局 Embedding 空间中被编码。个性化是"从全局信号中推导出的个体差异"，而非针对单个用户的独立优化。
    - 核心优势：受益于海量数据和强表达能力，能捕获长尾兴趣和深层语义关联。
    - 核心局限：离线训练 + 在线推理的分时架构导致个性化响应滞后（分钟至小时级），无法捕捉秒级兴趣漂移；隐私数据集中在云端。
    - 实例化形式：User Embedding / 用户历史序列作为全局模型输入 → 全局模型输出个性化排序。

  - **Real-Time Personalization（ODRec，实时个性化）**：
    - 核心资源：本地实时行为流 + 端侧传感器上下文 + 零网络延迟决策。
    - 个性化方式：端侧模型以用户本地数据为唯一优化目标，每次交互后毫秒级更新表征。个性化是"针对单一用户的持续在线适应"，不存在"被全局平均化"的问题。
    - 核心优势：零延迟推理与模型更新、数据绝对隐私、离线可用。
    - 核心局限：缺乏跨用户协同信号（信息茧房）、模型容量受设备严格限制、候选空间局限于端侧缓存。
    - 实例化形式：本地模型 $θ_u$ 仅从 $D_u^{\text{local}}$ 学习 → 纯端侧推理与更新。

  - **Utility-Privacy-Efficiency Collaborative Personalization（DCRec，协同个性化）**：
    - 核心资源：云端全局知识 + 端侧实时上下文，通过协同机制动态组合。
    - 个性化方式：将个性化拆分为云侧与端侧两个可协同的子问题——云侧负责"全局理解"（Contextual 的基础），端侧负责"实时响应"（Real-Time 的基础），通过 Computation / Model / Task 三类协同机制在二者之间建立信息桥梁。个性化强度 $P$ 由端侧自适应深度 $d_{\text{local}}$ 与云端干预频率 $f_{\text{cloud}}$ 共同决定。
    - 核心优势：$P$ 可动态调节——兴趣稳定时增大 $f_{\text{cloud}}$ 依赖全局上下文，兴趣漂移时增大 $d_{\text{local}}$ 强化实时适应。隐私预算 $\epsilon$、通信预算 $B$、延迟预算 $T$ 作为三维约束共同界定个性化可行域。
    - 核心局限：协同机制引入额外的通信开销与系统复杂度；个性化质量高度依赖协同协议的设计。
    - 实例化形式：云侧模型 $\Phi$ 端侧模型 $\{\theta_u\}$ 通过协同协议交换信息 → 联合优化的个性化。

  - **三种个性化的关系**：Contextual Personalization 和 Real-Time Personalization 分别是 Collaborative Personalization 的两个极端特例——当端侧贡献降为零时 DCRec 退化为 OC，当云端贡献降为零时 DCRec 退化为 OD。三种个性化构成一个连续谱，DCRec 的价值在于提供了在谱上任意位置定制个性化方案的能力。

  - **配图建议**：三维个性化对比表，列出 Contextual / Real-Time / Collaborative 在核心资源、响应延迟、隐私水平、协同信号覆盖四个维度的定性对比。

### 2.4 Chapter Summary and Usage Guide for Subsequent Chapters

> Ch2 是全文的"概念语言"——后续三章不重复定义，而是持续回钩此处的符号、挑战与设计原则。以下为各章与 Ch2 的显式关联约定，落稿时需在对应位置落实：

- **Ch3（OC）回钩点**：
  - §3.1-3.3 每代结尾标注该代际主要应对 Ch2.2.4 交叉矩阵中的哪些挑战（如 Shallow CF 的 Embedding 瓶颈较轻，Deep 和 Large Semantic 时代 Embedding 规模成为驱动分布式存储的关键因素 → Ch2.2.2）。
  - §3.4 章末回归 Ch2.3 的 Contextual Personalization：OC 擅长基于全局上下文的个性化，但缺乏实时响应能力是其核心局限。

- **Ch4（OD）回钩点**：
  - §4.1 开头显式标注 OD 面临的三大约束分别对应 Ch2.2 的哪项挑战（存储约束 → Embedding-Centric，算力约束 → Embedding-Centric + Massively Distributed，协同缺失 → Data Heterogeneity 的正反面）。
  - §4.2 在讨论各路适配方案时，标注其优化目标在 Ch2.2.4 矩阵中的位置。
  - §4.3 章末回归 Ch2.3 的 Real-Time Personalization：OD 擅长基于本地数据的实时个性化，但缺乏全局协同信号是其核心局限。

- **Ch5（DC）回钩点**：
  - §5.1 形式化定义直接引用 Ch2.1 的联合优化框架，并标注 DCRec 的特化形式。
  - §5.2 分类框架显式引用 Ch2.2.4 的 DC 列，说明三个主分类各应对哪些挑战。
  - §5.3-5.5 每节开头标注该机制的 Collaborative Personalization 实例化形式（引用 Ch2.3），说明该机制如何结合 Contextual 与 Real-Time 的个人化资源达成协同个性化。
  - §5.6 章末回归 Ch2.2.4 矩阵：DC 是唯一能在三列全 ● 的范式，三维机制协同是其必然选择。

---

## Chapter 3. On-Cloud Recommendation

> **目标**：~2.5 页双栏（约 2000 词 + 1 三代表）。对云侧推荐系统近三十年技术演进进行系统化重组，按三大技术代际组织，每代内部按技术范式做子分类。TKDE 版每子节约 200-300 词覆盖 3-4 篇代表性工作，以设计思想驱动而非方法罗列。
>
> **写作原则**：
>
> - 每代1段总起（建模范式与优化目标），子类每类压缩至 2-3 句代表工作 + 1 句 DC 关联，不再展开个人方法细节。3.1-3.3 各约 0.6-0.7 页双栏。
> - 每代结尾用 1-2 句标注应对哪些挑战（回钩 Ch2.2.4）及 Embedding 规模增长驱动。
> - §3.4 章末 3-4 句回归 Contextual Personalization（Ch2.3）：OC 核心局限是离线训练-在线推理分时架构无法实现实时个性化响应。

| 维度 | Shallow CF (1994-2014) | Deep Representation (2015-2022) | Large Semantic (2022-2026) |
|:---|:---|:---|:---|
| 核心建模 | 矩阵分解 / 相似度 | DNN 特征交互 / 序列 / 图 | LLM 语义理解 / 生成 |
| Embedding 规模 | MB 级 | GB 级 | TB 级（FM 参数） |
| 表达能力 | 线性交互 | 非线性 + 高阶结构 | 常识推理 + 零样本 |
| 推理延迟 | <1ms | 1-10ms | 100ms-10s |
| 典型代表 | MF, BPR, FM | Wide&Deep, SASRec, LightGCN | P5, TALLRec, HSTU |
| DC 启示 | Embedding 是核心抽象 | 序列+图需要全局视角 | 催生云大端小范式 |

![OCRec](figs/OCRec.svg)

### 3.1 Shallow Preference Matching Models

> 浅层协同过滤的核心在于利用用户-物品交互矩阵，通过统计学习方法挖掘潜在偏好模式。按建模范式分为三类：基于记忆的启发式方法、基于模型的学习方法和基于内容的方法。

#### 3.1.1 Memory-Based Collaborative Filtering

- **写作要点**：
  - 基于 User-Item 交互矩阵的启发式方法（UserKNN, ItemKNN, 基于 Pearson/余弦的相似度）。
  - 核心思想：利用"邻居"的评分预测自身偏好。相似度矩阵的计算极易在云端分布式架构下并行化。
  - 代表工作：GroupLens (1994), Item-based CF (2001), FISM (2013)。
  - 优势：直观、可解释、无需训练。局限：冷启动、稀疏性、扩展性差。FISM 将 ItemKNN 与矩阵分解统一到因子模型中，是记忆式方法向模型化过渡的桥梁。
  - 与 DC 的关联：去中心化场景中最简单的协同形式——在网络中通过 Gossip 协议传播评分，无需中心服务器。

#### 3.1.2 Model-Based Collaborative Filtering

- **写作要点**：
  - MF/PMF/BPR/SVD++：将 User-Item 矩阵分解为低维 Latent Vectors。
  - 核心思想：用 Embedding 捕获用户偏好与物品属性的潜在模式。
  - FM（Factorization Machines）：扩展至任意特征交互，成为 CTR 预估的基础组件。
  - 代表工作：PMF (2007), SVD++ (2008), WRMF (2008), BPR (2009), MF (2009), FM (2010), CLiMF (2012), Logistic MF (2014)。
  - 优势：良好的泛化能力、相对紧凑的表示。局限：线性交互能力有限、难以建模时序与高阶特征。
  - 与 DC 的关联：MF 的低维 Embedding 天然适合分布式存储与通信。Federated MF 是最早的 DC 实践形态之一。

#### 3.1.3 Content-Based Collaborative Filtering

- **写作要点**：
  - 利用物品内容或辅助信息构建用户偏好画像。
  - 代表工作：Syskill & Webert (1996), Recommendation as Classification (1998)。
  - 与 DC 的关联：内容特征作为"端云公共信息"的原始雏形——物品属性可以在云端和端侧同时访问，不受隐私限制。

#### 3.1.4 Summary: What Shallow Models Established

- Embedding 是推荐系统的核心抽象。CF 信号依赖全局数据，构成了端侧模型的核心矛盾。
- 线性模型的表达能力上限，驱动了下一代深度方法的兴起。

### 3.2 Deep Representation Learning Models

> 深度表征学习标志着推荐从线性模型向非线性表示学习的跃迁。按神经网络架构的技术范式分为三类：特征交互、序列编码和图传播。

#### 3.2.1 Feature Interaction

- **写作要点**：
  - 用神经网络替代 MF 内积，建模非线性特征交互。
  - 工业里程碑：YouTube DNN（召回+精排双塔架构）、Wide&Deep（记忆+泛化）。
  - 代表工作：RBM (2007), DeepMusic (2013), CDL (2015), VBPR (2016), YouTube DNN (2016), Wide&Deep (2016), DeepFM (2017), NCF (2017), DIN (2018)。
  - 优势：端到端学习，自动特征交叉。局限：参数规模显著增长，单一路径的特征交互缺乏结构归纳偏置。
  - 与 DC 的关联：深度模型参数规模使传输/更新/部署成本成为 DC 的实际瓶颈。

#### 3.2.2 Sequential Encoding

- **写作要点**：
  - 从"静态偏好"到"动态意图"的范式转换。
  - 核心创新：用自注意力/Transformer 捕捉行为序列中的时序依赖。早期 RNN/CNN 方法（GRU4Rec, Caser）奠定了序列化建模基础，SASRec 和 BERT4Rec 将自注意力引入推荐序列建模。
  - 代表工作：GRU4Rec (2016), Caser (2018), SASRec (2018), BERT4Rec (2019)。
  - 与 DC 的关联：序列编码的实时性刚需天然适合端侧部署（行为序列在本地最完整），但模型复杂度与 Embedding 规模需要云端支持——这构成了 DC 中"端侧序列+云端候选"的自然分工。

#### 3.2.3 Graph Propagation

- **写作要点**：
  - 利用 User-Item 二分图传播协同信号。高阶邻域信息的显式建模。SR-GNN 开创了图神经网络在会话推荐中的应用，LightGCN 通过去除非线性激活证明图推荐的本质在于邻域聚合本身。
  - 代表工作：SR-GNN (2019), LightGCN (2020), SGL (2021), LT-OCF (2021)。
  - 局限：图结构依赖全局数据，天然与端侧隔离矛盾。
  - 与 DC 的关联：Decentralized GNN 工作尝试将全局图分解为 Ego-graph 子图在端侧运行，是 DC 中"图分解"思路的起源。

#### 3.2.4 Summary: What Deep Models Enabled and Constrained

- 深度模型极大提升了表达能力，但也大幅增加了存储/通信/算力负载。
- 行为序列和图结构都需要全局视角，端侧天然受限 → DC 协作的必要性更加明显。

### 3.3 Large Semantic Alignment Models

> 大语义对齐模型标志着推荐系统从"专有模型"走向"基础模型语义赋能"的质变。按对齐方式分为三类：指令对齐、生成对齐和智能体对齐。

#### 3.3.1 Instruction Alignment

- **写作要点**：
  - 将推荐重构为语言/生成任务，利用 LLM 的语义理解与常识推理。
  - 代表工作：P5 (2022), TALLRec (2023), LLMRank (2024), BIGRec (2025)。
  - 优势：零样本/少样本泛化、可解释性、跨域知识迁移。
  - 局限：推理延迟极高（自回归生成）、训练/推理成本巨大。
  - 与 DC 的关联：催生了"云大端小"的 LLM 协同范式——云侧 LLM 做复杂推理，端侧 SLM 做高频快速响应。

#### 3.3.2 Generative Alignment

- **写作要点**：
  - 将推荐建模为从用户表示到物品 ID 的生成过程，利用扩散模型或自回归生成范式。
  - 代表工作：DiffRec (2023), TIGER/GenRec (2023), HSTU (Meta, 2024)。
  - 核心意义：验证了推荐领域也遵循 Scaling Law——万亿参数的生成式推荐彻底改变了"推荐是否需要大模型"的讨论。
  - 与 DC 的关联：生成式模型为端侧"合成数据"和"代理样本"提供了新工具。

#### 3.3.3 Agentic Alignment

- **写作要点**：
  - LLM 作为可规划/可记忆/可工具调用的推荐智能体。
  - 代表工作：RecMind (2024), InteRecAgent (2025), MemRec (2026)。
  - 核心思想：推荐不仅是"排序"，更是"理解→规划→执行"的闭环。
  - 与 DC 的关联：Agentic 范式天然适合端云角色分工——云端 Agent 负责全局规划与知识检索，端侧 Agent 负责实时感知与本地执行。这是 Ch5.5.3 和 Ch7 的重要铺垫。

#### 3.3.4 Summary: Lessons from the Cloud Side

- 云端推荐的能力边界：天然适合"全局理解"与"深度推理"，天然不适合"实时响应"与"隐私敏感"。
- 这种不对称性就是 DCRec 设计的核心依据。

### 3.4 Chapter Summary: What Three Generations of OC Tell Us

- **写作要点**：
  - OC 三十年的演进史呈现清晰的"表达能力递增、模型规模递增、推理延迟递增"的三角张力。每一代技术都在前一代的基础上突破表达能力的上限，但同时也加剧了实时性与隐私的短板。
  - 从 DC 视角反观：OC 积累的三代技术——Shallow 的 Embedding 抽象、Deep 的序列编码与图传播能力、Large Semantic 的零样本泛化——构成了端云协同中"云侧能力"的完整工具箱。Ch5 中的 DC 方法正是在这个工具箱之上设计端云分工策略。
  - 过渡句：OC 的瓶颈催生了 OD 的发展——端侧能否接管部分推荐任务，解决延迟与隐私问题？

---

## Chapter 4. On-Device Recommendation

> **目标**：~2.0 页双栏（约 1600 词 + 1 分类图）。对端侧推荐技术进行系统化重组，按"部署（把模型装进设备）→ 适配（让模型在设备上持续进化）"两阶段组织，适配阶段按优化目标细分为 Privacy / Latency / Resource 三个维度。
>
> **写作原则**：
>
> - §4.1 开头 1-2 句标注三大约束对应的挑战（回钩 Ch2.2.2-2.2.3），Embedding 瓶颈贯穿全章。
> - 每子类 2-3 篇代表方法 + 1 句 DC 关联，不展开方法细节。
> - **边界纪律**：Ch4 只保留纯 OD 或以端为主、无显式云协同的方法。有显式云协同的放 Ch5。
> - §4.3 章末 3-4 句回归 Real-Time Personalization（Ch2.3）：OD 核心局限是缺乏全局协同信号。

| 维度 | Embedding Slimming | Structure Compression | Knowledge Distillation | Privacy Adapt. | Latency Adapt. | Resource Adapt. |
|:---|:---|:---|:---|:---|:---|:---|
| 目标 | 缩减 Embedding 体积 | 量化/分解/跨层压缩 | 保留教师模型质量 | 数据不出设备 | 纯端侧低延迟推理 | 弹性适配硬件 |
| 技术手段 | 混合维度/组合 Hash/无表生成 | 量化/张量分解/跨层 | Ranking/Unbiased/LLM→Small | 纯本地/去中心化/遗忘 | 轻量架构/实时追踪/预取 | 弹性 Embedding/按需通信 |
| 压缩率 | 10-100× | 4-32× | 5-50× (参数) | N/A | N/A | 动态 |
| 精度代价 | 中 | 中-高 | 低-中 | 高（缺协同） | 中 | 中 |
| 与 DC 的边界 | 云托管全量表+按需下发 | 云指导压缩策略 | 云侧教师→端侧学生 | 纯端→需去中心化协同 | 端侧为主；引入云协同 → Ch5 | 云弹性配置 |

![ODRec](figs/ODRec.svg)

### 4.1 General Lightweight Methods: Deployable Inference

> **章节引言**：核心问题——如何将一个已经训练好的推荐模型"瘦身"到可以在端侧设备上以可接受的延迟和功耗运行？答案分三条路线：压缩 Embedding、结构压缩、知识蒸馏。

#### 4.1.1 Embedding Slimming

- **写作要点**：
  - 问题定义：推荐模型的 Embedding 表通常占据 90%+ 的参数量和存储空间，是端侧部署的第一瓶颈。
  - 技术路线：
    1. **混合维度分配**（Learnable Embedding Sizes, SSEDS）：低频特征低维、高频高维，自动化搜索。
    2. **组合式压缩**（Compositional Embeddings / Complementary Partitions）：多个小 Hash 表组合替代一个大 Embedding 表。
    3. **无表替代**（Deep Hash Embedding / No Embedding Table）：轻量网络从特征原始值直接生成 Embedding，完全去除查表。
  - 代表工作：Compositional Embeddings (2020), Learnable Embedding Sizes (2021), No Embedding Table (2021), SSEDS (2022), LEGCF (2024)。
  - 与 DC 的关联：端侧不需要存储全量 Embedding → 云侧托管完整表并按需下发子集。

#### 4.1.2 Structure Compression

- **写作要点**：
  - 问题定义：除了 Embedding，推荐模型的其他部分（特征交互层、排序网络）如何压缩？
  - 技术路线：
    1. **量化**（PQ-VAE）：用离散编码替代浮点 Embedding。
    2. **张量分解**（TT-Rec）：将高维 Embedding 矩阵分解为低秩张量链。
    3. **跨层压缩**（MLCC）：Embedding + 特征交互 + 输出层的协同优化。
  - 代表工作：PQ-VAE (2019), TT-Rec (2021), MLCC (2026)。
  - 与 DC 的关联：压缩后的小模型是端侧推理的"引擎"，但压缩策略可能需要云端的全局统计指导。

#### 4.1.3 Knowledge Distillation

- **写作要点**：
  - 问题定义：如何在模型大幅缩小的前提下，尽可能保留原始大模型的排序/推荐质量？
  - 技术路线：
    1. **Ranking Distillation**：从大模型（教师）的完整排序列表中学习。
    2. **Unbiased Distillation**：解决蒸馏过程中的曝光偏差问题。
    3. **LLM→Small Model Distillation**（EKD4Rec）：从 LLM 蒸馏知识到传统小推荐模型。
  - 代表工作：Ranking Distillation (2018), Unbiased KD (2023), EKD4Rec (2025)。
  - 与 DC 的关联：KD 是 DC 中最自然的"知识传输"机制——云侧教师模型持续产出软标签，端侧学生模型实时消费并适配。

#### 4.1.4 Summary of General Lightweight Methods

- 三条路线不是互斥的，实际端侧部署通常组合使用。核心矛盾始终是：**精度 vs 体积 vs 速度**。
- 这些方法解决的是"模型如何装进设备"——但没有回答"模型如何在设备上持续进化"→ 引出下一节。

### 4.2 Efficient Adaptation Solutions: On-Device Personalization

> **章节引言**：核心问题——部署到设备后，模型如何利用本地数据持续更新以适应个体用户？按优化目标分三条路线：隐私优先、延迟优先、资源优先。这三条路线并非互斥，而是在不同应用场景中各有侧重。

#### 4.2.1 Privacy-Oriented Adaptation

- **写作要点**：
  - 问题定义：数据绝对不能离开设备，如何在纯本地环境下实现个性化更新？
  - 技术路线：
    1. **Local Training**（DeepRec, PocketLens）：纯本地微调，数据绝对安全但缺乏协同信号。
    2. **Decentralized Collaborative Learning**（MAC, DCL4Rec）：设备间 Gossip 协议交换参数，无中心服务器。
    3. **Federated Unlearning**：用户退出/被遗忘权场景——从全局模型中删除特定用户的贡献。
  - 代表工作：PocketLens (2004), DeepRec (2021), Federated Unlearning (2023), MAC (2023), DCL4Rec (2024)。
  - 与 DC 的关联：纯端侧隐私方案走到极致 = 孤岛模型，需要某种形式的"去中心化协同"来引入外部信号。这是 Ch5.4 中 Federated Rec 的思想基础。

#### 4.2.2 Latency-Oriented Adaptation

- **写作要点**：
  - 问题定义：如何在保证推理低延迟（<10ms）和更新低延迟（分钟级）的前提下，在纯端侧环境中实现个性化？
  - 端侧低延迟适配的核心挑战：推理延迟与模型容量的权衡——更轻量的模型推理更快但表达能力更弱。纯 OD 场景下没有云端辅助推理的选项，所有计算必须在端侧完成。
  - 技术路线：
    1. **端侧轻量推理架构**：通过模型结构优化（深度可分离卷积、低秩分解、提前退出）在不牺牲太多精度的前提下压缩推理延迟。
    2. **端侧实时偏好追踪**（Real-time Short Video Rec）：针对短视频特殊场景，利用端侧实时行为信号在秒级窗口内更新用户表示。
    3. **端侧缓存与预取策略**：在端侧缓存高频候选物品的 Embedding，减少实时特征计算开销。
  - 代表工作：Real-time Short Video Rec (2022), OD-Rec (2022, 同时属于资源导向)。
  - **与 DC 的衔接**：纯端侧低延迟方案的优势在于零网络依赖，但受限于算力（复杂模型无法部署）和候选空间（缓存有限）。当端侧推理能力不足以满足当前请求质量时，就自然需要云侧参与——这类"端侧为主、云侧按需辅助"的方法（如 EdgeRec 的端侧重排+云召回、MPDA 的云协调域自适应）已进入 DC 范式，正文主分析在 Ch5.5。

#### 4.2.3 Resource-Oriented Adaptation

- **写作要点**：
  - 问题定义：不同设备的算力、内存、电量、网络千差万别，模型如何弹性适配？
  - 技术路线：
    1. **弹性 Embedding**（Learning Elastic Embeddings, PEEL）：根据可用资源动态调整 Embedding 维度。
    2. **按需更新与自适应剪枝**（Shaver）：在端侧运行时根据实际负载和模型置信度动态调整参数规模与更新频率。纯端侧的自适应剪枝不依赖云端信号，通过合作博弈等方法在本地决定参数重要性。（注：涉及云端协同更新的方法如 IntellectReq、ODUpdate 的主体分析在 Ch5.5。）
    3. **轻量架构**（SNN for Rec, On-Device LLM for Rec）：脉冲神经网络等极低功耗架构，甚至将微型 LLM 搬上端侧。
  - 代表工作：Elastic Embeddings (2021), OD-Rec (2022), EODRec (2023), PEEL (2024), Shaver (2025), SNN for Rec (2025), On-Device LLM for Rec (2026)。
  - 与 DC 的关联：资源异质性是 DC 场景的真实约束，Ch5.5 中的动态部署机制直接回应这一问题。

#### 4.2.4 Summary of Efficient Adaptation Solutions

- 端侧个性化从"纯隐私"到"纯延迟"是一个连续谱，实际方案通常在三点之间折中。
- 资源约束使得"一刀切"的端侧方案不可行 → 云端需要提供可配置的个性化机制 → DCRec 的动机在此达到顶峰。

### 4.3 Chapter Summary: Why On-Device Alone is Not Enough

- 端侧的优势和劣势恰好与云端互补：

  | 维度 | 云端优势 / 端侧劣势 | 端侧优势 / 云端劣势 |
  |:---|:---|:---|
  | 协同信号 | 全局 CF / 弱 | 强 / 信息孤岛 |
  | 实时性 | 弱 / 日志回流延迟 | 强 / 零延迟 |
  | 隐私 | 弱 / 集中式存储 | 强 / 数据不出设备 |
  | 算力 | 强 / 无限制 | 弱 / 严格受限 |
  | 候选集 | 大 / 全量物品 | 小 / 受限 |

- 这一互补关系是 DCRec 存在的根本逻辑。OC 章展示了云侧"能提供什么"，OD 章展示了端侧"能做什么"以及"缺什么"，二者的交集就是 DCRec 的设计空间。下一章将正式定义并系统化这个新兴范式。

---

## Chapter 5. Device-Cloud Collaborative Recommendation

> **目标**：~3.5 页双栏（约 2800 词 + 2 图/表）。DC 部分的主体，不再拆分为多章。包含 DCRec 的范式定义、三机制分类法，以及三大机制的综述。这是全文篇幅最长的章节，承载了 DCRec 的"定义、划界、分类、展望"四大功能。也是本文**最强原创性**的所在——首次将 DCRec 作为独立范式进行形式化定义，并提出互斥的三机制主分类。TKDE 版大幅精简：5.1-5.2 约 0.7 页，三大机制各约 0.8-1.0 页。
>
> **写作原则**：
>
> - 从"范式定义"开始（直接引用 Ch2.1 的联合优化框架，标注 DCRec 的特化形式），让不熟悉 DC 的读者在进入方法综述前先建立清晰的边界认知。
> - 分类框架（5.2）是三机制综述的导航图。此处的 `Mechanism × Challenge` 矩阵（5.2.5）需显式引用 Ch2.2.4 的 DC 列，说明三个主分类各应对哪些挑战。
> - 三大机制（5.3-5.5）每节开头标注该机制的 Collaborative Personalization 实例化形式（引用 Ch2.3）和主要应对的挑战（引用 Ch2.2.4）。
> - 三大机制各约 0.8-1.0 页双栏，以代表性方法与关键设计思想为主，不再逐篇展开。TKDE 版每机制精选 5-8 篇代表性工作，完整列表见 README。
> - **边界纪律**：跨维度方法（如 EdgeRec、Meta Controller、MPDA）以 Ch5 为主归属章节，不在 Ch4 中重复展开。在 Ch5 内部的跨机制引用通过辅助标签（如 `[Computation: Early Exit]`）处理，不在多个节重复作为代表性方法分析。
> - §5.6 章末回归 Ch2.2.4 矩阵：DC 是唯一在三个挑战维度都面临系统级约束的范式——这正是它需要 Computation + Model + Task 三维机制的根本原因。

### 5.1 Paradigm Definition and Formalization

- **写作要点**：
  - 在 Introduction 给出非正式定义的基础上（1.5），本节提供完整的形式化定义。
  - **正式定义**：DCRec 是一个联合优化问题，端侧模型 $\{\theta_u\}$ 与云侧模型 $\Phi$ 共同参与推荐流水线，通过显式协同协议交换信息（候选集 $\mathcal{C}_u$、软标签 $\mathbf{y}_u^{\text{soft}}$、表示 $\mathbf{h}_u$、参数 $\theta_u$、梯度 $\nabla\theta_u$、策略 $\pi_u$ 等），在通信预算 $B$、延迟预算 $T$、隐私预算 $\epsilon$ 的约束下最小化联合损失。
  - **纳入标准**（与 1.5 一致，此处可精简）：存在云-端双向信息流动；端侧承担实质性推荐角色（训练/推理/决策）；协同机制有可量化贡献。
  - **与 OC 和 OD 的边界**：
    - 若 $\mathcal{D}^{\text{cloud}} = \emptyset$ 且 $\Phi$ 不存在（所有计算在端侧）→ 退化为 OD。
    - 若 $\{\theta_u\}$ 不存在且所有计算在云端 → 退化为 OC。
    - 若 $\mathcal{D}^{\text{cloud}} = \emptyset$ 但 $\Phi$ 通过端侧梯度聚合生成 → FedRec（DCRec 在强隐私约束下的特例）。
  - **安全与隐私的跨切面定位**：重申隐私不是主分类维度而是跨切面约束（与 Ch2.1 一致）。在 5.4 中讨论 DP / SecAgg 等隐私机制作为 FedRec 的具体技术手段。

### 5.2 Taxonomy of Collaborative Mechanisms

> **设计原则**：DCRec 方法的**本质差异不在于用了什么技术，而在于协作机制主要优化什么目标**。因此我们按每个方法的 **primary optimization objective** 将 DCRec 方法分为三个互斥的主分类。
>
> **与 Ch2 挑战的映射**：三个主分类分别从不同层面回应 Ch2.2 中定义的三大系统级挑战——Computation Allocation 从基础设施层解决异构算力的分配问题，Model Sharing 从知识层解决数据异质性下的协同学习问题，Task Orchestration 从应用层解决海量设备下的任务调度问题。三种机制各对应 Ch2.3 的 Collaborative Personalization 的不同侧重维度。

#### 5.2.1 Computation Allocation Mechanisms (5.3)

- **核心优化目标**：训练/推理的计算负载在端云之间的**格局**（topology）与**节奏**（synchronization）。核心问题是"谁来维护推荐知识的全局一致性，以及计算任务如何据此分配"。
- **两个子类**（按全局知识的治理方式区分）：
  1. **Coordinated Parallelism**（5.3.1）：由一个显式的中心协调者（Parameter Server / Master / Pipeline Scheduler）维护统一的全局模型状态；Workers 执行数据并行、模型并行或流水线并行子任务并向协调者汇报。核心挑战在于同步约束下的通信-效率权衡。
  2. **Decentralized Partitioning**（5.3.2）：无中心权威节点，推荐知识通过 Peer-to-Peer 交互（Gossip、共识算法、区块链协调）在节点间扩散涌现。每个节点维护本地模型，全局知识从局部交换中涌现。核心挑战在于数据异质性和部分信息条件下的收敛。
- **归属判定**：如果论文的主要贡献是设计新的分布式拓扑/同步协议、Pipeline 调度策略或 Peer-to-Peer 协同收敛协议，则归入此类。

#### 5.2.2 Model Sharing Mechanisms (5.4)

- **核心优化目标**：模型参数/知识在端云之间的**流动**（flow）与**聚合**（aggregation）。
- **关键技术**：联邦聚合算法、个性化联邦策略、跨域/跨模态知识迁移。
- **归属判定**：如果论文的主要贡献是设计新的联邦聚合算法、个性化联邦策略或跨域知识迁移机制，则归入此类。前提是**端侧运行推荐模型**且存在显式的端云参数同步。

#### 5.2.3 Task Orchestration Mechanisms (5.5)

- **核心优化目标**：推荐流水线中端云之间的**任务级分工**——云端构建/指导/协同端侧执行推荐子任务。核心问题是"云和端在推荐流水线中各自承担什么角色，以及何时、以何种方式交互"。
- **三个子类**（按云侧介入时机与方式区分）：
  1. **Module Scheduling**（5.5.1）：云端**离线**为端侧构建定制化模型——生成个性化参数或架构规格，端侧使用云构建的模型独立推理，部署后不再依赖云端实时信号。
  2. **Knowledge Routing**（5.5.2）：云端在端侧模型**部署期间**持续提供监督信号（软标签、域校正、语义校准），引导端侧模型适应本地数据。云端不替代端侧推理，只提供知识层面的纠正信号。
  3. **Inference Dispatching**（5.5.3）：云端和端侧在**推理时刻**联合分解并执行推荐流水线中的互补子任务——云端承担重语义推理/候选检索，端侧执行响应式个性化重排。
- **归属判定**：如果论文的主要贡献是设计端云任务级分工协议、动态触发机制、或协同推理的交互接口，则归入此类。

#### 5.2.4 Multi-Dimensional Methods and Placement Rules

> 部分方法天然跨多个维度。我们按以下规则做仲裁：

- **仲裁规则 1（核心创新优先）**：以核心创新点所在的维度为准。
  - Meta Controller 的核心创新在于"云端持续监控端侧质量并触发知识纠正"→ **归入 Task: Knowledge Routing**（5.5.2），而非 Computation。
  - EdgeRec 的核心创新在于"端侧实时重排的完整流水线设计"→ **归入 Task: Inference Dispatching**（5.5.3），而非 Computation。
  - DUET 的核心创新在于"云端离线生成端侧模型参数"→ **归入 Task: Module Scheduling**（5.5.1），而非 Model Sharing。
- **仲裁规则 2（同类边界案例统一裁定）**：
  - EdgeRec / MC2-SF 的 Slow-Fast 架构模式：其分布式计算拓扑的讨论在 Comp: Coordinated Parallelism（5.3.1）中交叉引用，但完整的流水线分析在 Task (5.5)。
  - LLM-SLM 协同（LSC4Rec, CDA4Rec）：调度协议归入 Task: Inference Dispatching（5.5.3），LLM 的知识融合方面在 Model (5.4.3) 中交叉引用。
  - Federated + Task 交叉方法（如 FedRec 框架下的个性化部署策略）：主体按核心贡献归属 Model Sharing (5.4)，部署层面在 Task (5.5) 交叉引用。
- **辅助标签系统**：每个跨维度方法标注 `[Computation: Coordinated]` / `[Task: Module Sched]` 等标签，方便研究者按交叉维度检索。

#### 5.2.5 Mechanism × Challenge Cross-Matrix

- 与 Ch2.2.4 不同，此处的矩阵聚焦于 DCRec 内部的挑战分配：

  |                         | Computation Allocation (5.3) | Model Sharing (5.4) | Task Orchestration (5.5) |
  |:------------------------|:----------------------------:|:-------------------:|:------------------------:|
  | Data Heterogeneity      | ● (Non-IID → Straggler / 异步策略) | ● (Client Drift → 个性化聚合) | ○ (同一用户跨场景调度) |
  | Embedding-Centric       | ● (Embedding 表放置与访问) | ● (Embedding 安全同步) | ○ (候选 Embedding 下发时机) |
  | Massively Distributed   | ● (参与率/设备可用性) | ● (部分参与/通信失败) | ● (设备粒度的任务分配) |

- 此表说明 DC 需要三维联动才能完整应对所有挑战——没有哪个单一维度能覆盖全部三列全 ●。

### 5.3 Computation Allocation Mechanisms

> **目标**：~1.0 页双栏。DC 基础设施层——"谁来计算"。分为两个子类：Coordinated Parallelism（中心协调）和 Decentralized Partitioning（去中心化分区）。
>
> **与 Ch2 的衔接**：本机制主要应对 Ch2.2.4 交叉矩阵 DC 列中的计算负载分配与 Embedding 表放置问题。Coordinated 和 Emergent 分别从不同角度回应 Massively Distributed 挑战。
>
> **Collaborative Personalization 实例化**：在 Coordinated Parallelism 中，协同个性化体现为云侧中心节点维护 Contextual 全局模型状态 + Workers 贡献局部计算并消费全局知识；在 Decentralized Partitioning 中，协同个性化体现为局部 Peer 交换（Real-Time 本地交互）+ 渐进全局一致（涌现式 Contextual 知识）。

#### 5.3.1 Coordinated Parallelism

> 中心协调并行方法在一个显式的中心协调者（Parameter Server / Master / Pipeline Scheduler）的管理下，将推荐计算并行化以处理单机无法承载的超大规模模型和数据。协调者显式维护全局模型状态；Workers 执行数据并行、模型并行或流水线并行子任务并向协调者汇报。核心挑战在于同步约束下的通信-效率权衡。

- **写作要点**：
  - **Parameter Server 架构**（OpenEmbedding, PGLucid, Herald）：推荐场景的特殊性——Embedding 表通常放 Server 侧（因其巨大），Dense 参数可在 Worker 侧。这是推荐系统参数服务器的典型"混合放置"策略。
  - **Pipeline 并行与流水线调度**（NestPipe, Ekko）：面向超大规模训练（1500+ 加速器），通过嵌套流水线实现低延迟模型更新。Ekko 的 Bounded Staleness 策略在同步效率与模型新鲜度之间折中。
  - **通信效率优化**：梯度压缩（稀疏化/量化）、低秩分解（CoLR-FedRec）、等价替代训练（DEST）。每个优化都伴随精度损失，需统一框架评估。
  - **工业级系统**：TiVo (2004) 作为最早的去中心化推荐工业系统的雏形；分布式 GPU PS（PGLucid）支撑十亿级广告推荐；FairSync 解决分布式检索中的群体曝光公平性问题。
  - 关键表格：

    | 方法 | 并行模式 | 同步策略 | 通信负载 | 代表工作 |
    |:---|:---|:---|:---|:---|
    | Parameter Server | 数据并行 | 同步/BSP | 高（全梯度） | OpenEmbedding, PGLucid |
    | Pipeline Parallel | 模型/流水线并行 | Bounded Async | 中（分段传输） | NestPipe, Ekko |
    | Comm-Efficient | 数据并行 | 混合 | 低（压缩/量化） | CoLR-FedRec, DEST |

#### 5.3.2 Decentralized Partitioning

> 去中心化分区方法无需中心权威节点，推荐知识通过 Peer-to-Peer 交互在节点间扩散涌现。数据保留在源节点；每个 Peer 维护本地模型，全局推荐知识通过 Gossip 协议、共识算法或区块链协调从局部交换中涌现。核心挑战在于数据异质性和部分信息条件下的收敛。

- **写作要点**：
  - **Gossip-Based 推荐**（PEPPER, GDSRec, Decentralized POI Rec）：节点随机选择邻居交换模型参数，通过流行病传播达到渐进一致性。天然适应设备不可靠的移动边缘网络，无单点故障风险。
  - **去中心化 MF 与图推荐**（PDMFRec, Decentralized GNN）：矩阵分解和图神经网络在无中心服务器条件下的去中心化实现。每个节点仅持有局部子图/局部评分，通过邻居通信补全全局结构。
  - **区块链与 Web3 推荐**（D-RecSys）：利用区块链智能合约作为去中心化协调层，在无信任环境中实现推荐知识的可审计共享。
  - 关键表格：

    | 方法 | 协同协议 | 知识扩散方式 | 代表工作 |
    |:---|:---|:---|:---|
    | Gossip Learning | 随机邻居交换 | 模型参数流行病传播 | PEPPER, GDSRec |
    | Decentralized MF | 邻居梯度聚合 | 局部评分矩阵补全 | PDMFRec |
    | Graph-Based | 子图扩散 | Ego-graph 邻域扩展 | Decentralized GNN, DecKG |
    | Blockchain-Based | 共识/智能合约 | 链上知识审计 | D-RecSys |

  > **注**：DCL4Rec / MAC 等纯端侧去中心化方法的主体分类为 OD 隐私适配（Ch4.2.1），此处仅从计算拓扑角度交叉引用。两者边界：Decentralized Partitioning 的方法仍属于 DCRec（存在显式端云或端端知识交换协议），纯 OD 的去中心化工作则以隐私保护为首要目标。

### 5.4 Model Sharing Mechanisms

> **目标**：~1.0 页双栏。DC 知识层——"知识如何流动与聚合"。联邦推荐（Federated Recommendation）是主要技术载体。按技术演进组织：Foundational Fusion → Personalized Aggregation → Advanced Integration。
>
> **与 Ch2 的衔接**：本机制主要应对 Ch2.2.4 交叉矩阵 DC 列中的 Client Drift / 数据异质性问题、Embedding 安全同步问题，以及部分参与/通信失败问题。
>
> **Collaborative Personalization 实例化**：在 Model Sharing 中，协同个性化表现为全局聚合（Contextual 知识汇聚）与本地适应（Real-Time 微调）的迭代——FedAvg 聚合后分发全局知识 → 端侧基于本地数据个性化 → 再聚合。个性化强度的调节通过端侧自适应深度 $d_{\text{local}}$ 与聚合频率 $f_{\text{aggregate}}$ 实现。

#### 5.4.1 Foundational Fusion

- **写作要点**（精简至核心脉络）：
  - **Federated MF**（FedMF, MetaMF, LightFR, FCF, FedRec）：最早、最基本的 FedRec 方案。客户端持有 User Embedding，服务端聚合 Item Embedding 梯度。隐私增强：DP 噪声 + 安全聚合（FR-FMSS 等）。
  - **Federated Deep Models**（FedNCF, FedGNN, FedFast）：扩展到深度模型和图模型。图联邦的特殊性——缺失全局图结构需要匿名子图扩展。
  - **隐私与安全攻防**：DP + SecAgg 基础机制；投毒攻击（PipAttack, PoisonFedDLRS）与拜占庭鲁棒性（NotOneLess, PrivFR）；模型窃取与无参数传输方案（PTF-FedRec）；联邦遗忘（FedAU2）。
  - 核心局限：个性化不足（用户被均质化）→ 推动下一阶段。

#### 5.4.2 Personalized Aggregation

- **写作要点**（以分类为主，精简代表工作讨论）：
  - 个性化策略分类：
    1. **加性个性化**（FedRAP）：$\theta_u = \Phi_{\text{global}} + \Delta_u$
    2. **聚类个性化**（CoFedRec, GPFedRec）：簇内共享/簇间不同
    3. **层次个性化**（Hierarchical PFL）：用户-组-全局三层
    4. **图引导个性化**（GPFedRec, LPSFed）：局部图指导聚合权重
    5. **双个性化**（PFedRec）：User + Item 双端个性化
    6. **异构模型聚合**（HeteFedRec, FedEM, FedCA）：允许不同端侧运行不同架构
  - **冷启动处理**：属性分离（IFedRec）、知识融合（WWW 2025）、联邦持续学习（F3CRec）——冷启动用户最需要云端全局先验，体现 DC 中"云端冷启动 + 端侧热适配"的自然分工。

#### 5.4.3 Advanced Integration: Cross-Domain, Multi-Modal, and Foundation Models

- **写作要点**（高度精简）：
  - **跨域联邦**（FedCDR, Win-Win, PFCR）：隐私约束下利用互补的跨域用户行为。
  - **多模态联邦**（FedVLR, GFMFR, FFMSR）：图像/文本特征作为"端云共享公共空间"，绕过部分隐私限制。
  - **基础模型集成**：联邦 Adapter/Prompt 学习（FedPA, TransFR, MRFF）——冻结云端基础模型，端侧仅微调 Adapter，云端聚合轻量模块。这是 LLM 时代 FedRec 的关键范式转变：从"同步全参数"到"同步轻量适配器"。
  - **LLM 作为外部服务**（FELLAS）：LLM 提供语义增强，FedRec 框架负责个性化聚合。

### 5.5 Task Orchestration Mechanisms

> **目标**：~1.0 页双栏。DC 应用层——"云和端在推荐流水线中各自承担什么角色"。根据**云侧介入时机与方式**分为三个子类：Module Scheduling（离线构建）、Knowledge Routing（在线引导）、Inference Dispatching（推理协同）。TKDE 版仅以 2-3 篇代表性方法 per 子类展开。
>
> **与 Ch2 的衔接**：本机制主要应对 Ch2.2.4 交叉矩阵 DC 列中的 Massively Distributed Devices（设备粒度的异构任务分配），同时涉及跨场景调度的数据异质性和 Embedding 下发时机。
>
> **Collaborative Personalization 实例化**：Module Scheduling 中云侧提供 Contextual 的知识（离线构建个性化模型），端侧消费此知识进行 Real-Time 独立推理；Knowledge Routing 中云侧持续产出 Contextual 监督信号，端侧以此信号实时纠正本地个性化；Inference Dispatching 中云侧承担 Contextual 重语义推理，端侧执行 Real-Time 响应式重排。

#### 5.5.1 Module Scheduling

> 模块调度方法的核心特征是**云端离线为端侧构建定制化模块**——生成个性化参数或架构规格后，端侧使用云构建的模块独立推理，部署后不再依赖云端实时信号。核心挑战在于生成模块的个性化精度与通用性的平衡，以及端侧模块与云端模块的结构兼容性。

- **写作要点**：
  - **参数生成**（DUET, WWW 2023）：云端直接为端侧生成完整或部分模型参数，端侧无需本地训练即获得个性化能力。核心创新在"免调参"——端侧不需要任何梯度计算。
  - **架构生成与超网适配**（DIET, KDD 2024; Forward-OFA, KDD 2025; CHORD, MM 2025）：云端维护一个覆盖多种设备能力的超网（Supernet），端侧设备通过结构参数从中提取适合自身算力/存储的子网。解决"不同设备运行不同模型架构"的异构部署问题。
  - **协同学习框架**（DCCL, KDD 2021）：最早的端云协同学习框架，提出"千人千模"概念——云端吸收海量端侧个性化模型知识后生成更优的全局模型。
  - **跨域模型生成**（DeCoCDR, SIGIR 2024; DualRec, IEEE TMC 2025）：将模型生成扩展至跨域和双模型场景。
  - 设计核心：云侧生成的模型参数可以是全量的、适配器模块的、或仅指定架构的——粒度越细个性化越强但通信成本越高。

#### 5.5.2 Knowledge Routing

> 知识路由方法的核心特征是**云端在端侧模块部署期间持续提供监督信号**——软标签、域校正、语义校准等——路由到端侧模块以适应本地数据。与 Module Scheduling 不同，云端不直接生成端侧模块参数，而是以"教师/裁判"角色提供知识层面的纠正信号。核心挑战在于路由信号的时效性（何时介入）与精度（纠正了什么）。

- **写作要点**：
  - **云协调域自适应**（MPDA, KDD 2022）：云端提供域适应策略，端侧在策略指导下进行本地个性化，平衡全局一致性与本地个性化。
  - **元控制与智能触发**（Meta Controller, KDD 2022; IntellectReq, WWW 2024）：云端部署控制器持续监控端侧推荐质量，仅在检测到候选不足或模型"不自信"时触发知识纠正，避免不必要的云端通信。
  - **协同纠错**（CoCorrRec, IJCAI 2025）：云端识别并纠正端侧模型的系统性偏差，提供纠正信号而非替代端侧决策。
  - **无反向传播引导**（CDC-MMPG, TOMM 2025; RelayRec, IPSN 2024）：云端通过正向传播或中继机制传递知识，端侧完全不需要反向传播，大幅降低端侧训练开销。
  - **语义校准**（SCaLRec, Arxiv 2026）：在 LLM 时代，云端 LLM 的通用知识与端侧推荐模型的协同过滤表示之间存在语义鸿沟，SCaLRec 提出语义校准框架解决此问题。
  - 设计核心：引导信号的设计——可以是软标签（Teacher-Student）、域校正信号、或自然语言语义指导。触发策略——持续引导 vs 按需引导，权衡通信成本与推荐质量。

#### 5.5.3 Inference Dispatching

> 推理分发方法的核心特征是**云和端在推理时刻联合分解并执行推荐流水线中的互补子任务**。云端承担重语义推理和候选检索（大模型、全量候选库），端侧执行响应式个性化重排（实时上下文、隐私敏感特征）。核心挑战在于延迟预算内的端云任务切分与信息交换协议。

- **写作要点**：
  - **云召回 + 端重排**（工业标准模式）：
    - EdgeRec (CIKM 2020)：淘宝移动端重排系统，展示工业级方案的延迟（<10ms）、功耗与效果统计。
    - Real-time Short Video Rec (CIKM 2022)：短视频场景的端侧实时偏好追踪与云端候选补充。
    - On-Device Integrated Re-ranking (DIR, KDD 2023)：端侧重排中的异质行为建模（点击、购买、停留等多信号融合）。
    - 核心设计：候选集大小 K 的选择、云端下发信息粒度（ID/Embedding/完整特征）、端侧重排延迟预算。
  - **LLM-SLM 协同推理**（LSC4Rec, KDD 2025）：端侧 SLM 先尝试处理，不确定时调用云端 LLM。核心创新在协作触发策略（何时调用 LLM）、提示构造（端侧状态如何转化为 LLM 可理解的输入）、结果融合（LLM 输出如何与端侧模型输出整合）。
  - **扩散式协同推理**（DCPR, KDD 2024）：利用扩散模型在云端生成候选，端侧进行采样和个性化筛选。
  - **Agentic 协同**（CDA4Rec, Arxiv 2025）：云侧 Global Orchestrator 与端侧 Personal Assistant 通过标准化协议交互——这是 Inference Dispatching 的最高级形态，当前仍处早期探索阶段。

#### 5.5.4 Production Systems and Industrial Practice

- **写作要点**（精简，贯穿三个子类）：
  - 淘宝技术演进线：EdgeRec (Inference Dispatching) → Meta Controller (Knowledge Routing) → IntellectReq (Knowledge Routing) → LSC4Rec (Inference Dispatching)。展示了 Task Orchestration 从端侧重排到 LLM 协同的完整进化路径。
  - 快手/抖音：端侧重排 + 实时更新（Inference Dispatching）。
  - 关键经验：工业系统追求"在给定延迟/成本约束下最大化推荐质量"；DC 在工业界的采纳是渐进式的——从 Module Scheduling（离线构建→部署），到 Knowledge Routing（部署后云端持续引导），再到 Inference Dispatching（推理时实时协同）。

### 5.6 Chapter Summary

- **写作要点**：
  - DCRec 的三维机制（Computation → Model → Task）及其子分类构成从基础设施到应用的完整设计栈：
    - **Computation Allocation**：Coordinated（中心协调）和 Decentralized（去中心化分区）分别代表两种截然不同的全局知识治理哲学——前者追求效率与一致性，后者追求去中心鲁棒性与隐私。
    - **Model Sharing**：从 Foundational Fusion（基础联邦融合）到 Personalized Aggregation（个性化聚合）再到 Advanced Integration（跨域/多模态/FM 集成），展现了联邦推荐从"可行性"到"高质量"的完整演进路径。
    - **Task Orchestration**：Module Scheduling（离线构建）→ Knowledge Routing（在线引导）→ Inference Dispatching（推理协同）构成云侧介入程度递进的三个层次——从"离线调度"到"持续路由"再到"实时分发"。
  - 三者不是替代关系而是递进依赖：好的 Task Orchestration 需要高效的计算分配（Comp）和合理的模型共享策略（Model）作为基础设施。
  - Ch3（OC）和 Ch4（OD）分别为 DCRec 提供了"云侧能力工具箱"和"端侧部署约束集"，DCRec 的本质任务是在这两个集合之间设计协同协议。
  - LLM 时代正在重塑所有三个机制：Computation 从"参数同步"到"推理卸载"（投机解码），Model 从"全参同步"到"Adapter 聚合"，Task 从"端云流水线"到"Agent 对话"。
  - 过渡句：以上方法论综述依赖于统一的评估体系来比较与推进——下一章综述实验生态。

---

## Chapter 6. Evaluation, Platforms, and Datasets

> **目标**：~1.0 页双栏（约 800 词 + 1 数据集/平台表）。综述 DCRec 研究的实验生态，按 systematic survey 标准为必选章。三段式：指标讨论 + 数据集表格 + Benchmark Gap。

| 数据集/平台 | 类型 | 规模 | 时间戳 | DC 适配方案 | 不足 |
|:---|:---|:---|:---|:---|:---|
| MovieLens-25M | 评分 | 162K users, 62K items | 有 | 按 User ID 分割 | 规模小，无设备异构 |
| Amazon Reviews | 评论 | 百万级 | 有 | 按 User ID 分割 | 缺乏会话结构 |
| Yelp | POI+评论 | 百万级 | 有+地理 | 按 User ID + 时间分割 | 单域 |
| Taobao/Alimama | 工业 | 亿级 | 有 | 已有 DC 实验设置 | 获取门槛高 |
| Flower | FL 仿真平台 | 可配置 | N/A | 原生支持 FedRec | 缺推荐专用 DSL |
| FedScale | FL 基准 | 可配置 | N/A | 含推荐任务 | 缺端侧推理模拟 |

### 6.1 Evaluation Metrics

- **写作要点**：
  - 传统推荐指标：Precision@K, Recall@K, NDCG@K, MAP, MRR, AUC。
  - DC 特有指标：
    - **通信效率**：通信轮数、每轮传输量（KB/MB）、总通信时间。
    - **设备资源**：端侧推理延迟（ms）、模型存储（MB）、峰值内存、电量消耗。
    - **隐私度量**：差分隐私预算 $\epsilon$、成员/属性推理攻击成功率。
    - **系统吞吐**：最大并发端侧连接数、云端 QPS。
  - 建议：每个 DC 方法至少应报告排序质量 + 通信开销 + 端侧资源消耗 + 隐私保障四维指标。

### 6.2 Simulation Platforms and Frameworks

- **写作要点**：
  - 通用 FL 平台：Flower, FedScale, TensorFlow Federated。
  - 推荐仿真需求：用户行为序列生成器、物品冷启动模拟器、Embedding 存储模拟器。
  - 仿真与真实部署的差异：用户行为逼真度、网络条件简化、设备异质性建模不足。

### 6.3 Datasets

- **写作要点**：
  - 常用公开数据集与 DC 适配方法：
    - MovieLens：经典但用户数少，用于 Proof-of-concept。
    - Amazon Reviews：按用户 ID 分割模拟端侧本地数据。
    - Yelp：带地理和时间戳，适合 POI 推荐 DC 仿真。
    - Steam / LastFM：适合序列推荐 DC 仿真。
  - 数据分割策略：按 User ID 分割（每个用户 = 一个设备）、按时序分割（旧数据云端、新数据端侧）。
  - 核心不足：缺乏真实的设备异构信息（型号、网络、电量等）。

### 6.4 The Benchmark Gap: A Community Infrastructure Agenda

- **写作要点**：
  - 当前 DCRec 研究各自采用不同数据集、模拟器、指标，实验可复现性和公平比较严重不足。
  - **注意定位**：这不是 Ch7 中那些已有文献簇支撑的 "Emerging Trends"，而是由社区碎片化状态倒推出来的基础设施需求。
  - **方向**：
    - 标准化数据集分割方案（User-split + Time-split 复合策略）。
    - 统一仿真环境配置（网络带宽/延迟/丢包率、设备算力/内存档位）。
    - 多维度评估体系（推荐精度 + 通信成本 + 端侧资源 + 隐私保护联合报告）。

---

## Chapter 7. Challenges and Future Directions

> **目标**：~1.5 页双栏（约 1200 词）。分两层结构：Part A 有文献簇支撑的新兴趋势（≥3 篇代表性工作），Part B 早期阶段的范式变革构想。避免平铺所有方向。
>
> **写作原则**：
>
> - Part A 每个方向必须有 ≥3 篇已发表的代表性工作作为证据支撑。
> - Part B 每个方向必须标注当前文献状态（"已有初步探索" / "推荐场景尚属空白" / "纯技术构想"），并与 Part A 形成阶梯。

### 7.1 Emerging Trends with Established Literature

> 本节收录已有稳定文献簇支撑的方向，每条趋势均有明确的研究社群和可追溯的工作线索。

#### 7.1.1 Parameter-Efficient Federated Fine-Tuning for Recommendation

- **文献支撑**：FedPA (IJCAI 2024)、TransFR (AAAI 2026)、MRFF (AAAI 2025)、GFMFR (WWW 2026)——已形成 "FM + FedRec + PEFT" 的稳定研究线。
- **动机**：端侧无法对数十亿参数的基础模型进行全参微调。PEFT（LoRA/Adapter/Prompt Tuning）只更新 <1% 的参数，天然适合端侧联邦学习。
- **当前进展**：初步探索已证明可行性，但尚未系统化。
- **挑战**：LoRA rank 与通信效率/表达能力的最优权衡；非欧空间中 LoRA 模块的有效聚合；端侧推理时 Adapter 与基础模型的融合延迟。

#### 7.1.2 Cross-Domain and Multi-Modal Federated Recommendation

- **文献支撑**：FedCDR (CIKM 2022)、Win-Win (AAAI 2023)、PFCR (WWW 2024)、FedVLR (AAAI 2026)、GFMFR (WWW 2026)、FFMSR (TOIS 2025)——社区正从单域 FedRec 向跨域/多模态方向系统化扩展。
- **方向**：
  1. 跨域联邦如何在隐私约束下利用互补的跨域用户行为？
  2. 多模态特征（图像/文本/视频）如何作为"端云共享公共空间"来降低隐私敏感度？
  3. 如何将 VLM/LLM 的基础语义能力注入联邦框架而不破坏数据本地化约束？

### 7.2 Speculative Outlook: Paradigm Shifts in Early Stages

> 本节收录目前处于"概念验证"或"技术构想"阶段的方向。每个方向的文献证据尚不足以形成稳定研究线，但代表了 LLM 时代可能重塑 DCRec 基础范式的变革潜力。

#### 7.2.1 Speculative Decoding for On-Device LLM Inference

- **文献状态**：投机解码在通用文本生成中已有成功实践（Leviathan et al. 2023），在推荐场景中**尚无公开发表的工作**。
- **技术构想**：端侧部署微型 LLM（0.1-0.5B）作为 Draft Model 快速生成候选物品序列；云端 LLM（8-70B）进行批处理验证。
- **推荐场景的独特优势**：物品空间是有限词汇表，Draft Model 可专门优化 Item 令牌预测。
- **挑战**：Draft-Target 接受率在推荐场景的特性、KV Cache 的端云共享与卸载、串行验证的延迟上限。

#### 7.2.2 Agentic Collaboration: Multi-Agent Cloud-Device Recommendation

- **文献状态**：已有初步探索（CDA4Rec 2025, MemRec 2026），但距成熟的端云 Agentic 框架仍有显著差距。目前属于"概念验证"阶段。
- **技术构想**：云侧 Global Orchestrator Agent + 端侧 Personal Assistant Agent，通过自然语言/结构化 Prompt 协商推荐，而非传输梯度/参数。
- **长期优势**：模型架构完全解耦、天然可解释、隐私更优。
- **近期挑战**：Agent 间通信协议标准化、意图理解可靠性、多轮对话延迟可控性、Agent 幻觉抑制。

#### 7.2.3 Synchronized On-Device RAG

- **文献状态**：尚无公开发表的端云分布式 RAG 推荐工作。当前属于技术构想，受通用端云 RAG 讨论启发。
- **技术构想**：云端维护全量物品向量数据库；端侧基于用户近期画像动态预加载个性化子索引；推理时优先本地 RAG 检索，Cache Miss 时回源云端。
- **挑战**：子索引更新策略、同步通信成本、端侧近似近邻搜索性能。

#### 7.2.4 Beyond Recommendation: General Cloud-Device Collaborative Intelligence

- **文献状态**：本文的方法论贡献（PEFT Federated Learning、Device-Cloud Task Orchestration、Collaborative Personalization）可溢出到更广泛的端云 AI 场景。以下为推理性展望，非实证结论。
- **潜在迁移**：个性化教育（端侧学习行为/云侧全局课程图谱）、健康监测（本地生理数据隐私/云侧医学知识库）、自动驾驶（本地实时决策/云端全局地图规划）。
- **呼吁**：DCRec 社区为更广泛的端云协作智能提供可迁移的方法论框架。

---

## Chapter 8. Conclusion

> **目标**：~0.5 页双栏（约 400 词），简洁有力。

### 8.1 Summary of Contributions

- **写作要点**：
  - 重申本文对推荐系统三大范式的系统性梳理：
    - **OC（Ch3）**：三代入演进 + 每代内子分类（Memory/Model/Content-based → Feature/Sequential/Graph → Instruction/Generative/Agentic），为新人提供完整的云端推荐认知路径。
    - **OD（Ch4）**：部署轻量化（Embedding Slimming / KD / Compression）+ 在线适配（Privacy / Latency / Resource 三分），为端侧推荐建立清晰的分类框架。
    - **DCRec（Ch5）**：首次将该领域作为独立范式进行定义与划界，提出按协同目标的三机制主分类（Computation / Model / Task），制定明确的归属仲裁规则与辅助标签。
  - 强调三种个性化范式作为全文的概念主线：OC 实现 Contextual Personalization（基于全局上下文的差异化），OD 实现 Real-Time Personalization（基于本地数据的实时自适应），DCRec 通过 Computation / Model / Task 三类协同机制实现 Utility-Privacy-Efficiency Collaborative Personalization（端云协同的最优个性化）。
  - 系统综述 200+ 篇文献，识别 LLM 时代的前沿趋势与远期范式变革。

### 8.2 Key Takeaways

- **写作要点**（每点 1-2 句话）：
  1. **推荐系统的演化是端云分工不断细化的过程**——从纯云集中式计算到端云协同智能，本质上是将"理解"（云端）与"响应"（端侧）解耦并各自优化。
  2. **OC、OD、DCRec 各有不可替代的定位**：OC 提供全局协同信号与语义理解，OD 提供零延迟实时个性化，DCRec 通过三种协同机制将二者优势结构化地整合。
  3. **LLM 不仅增强了推荐能力，更重构了协同范式**——从参数共享到语义协作（Ch5.5.3），从全参同步到 Adapter 聚合（Ch5.4.3），从固定流水线到 Agent 对话（Ch7.2.2）。
  4. **本文的三大分类体系（OC 的三代+子类 / OD 的部署+适配 / DCRec 的三机制）为后续研究者提供了可操作的方法论框架**。

### 8.3 Closing Remarks

- **写作要点**：
  - 对未来 5 年的展望：端云协同推荐将从"专用系统"演变为"通用智能基础设施"。
  - 呼吁学术界与工业界建立更紧密的合作，推动 DCRec 从"经验驱动"走向"原理驱动"。
  - 希望本文的三维分类框架和评估建议能加速领域的收敛与成熟。

---

## References
