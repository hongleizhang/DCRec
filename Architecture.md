# Device-Cloud Personalization and Collaboration in Recommendation: A Survey

note
历程加入llm
参数量演进

### Part 1: 多维度分类法设计 (The Proposed Taxonomy)

* **(Why) Dimension 1: Objective-Oriented Paradigm (目标导向范式)**
  * *Privacy-Priority (Federated Recommendation, 联邦范式)*：传统联邦推荐本质上属于端云协同的一个子集，其核心目标是数据不出域，强调多方安全计算、本地差分隐私。
  * *Performance-Priority (Classic Device-Cloud Collaborative Rec, 经典端云范式)*：追求极致实时性能，云侧维护泛化大模型，端侧维护千人千面的实时轻量模型。
  * *Efficiency-Priority (Distributed/Edge-Assisted Rec, 分布式/边缘范式)*：跨WAN的广义分布式协同，强调将设备作为海量异构Worker，以极低的成本分担云端算力压力,追求极致的资源利用率，通过计算卸载、通信压缩解决算力瓶颈。
* **(Where) Dimension 2: Level-Oriented Architecture (层次导向架构) —— [本文的核心章节划分依据]**
  * *Data Collaboration (数据层)*：虚拟数据合成、基于差分隐私的软标签（Soft-labels）交互、User-Item局部交互图传输，解决“信息孤岛”与隐私的矛盾。
  * *Model Collaboration (模型层)*：云到端的知识蒸馏（KD）、端到云的实时个性化特征（Real-time Feedback）上报，解决端云异构算力与模型容量的矛盾。
  * *Computation Collaboration (计算层)*：训练态的Co-training（如异步/同步梯度调度）、推理态的Co-inference（如云端粗排+端侧精排/重排），解决训练/推理负载的全局最优分配问题。
* **(How) Dimension 3: Mechanism-Oriented Topology (机制导向拓扑)**
  * *Symmetric (Aggregation-based, 同构聚合型)*：云端与端侧拥有相同的网络骨架（如传统横向联邦推荐），同构模型参数/梯度的自下而上汇聚（如FedAvg）。
  * *Asymmetric (Distillation-based, 异构蒸馏/迁移型)*：云端大网络（大Embedding/大LLM）+ 端侧小网络（轻量级双塔/SLM/实时重排模块），异构模型之间的知识自上而下或双向传递（如KD, Soft-labels）。
  * *Split/Offloading-based (切分/卸载型)*：深度计算图的物理截断与接力（如Split Learning, 边缘重排）。

<!-- ![1774978651812](image/Architecture/1774978651812.png) -->
![1775005464195](image/Architecture/1775005464195.png)
![1774978772455](image/Architecture/1774978772455.png)

---

### Part 2: 详细论文大纲 (Detailed Paper Outline)

将核心部分按 **Level-Oriented (Data -> Model -> Computation)** 进行组织，这样符合机器学习Pipeline的自然逻辑，极具条理性。

#### **Title:** Device-Cloud Personalization and Collaboration in Recommendation: A Survey

**Abstract**

* (要点) 概述推荐系统的发展瓶颈；定义端云协同推荐的广义范式；介绍本文提出的多维度分类法；总结在LLM时代的未来挑战。

**1. Introduction**

* **1.1 Evolution and Bottlenecks of On-Cloud Recommendation**
  * **要点**：回顾云端推荐的演进（从CF、GNN到百亿参数的LLM架构），强调其优势在于拥有全局知识结构与极强的表达能力。然后笔锋一转，指出其面临的“三座大山”：网络延迟高导致无法捕捉秒级短期兴趣、集中式存储带来的隐私泄露风险、以及高昂的推理成本。
  * **过渡**：为了解决上述问题，研究者开始将目光转向离用户最近的设备端。
* **1.2 The Rise and Limitations of On-Device Recommendation**
  * **要点**：总结设备端推荐（On-device Rec）的核心技术栈，如模型量化、蒸馏、剪枝等压缩技术，强调其在隐私保护和零延迟（Zero-latency）上的优势。接着指出其致命瓶颈：端侧算力与存储受限（无法容纳千万级Item ID）、存在“信息孤岛”（无法获知其他用户的协同过滤信号）。
  * **过渡**：云端有全局知识和算力，端侧有实时特征和隐私，一种必然的趋势——端云协统范式应运而生。
* **1.3 The Paradigm Shift: Device-Cloud Collaboration**
  * **要点**：正式提出“端云协同推荐（Device-Cloud Recommendation）”这一终极范式。强调该范式的核心在于**Co-training & Co-inference**，完美中和了云与端的优缺点。
  * **过渡**：然而，该领域的现有研究庞杂且缺乏统一界定，引出本文的贡献。
![1774977597498](image/TOTO/1774977597498.png)

**2. Preliminaries and Taxonomy**

* **2.1 Problem Formulation**
  * *写作要点*：使用严谨的数学符号，统一表示端侧数据集、云端数据集、端侧模型($f_\theta$)、云端模型($F_\Phi$)及其优化目标。
* **2.2 The Proposed Taxonomy**
  * *写作要点*：引出前文设计的“目标-层次-机制”三维分类法，并配以Overview插图，为下文章节奠定基调。

**3. Data-Level Collaboration: Bridging Silos and Privacy**

* *段落总起*：本节主要探讨端云之间如何进行“数据/信息”层面的协同，核心矛盾是在不侵犯隐私的前提下实现全局协同。
* **3.1 Isolated Data Aggregation (联邦数据隔离协同)**
  * *写作要点*：回顾隐私优先的联邦推荐（FedRec），数据绝对物理隔离，仅交换中间梯度/参数。探讨Non-IID数据导致的客户端漂移（Client Drift）问题及解决策略。
* **3.2 Proxy Data and Soft-Label Communication (代理数据与软标签传输)**
  * *写作要点*：介绍性能优先的协同方式，如云端将全局特征转化为Soft-labels或Knowledge Embeddings下发到端侧（如KD机制），避免直接传输生数据。
* **3.3 Synthetic Data and Generative Sharing (合成数据与生成式共享)**
  * *写作要点*：探讨前沿的效率优先协同，利用生成模型（如GAN/Diffusion）在端侧生成脱敏的User-Profile或在云端生成Synthetic Item，进行端云交换以扩充样本空间。

**4. Model-Level Collaboration: Managing Heterogeneity and Capacity**

* *段落总起*：本节探讨面对端云几百倍的算力差异，如何设计协同的模型架构。
* **4.1 Symmetric Architecture (同构模型协同)**
  * *写作要点*：经典的联邦学习架构，端云维护结构完全相同的模型。重点分析其在算力受限设备上的缩放难题。
* **4.2 Asymmetric Architecture (异构模型协同/云大端小)**
  * *写作要点*：目前工业界最主流的架构（Performance-Priority）。云端部署大模型（百亿参数，负责泛化和召回），端侧部署个性化小模型（百万参数，负责实时重排/特征交互）。
* **4.3 Split Architecture (切分模型协同)**
  * *写作要点*：效率优先的Split Learning机制。将深层网络在某一层切断（Cut layer），底部在端侧运行提取特征，顶部在云端运行完成预测，重点综述切分点的动态优化策略。

**5. Computation-Level Collaboration: Co-Training and Co-Inference**

* *段落总起*：本节深入到计算负载的分配，探讨如何协同训练参数与协同推理推荐结果。
* **5.1 Collaborative Training Workloads (协同训练)**
  * *5.1.1 Synchronous vs. Asynchronous Parameter Updating*: 综述在不可靠网络下，端云梯度的异步更新机制与分布式参数服务器（Distributed PS）的实现。
  * *5.1.2 Meta-Learning for On-Device Personalization*: 云端学习Meta-knowledge，端侧利用几步本地Gradient Descent实现极其快速的个性化适应（如MAML在端云的变体）。
* **5.2 Collaborative Inference Workloads (协同推理)**
  * *5.2.1 Cloud-Retrieval & Device-Reranking (云端预排+端侧重排)*: 分析工业界标准的推理Pipeline，云端下发Top-K候选集，端侧结合设备实时Context（电量、滑动速度、最新点击）进行最终排序。
  * *5.2.2 Dynamic Early Exit (动态提前退出)*: 算力卸载策略。端侧先用轻量级模型评估，如果置信度极高则直接输出推荐结果；若遇到长尾/复杂请求，则fallback到云端大模型推理，节省网络带宽。

**6. System Evaluation, Simulation Platforms, and Datasets**

* *写作要点*：由于端云协同难以在真实环境复现，本节整理目前主流的仿真框架（如FedScale, Flower, EdgeRec等）和适合进行端云协同测试的Public Datasets（带有时间戳和设备属性的数据）。

**7. Future Directions (重点见下文 Part 3)**

* *写作要点*：展望大模型时代和异构硬件时代的新机遇。

**8. Conclusion**

---

### Part 3: Future Directions Highlights (大模型时代的端云协同前瞻)

在LLM（大语言模型/Foundation Models）席卷推荐领域的当下，端云协同的范式正在发生本质的改变（从单纯的参数同步转向语义和知识的协同）。

**1. Collaborative Inference in Rec-LLMs (大模型推荐的端云联合推理与投机解码)**

* **核心痛点**：基于生成式的推荐大模型（如基于LLaMA构建的推荐模型）推理延迟极高，无法满足工业界几十毫秒的要求。
* **未来方向**：将投机解码（Speculative Decoding）引入端云协同。端侧部署极小的语言模型（SLM, 如0.5B）作为Draft Model，快速生成推荐Item序列；云端的巨型LLM（如100B）作为Target Model进行批处理验证。同时，研究KV Cache的端云卸载与共享，打破自回归生成的延迟瓶颈。

**2. Parameter-Efficient Federated Fine-Tuning (面向推荐的端云参数高效联邦微调)**

* **核心痛点**：无法在端侧设备对几十亿参数的模型进行Full Fine-Tuning。
* **未来方向**：采用端云PEFT（Parameter-Efficient Fine-Tuning）架构。云端冻结巨大的Foundation Model，向端侧下发轻量级的LoRA/Adapter模块。端侧仅利用本地User Behavior微调几兆大小的Adapter。云端定期聚合这些个性化Adapter进行全局知识的融合与聚类（Federated LoRA）。

**3. Agentic Collaboration: Multi-Agent Cloud-Device Recommender (基于多智能体的端云协同推荐)**

* **核心痛点**：传统的端云协同过度依赖张量（Tensor）和梯度（Gradient）的传输，缺乏可解释性，且受限于严格的模型结构。
* **未来方向**：端云不再传递梯度，而是交互**自然语言或System Prompts**。云端充当“Global Orchestrator Agent”，掌握全域物品图谱；端侧充当用户的“Personal Assistant Agent”，完全掌握用户的隐私偏好。两者通过对话（Conversation）/隐式Prompt压缩的方式协商推荐列表，实现完全解耦的语义级协同。

**4. Synchronized On-Device RAG (端侧增强与云端RAG的动态同步)**

* **核心痛点**：大模型容易产生幻觉（Hallucination），而纯云端的RAG（检索增强生成）需要检索千万级Item Vector库，消耗巨大计算量且无法兼顾用户本地最新状态。
* **未来方向**：实现端云RAG的分布式缓存。云端维护全量物品的向量数据库（Vector DB）；端侧基于用户近期画像，本地预加载一个微型的个性化子索引（Sub-Index）。推理时，优先在本地进行轻量级RAG检索作为大模型的上下文，仅在Cache Miss时向云端发起检索，大幅降低云端向量数据库的QPS压力。
