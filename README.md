# Root Cause Analysis 文献


| 方法族 | 主要回答的问题 | 核心中间产物 |
| --- | --- | --- |
| 综述与评测 | 方法如何定义、比较和复现？ | 数据集、环境、指标、审计协议 |
| 统计/规则/模式 | 哪些观测首先显著异常？ | 变点、异常分数、趋势或可疑序列 |
| 依赖图传播 | 异常沿既有结构如何扩散？ | 加权依赖图、路径或随机游走分数 |
| 因果发现 | 哪些上游关系可能真正致因？ | 因果边、事件链或时间激励图 |
| 学习式图表示 | 如何从复杂关系中学习诊断表示？ | GNN/超图/动态图嵌入 |
| 反事实与干预 | 修复候选原因后异常是否消失？ | 反事实恢复量、干预效应、责任概率 |
| LLM/Agent | 如何检索证据、调用工具并组织推理？ | 推理轨迹、工具结果、解释与建议 |


---

## 1. 综述、数据集与评测（Surveys, Datasets and Evaluation）

- **方法共性**：这一类不直接提出定位模型，而是定义研究对象、数据边界和比较协议。共同链路是“任务定义 → 数据/环境 → 指标与报告协议 → 结论审计”。

### 1.1 综述与研究框架

这些工作反映近三年分类体系由“按单一遥测模态罗列”转向“方法机制、任务生命周期和部署约束联合分析”。

#### 1.1.1 微服务 RCL 专题综述

- Intelligent Root Cause Localization in MicroService Systems: A Survey and New Perspectives（ACM Computing Surveys, 2025）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3736755)]：梳理基于指标、日志、调用链及多模态可观测数据的微服务根因定位方法，并总结动态拓扑、多源融合、实时性、泛化性与可解释性等研究方向。

#### 1.1.2 方法—挑战—趋势全景综述

- A Comprehensive Survey on Root Cause Analysis in (Micro) Services: Methodologies, Challenges, and Trends（CoRR, 2024）[[PDF](https://arxiv.org/pdf/2408.00803)]：按指标、调用链、日志、多模态及 LLM 增强方法梳理微服务 RCA，并总结数据质量、动态依赖、故障传播、评测标准和自动化诊断等挑战。

### 1.2 方法与报告协议审计

重点不再只是比较平均准确率，而是检查参数鲁棒性、真实系统外推能力和跨系统排名反转。

#### 1.2.1 因果算法有效性与参数鲁棒性

- Root Cause Analysis for Microservice System based on Causal Inference: How Far Are We?（ASE, 2024）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3691620.3695065)] [[CODE](https://github.com/phamquiluan/RCAEval/tree/ase24)]：在合成数据与真实微服务数据上评测 9 种因果发现方法和 21 种 RCA 方法，发现现有方法难以同时兼顾准确性、效率与参数鲁棒性，且合成数据排名不能可靠外推到真实系统。

#### 1.2.2 汇总榜单与跨系统胜负反转

- Pooled Leaderboards Hide System-Specific Winners: A Reporting-Protocol Audit of Offline Root-Cause Analysis Benchmarks（CoRR, 2026）[[PDF](https://arxiv.org/pdf/2606.29159)]：审计 OpenRCA、RCAEval 和 PetShop 的 11 个子系统、778 个案例，证明汇总 Top-1 排名会掩盖不同系统之间的方法胜负反转。

### 1.3 数据集与评测环境

基准从静态故障标签扩展到日志成本、多模态证据、代码级标签和可交互 Agent 诊断轨迹。

#### 1.3.1 LLM 日志压缩成本—质量基准

- LogDx-CI: Benchmarking Log Reduction Tools for LLM Root-Cause Diagnosis（CoRR, 2026）[[PDF](https://arxiv.org/pdf/2605.28876)] [[CODE](https://github.com/eyuansu62/LogDx)]：在 35 个真实 GitHub Actions 故障上评测 11 种日志压缩方法，发现 grep＋tail 混合路由具有较好的成本—质量权衡，而 Agent 可弥补低质量上下文但会显著增加诊断成本。

#### 1.3.2 可复现云环境与 Agent 轨迹评测

- Cloud-OpsBench: A Reproducible Benchmark for Agentic Root Cause Analysis in Cloud Systems（CoRR, 2026）[[PDF](https://arxiv.org/pdf/2603.00468)] [[CODE](https://github.com/LLM4Ops/Cloud-OpsBench)]：通过 Kubernetes 状态快照构建可确定性复现的数字孪生，提供覆盖 40 类根因、452 个故障案例的交互式 Agent RCA 基准，并同时评估最终结果与诊断轨迹。

#### 1.3.3 微服务五模态与代码级标签

- AnoMod: A Dataset for Anomaly Detection and Root Cause Analysis in Microservice Systems（CoRR, 2026）[[PDF](https://arxiv.org/pdf/2601.22881)] [[CODE](https://github.com/EvoTestOps/AnoMod)]：在 SocialNetwork 和 TrainTicket 中注入性能、服务、数据库及代码级异常，采集日志、指标、调用链、API 响应和代码覆盖率五种模态，支持服务级与代码级定位。

#### 1.3.4 IT/OT 多域多模态统一数据集

- LEMMA-RCA: A Large Multi-modal Multi-domain Dataset for Root Cause Analysis（CoRR, 2024）[[PDF](https://arxiv.org/pdf/2406.05375)] [[CODE](https://github.com/KnowledgeDiscovery/rca_baselines)]：构建覆盖微服务、云计算、水处理与配水等 IT/OT 场景的多域多模态数据集，提供故障时间和根因实体标注，用于统一评测离线/在线及单模态/多模态方法。

### 1.4 综述与可信评测

#### 1.4.1 微服务故障诊断统一综述

- Failure Diagnosis in Microservice Systems: A Comprehensive Survey and Analysis（ACM Transactions on Software Engineering and Methodology, 2026）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3715005)]：系统梳理 98 篇微服务故障检测、根因定位和故障分类研究，统一分析多模态数据、系统架构、任务定义、数据集、工具与评测指标，并总结工业实践与研究空白。

#### 1.4.2 遥测数据统一评测

- RCAEval: A Benchmark for Root Cause Analysis of Microservice Systems with Telemetry Data（The Web Conference Companion, 2025）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3701716.3715290)][[CODE](https://github.com/phamquiluan/RCAEval)]：统一封装多种微服务 RCA 方法与多代公开遥测数据集，提供可复现实验入口，用于比较不同系统、故障类型、数据窗口和算法组合下的定位性能。

#### 1.4.3 真实软件故障上的 LLM 评测

- OpenRCA: Can Large Language Models Locate the Root Cause of Software Failures?（ICLR, 2025）[[PDF](https://openreview.net/pdf?id=M4qNIzQYpd)][[CODE](https://github.com/microsoft/OpenRCA)]：构建包含三个企业软件系统、335 个真实故障及日志、指标和调用链的开放基准，并以可调用分析工具的 RCA-Agent 评估大模型的跨模态取证与推理能力。

#### 1.4.4 故障传播感知评测

- Rethinking the Evaluation of Microservice RCA with a Fault Propagation-Aware Benchmark（FSE, 2026）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3797100)][[CODE](https://zenodo.org/records/19494726)]：构建含复杂故障传播、动态负载和服务到代码分层真值的大规模基准，重新评测 11 种方法并揭示简单公开数据对现有 RCA 性能的系统性高估。

---

## 2. 统计检测、规则学习与序列/模式挖掘（Statistical, Rule and Pattern-based RCA）

- **方法共性**：共同做法是先把原始遥测转成变点、异常分数、趋势符号或可疑序列，再依据显著性、先后关系、相关性或模式频率缩小根因候选。

### 2.1 日志、Trace 与轻量监督方法

这些工作按实际机制归类，不因发表年份较新就自动标为“最新进展”。

#### 2.1.1 异常日志自动标注（RCA 前置环节）

- Progressing from Anomaly Detection to Automated Log Labeling and Pioneering Root Cause Analysis（CoRR, 2023）[[PDF](https://arxiv.org/abs/2312.14748)] [[CODE](https://github.com/dos-group/LogLAB)]：提出日志模板/属性/上下文异常分类体系，并用告警时间窗和 PU 弱监督自动标注异常日志；论文仅设想基于日志行分数的 RCA，尚未实现和评测完整根因定位，属于邻接工作。

#### 2.1.2 Trace 频繁项集与可疑服务集合

- Practical Root Cause Localization for Microservice Systems via Trace Analysis（IWQoS, 2021）[[PDF](https://netman.aiops.org/wp-content/uploads/2021/05/1570705191.pdf)] [[CODE](https://github.com/NetManAIOps/TraceRCA)]：对 Span/Invocation 多维指标做无监督异常检测，将请求划分为正常与异常 Trace，再以 FP-Growth 挖掘异常 Trace 高频、正常 Trace 低频的可疑服务集合。

#### 2.1.3 边云 KPI 的 SVM/随机森林分类

- ML Driven Root Cause Analysis (RCA) in Telco Microservices Continuum（ICC Workshops, 2024）[[PDF](https://ieeexplore.ieee.org/document/10615702)] [[CODE](https://github.com/ZHAW-BA-2023-FS-RCA-Mon/maleaf)]：MALEAF 周期采集边云微服务的延迟、可用性、CPU 和内存指标，先检测 SLA 异常，再用 SVM 与随机森林估计根因服务及流量、可靠性或性能故障类型。

### 2.2 稳健统计与细粒度序列证据

这一方向的新变化是更稳健地确定故障区间，并把日志、Trace、信令和指标之间的证据关联到更细定位粒度。

#### 2.2.1 多变量贝叶斯在线变点＋稳健排序

- BARO: Robust Root Cause Analysis for Microservices via Multivariate Bayesian Online Change Point Detection（FSE, 2024）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3660805)] [[CODE](https://github.com/phamquiluan/baro)]：以多变量贝叶斯在线变点检测联合建模指标依赖并确定异常区间，再通过非参数统计检验 RobustScorer 排序根因，降低异常区间偏差对 RCA 的影响。

#### 2.2.2 时序分解＋SAX 趋势压缩

- KPIRoot+: An Efficient Integrated Framework for Anomaly Detection and Root Cause Analysis in Large-Scale Cloud Systems（Empirical Software Engineering, 2026）[[PDF](https://link.springer.com/article/10.1007/s10664-025-10769-0)]：通过时序分解检测 KPI 异常、改进 SAX 压缩变化趋势，再融合趋势相似性与变化先后关系定位根因 KPI/VM；原清单中的代码仓库实际对应前作 KPIRoot，故不再标为本论文代码。

#### 2.2.3 PU 弱监督日志证据学习

- LogRCA: Log-Based Root Cause Analysis for Distributed Services（Euro-Par, 2024）[[PDF](https://arxiv.org/pdf/2405.13599)]：采用半监督 PU Learning＋Transformer 为故障前日志行计算根因相关分数，并用聚类与数据重平衡处理罕见和未知故障，最终返回可共同解释故障的最小日志集合。

#### 2.2.4 关键路径＋对比 Trace 序列模式

- Trace-based Multi-Dimensional Root Cause Localization of Performance Issues in Microservice Systems（ICSE, 2024）[[PDF](https://ieeexplore.ieee.org/abstract/document/10549362)]：TraceContrast 从调用链提取关键路径，把服务、实例、版本和请求属性编码为事件序列，再通过对比序列模式挖掘与频谱分析定位多维根因。

#### 2.2.5 5G 信令轨迹重构与模式评分

- STRCA: A Lightweight and Accurate Root Cause Analysis System Based on 5G Signalling Trace（ICIC, 2024）[[PDF](https://link.springer.com/chapter/10.1007/978-981-97-5672-8_4)]：从海量 5G 核心网信令报文重构端到端信令轨迹，检测异常轨迹后挖掘可疑网元集合，并以专门评分函数定位根因网元。

#### 2.2.6 Trace 症状—指标相关的分层定位

- HeMiRCA: Fine-Grained Root Cause Analysis for Microservices with Heterogeneous Data Sources（ACM Transactions on Software Engineering and Methodology, 2024）[[PDF](https://dl.acm.org/doi/full/10.1145/3674726)] [[CODE](https://github.com/zhouruixingzhu/HeMiRCA)]：从 Span 延迟构造时序异常分数，再用 Spearman 相关性衡量该分数与各服务指标的单调关联，分层定位根因服务和根因指标。

### 2.3 事件图与端到端日志诊断

#### 2.3.1 工业事件因果图

- Groot: An Event-graph-based Approach for Root Cause Analysis in Industrial Settings（ASE, 2021）[[PDF](https://taoxiease.github.io/publications/ase21-groot.pdf)]：把指标、日志和运维活动归一为事件并实时构建因果事件图，同时允许 SRE 注入领域规则，在 eBay 数千个生产服务和 952 起真实事故上执行根因排序。

#### 2.3.2 日志检测—定位交互式多任务学习

- United We Stand: Towards End-to-End Log-based Fault Diagnosis via Interactive Multi-Task Learning（ASE, 2025）[[PDF](https://arxiv.org/pdf/2509.24364)]：提出 Chimera，在数据、特征和诊断结果三个层次双向传递异常检测与根因定位知识，以端到端多任务学习减少误差累积并降低对昂贵多模态监控的依赖。

---

## 3. 依赖图传播、随机游走与案例匹配（Dependency Graph and Propagation Ranking）

- **方法共性**：这类方法通常不重新发现因果结构，而是在已有服务、实例、主机或专家关系图上注入异常权重，再执行路径搜索、随机游走或历史图匹配。

### 3.1 层次图、属性图与历史案例匹配

#### 3.1.1 服务—指标层次图与路径搜索

- CauseInfer: Automated End-to-End Performance Diagnosis with Hierarchical Causality Graph in Cloud Environment（IEEE Transactions on Services Computing, 2019）[[PDF](https://ieeexplore.ieee.org/document/7563819)]：用通信流量的时滞相关性构建服务层依赖图，以 PC 算法学习服务内部指标图，形成两层层次图并沿异常路径 DFS 定位故障服务和根因指标。

#### 3.1.2 服务—主机属性图＋个性化 PageRank

- MicroRCA: Root Cause Localization of Performance Issues in Microservices（NOMS, 2020）[[PDF](https://ieeexplore.ieee.org/document/9110353)] [[CODE](https://github.com/elastisys/MicroRCA)]：融合调用关系、响应时间和主机资源利用率构建服务—主机属性图，再结合异常传播权重、指标相关性与个性化 PageRank 排序根因服务。

#### 3.1.3 异常子图知识库＋加权相似度

- Graph-based Root Cause Analysis for Service-Oriented and Microservice Architectures（Journal of Systems and Software, 2020）[[PDF](https://www.sciencedirect.com/science/article/pii/S0164121219302067)]：将服务、容器、主机及逻辑/物理连接表示为带属性系统图，从当前状态提取异常子图，并与历史已诊断异常图做加权相似度匹配以确定根因。

### 3.2 细粒度、去中心化与跨层传播

新进展主要体现在历史故障迁移、实例级拓扑、边缘去中心化和通信跨层因果链。

#### 3.2.1 历史故障子图迁移＋PageRank

- MicroHFRCL: A History Faults Based Root Cause Localization Framework in Microservice Systems（IJCNN, 2024）[[PDF](https://ieeexplore.ieee.org/document/10650929)]：以 GCN＋Transformer 编码历史故障异常子图，用当前故障与历史故障的相似度调整实例因果图权重，再以 PageRank 排序根因实例。

#### 3.2.2 服务—实例—主机异构随机游走

- MicroIRC: Instance-level Root Cause Localization for Microservice Systems（Journal of Systems and Software, 2024）[[PDF](https://www.sciencedirect.com/science/article/pii/S0164121224001900)] [[CODE](https://github.com/WHU-AISE/MicroIRC)]：由实例级指标学习 MetricSage 异常表示，并结合服务—实例—主机异构加权拓扑和个性化随机游走生成实例级根因排序。

#### 3.2.3 分簇去中心化个性化 PageRank

- A Decentralized Root Cause Localization Approach for Edge Computing Environments（IEEE Transactions on Services Computing, 2026）[[PDF](https://ieeexplore.ieee.org/document/11414199)]：按通信和共置关系把边缘微服务划为多个集群，各集群独立执行分层异常评分与个性化 PageRank，再通过点对点交换近似分数处理跨集群传播。

#### 3.2.4 5G—传输—应用专家因果链匹配

- Automated, Cross-Layer Root Cause Analysis of 5G Video-Conferencing Quality Degradation（ACM Internet Measurement Conference, 2025）[[PDF](https://dl.acm.org/doi/10.1145/3730567.3764434)][[CODE](https://github.com/PrincetonUniversity/Domino-IMC)]：Domino 将 5G PHY/MAC/RLC、传输层和 WebRTC 遥测转成跨层事件，并在专家因果图中匹配从底层无线或网络因素传播至视频卡顿、码率下降的因果链。

### 3.3 性能调试、历史故障与事故图

#### 3.3.1 主动式性能违约预测与定位

- Seer: Leveraging Big Data to Navigate the Complexity of Performance Debugging in Cloud Microservices（ASPLOS, 2019）[[PDF](https://www.csl.cornell.edu/~delimitrou/papers/2019.asplos.seer.pdf)]：结合轻量 RPC 调用追踪与底层硬件监控，以深度模型提前预测 QoS 违约并定位最先触发性能退化的微服务，实现主动式性能根因诊断。

#### 3.3.2 反事实性能调试与纠正动作

- Sage: Practical & Scalable ML-Driven Performance Debugging in Microservices（ASPLOS, 2021）[[PDF](https://www.csl.cornell.edu/~delimitrou/papers/2021.asplos.sage.pdf)]：以无监督 VAE 和因果贝叶斯网络建模服务依赖与性能指标，再通过反事实推断在线识别 QoS 违约的根因服务并推荐纠正动作。

#### 3.3.3 历史故障依赖图与案例解释

- Actionable and Interpretable Fault Localization for Recurring Failures in Online Service Systems（ESEC/FSE, 2022；DéjàVu）[[PDF](https://netman.aiops.org/wp-content/uploads/2022/11/DejaVu-paper.pdf)][[CODE](https://github.com/NetManAIOps/DejaVu)]：从历史故障和组件依赖学习故障依赖图，对新故障同时定位故障组件与指标组，并用全局模型和相似历史案例提供可执行解释。

#### 3.3.4 级联事故图抽取与诊断

- Graph based Incident Extraction and Diagnosis in Large-Scale Online Systems（ASE, 2022；GIED）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3551349.3556904)]：从海量异常问题中抽取包含症状、受影响服务和属性的级联传播图，再以图神经网络识别真实事故并支持事故诊断。

#### 3.3.5 Trace 与性能剖析上下文联合定位

- CARE: Context Aware Root Cause Identification Using Distributed Traces and Profiling Metrics（IEEE Transactions on Software Engineering, 2025）[[PDF](https://ieeexplore.ieee.org/document/11262552)]：融合分布式调用链和性能剖析指标，以网络分析衡量服务、服务社区及请求上下文的重要性，再通过加权频谱分析定位单根因与双根因。

#### 3.3.6 调用指标歧义消解

- CMDiagnostor: An Ambiguity-Aware Root Cause Localization Approach Based on Call Metric Data（The Web Conference, 2023）[[PDF](https://nkcs.iops.ai/wp-content/uploads/2023/02/WWW23-CMDiagnostor.pdf)][[CODE](https://github.com/NetManAIOps/CMDiagnostor)]：针对聚合调用指标无法准确恢复上下游请求路径的问题，用流量回归消除构图歧义，再通过异常检测、传播链剪枝与候选排序定位根因服务。

---

## 4. 因果发现、时间因果与事件因果图（Causal Discovery and Event Causal Graphs）

- **方法共性**：共同目标是从相关症状中识别上游原因：先学习或约束因果边，再结合时间方向、干预偏离或事件传播权重对候选根因排序。

### 4.1 约束式因果发现与多模态事件图

#### 4.1.1 观测—软干预联合局部发现

- Root Cause Analysis of Failures in Microservices through Causal Discovery（NeurIPS, 2022）[[PDF](https://dl.acm.org/doi/10.5555/3600270.3602529)] [[CODE](https://github.com/azamikram/rcd)]：把故障前后指标分别视为观测数据与软干预数据，引入故障指示节点，并通过改进 Ψ-PC 与分治式局部学习把该节点的干预目标识别为根因指标。

#### 4.1.2 强化学习剪枝＋条件独立性检验

- TraceDiag: Adaptive, Interpretable, and Efficient Root Cause Analysis on Large-Scale Microservice Systems（ESEC/FSE, 2023）[[PDF](https://dl.acm.org/doi/10.1145/3611643.3613864)]：从调用链构建服务依赖图和时序特征，先用强化学习自适应剪除无关服务，再在裁剪图上执行条件独立性检验和因果根因排序。

#### 4.1.3 正常/故障事件图差分

- Nezha: Interpretable Fine-Grained Root Causes Analysis for Microservices on Multi-modal Observability Data（ESEC/FSE, 2023）[[PDF](https://dl.acm.org/doi/10.1145/3611643.3616249)] [[CODE](https://github.com/IntelligentDDS/Nezha)]：把指标、日志与调用链统一转成事件，分别构建正常期和故障期事件图，再通过事件模式差异把根因定位到服务、代码区域或资源类型。

#### 4.1.4 跨层实体因果图与代码级映射

- TrinityRCL: Multi-Granular and Code-Level Root Cause Localization Using Multiple Types of Telemetry Data in Microservice Systems（IEEE Transactions on Software Engineering, 2023）[[PDF](https://ieeexplore.ieee.org/document/10034937)]：融合指标、日志与调用链构建跨应用、服务、主机、指标和代码实体的因果图，并按异常相关性进行多粒度排序，日志可进一步映射到代码位置。

### 4.2 非侵入采集与时间因果

近期方法开始显式处理采集侵入性、神经 Granger、离散事件激励和外生干预解释。

#### 4.2.1 eBPF 非侵入采集＋拓扑约束发现

- NRCAC: Non-Intrusive Microservice Root Cause Analysis Framework for Cloud Providers（INFOCOM, 2025）[[PDF](https://ieeexplore.ieee.org/document/11044716)] [[CODE](https://github.com/micturkey/NRCAC)]：用 eBPF 从宿主机内核无侵入采集 CPU、内存和 TCP 调用负载等时间序列，再通过 RCD-DK 把服务依赖约束注入局部因果发现并定位根因。

#### 4.2.2 对比表示＋神经 Granger

- Root Cause Analysis in Microservice Using Neural Granger Causal Discovery（AAAI, 2024）[[PDF](https://arxiv.org/abs/2402.01140)] [[CODE](https://github.com/zmlin1998/RUN)]：通过对比学习增强多变量指标表示，利用预测模型执行神经 Granger 因果发现并构图，最后以个性化 PageRank 排序 Top-k 根因。

#### 4.2.3 指标/审计事件化＋Hawkes 激励图

- MHP-RCA: Multivariate Hawkes Process-based Root Cause Analysis in Microservice Systems（Information and Software Technology, 2026）[[PDF](https://www.sciencedirect.com/science/article/pii/S0950584925002770)] [[CODE](https://github.com/WHU-AISE/MHP-RCA)]：把指标时间序列与审计日志转为异常事件序列，以多变量 Hawkes 过程学习事件间的时序激励图，并把根因细化到服务进程。

#### 4.2.4 正常外生分布＋Granger 干预归因

- Root Cause Analysis of Anomalies in Multivariate Time Series through Granger Causal Discovery（ICLR, 2025）[[PDF](https://openreview.net/pdf?id=k38Th3x4d9)] [[CODE](https://github.com/hanxiao0607/AERCA)]：AERCA 通过编码器—解码器联合学习 Granger 因果结构和正常外生变量分布，将异常视为外生干预，并按其偏离正常分布的程度识别根因。

### 4.3 多模态与动态事件因果

这一支线由静态统一图转向模态共享/特有结构、事故特定事件链和动态调用图。

#### 4.3.1 模态共享/特有因果结构学习

- MULAN: Multi-modal Causal Structure Learning and Root Cause Analysis for Microservice Systems（The Web Conference, 2024）[[PDF](https://dl.acm.org/doi/10.1145/3589334.3645442)]：从指标和日志学习模态共享/特有表示与各自因果图，以 KPI 感知注意力融合最终因果结构，再通过带重启随机游走排序根因实体。

#### 4.3.2 监督加权事件因果链

- Chain-of-Event: Interpretable Root Cause Analysis for Microservices through Automatically Learning Weighted Event Causal Graph（FSE Companion, 2024）[[PDF](https://dl.acm.org/doi/10.1145/3663529.3663827)] [[CODE](https://github.com/NetManAIOps/Chain-of-Event)]：把多模态遥测转成事件，用历史事故监督学习事件重要度与因果边权，构建事故特定的加权事件图并按事件链贡献定位根因。

#### 4.3.3 动态调用图＋因果优先成对排序

- DynaCausal: Dynamic Causality-Aware Root Cause Analysis for Distributed Microservices（CoRR, 2025）[[PDF](https://arxiv.org/abs/2510.22613)]：融合指标、日志和调用链构建动态加权调用图，通过 Transformer、混合感知图注意力、动态对比学习和因果优先成对排序定位根因服务。

### 4.4 干预、时滞、盲区与泛化

#### 4.4.1 在线服务干预识别

- Causal Inference-Based Root Cause Analysis for Online Service Systems with Intervention Recognition（KDD, 2022；CIRCA）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3534678.3539041)][[CODE](https://github.com/NetManAIOps/CIRCA)]：把在线服务 RCA 形式化为干预识别问题，利用系统架构构建因果贝叶斯网络，并通过故障前后条件分布变化定位根因指标。

#### 4.4.2 有限可观测条件下的潜空间干预

- Microservice Root Cause Analysis With Limited Observability Through Intervention Recognition in the Latent Space（KDD, 2024；LatentScope）[[PDF](https://nkcs.iops.ai/wp-content/uploads/2024/09/SIGKDD_24_LatentScope.pdf)][[CODE](https://github.com/eBay/LatentScope)]：把服务、Pod、主机等异构根因组件建模为潜变量，并在指标有限和组件粒度不一致的条件下通过潜空间干预识别完成量化定位。

#### 4.4.3 多时滞故障传播

- Bridging the Delay: Lag-Aware Spatio-Temporal Causal Inference for Microservice Root Cause Analysis（FSE Industry, 2026；LagRCA）[[PDF](https://netman.aiops.org/wp-content/uploads/2026/04/fse2026-industry-paper77.pdf)]：显式建模上下游症状之间异质且动态的传播时滞，区分服务因果影响与指标共振，并输出可解释的传播路径。

#### 4.4.4 调用图盲区下的多源定位

- TORAI: Multi-Source Root Cause Analysis for Blind Spots in the Microservice Service Call Graph（FSE, 2026）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3808137)][[CODE](https://github.com/phamquiluan/RCAEval/tree/fse26)]：针对黑盒服务或缺失 Trace 形成的调用图盲区，从现有多源遥测估计异常强度、聚类症状、执行簇内因果排序并以假设检验定位细粒度根因。

#### 4.4.5 元因果知识驱动的跨系统泛化

- MetaRCA: A Generalizable Root Cause Analysis Framework for Cloud-Native Systems Powered by Meta Causal Knowledge（FSE, 2026）[[PDF](https://arxiv.org/pdf/2603.02032)]：离线融合 LLM、历史故障报告和可观测数据构建可跨拓扑复用的元因果图，在线按目标系统上下文实例化、加权和剪枝以实现可扩展定位。

#### 4.4.6 多根因主动干预

- Towards the Localization of Multi-Root-Cause Failures in Microservice Systems: An Active Intervention Framework（FSE, 2026）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3808180)]：用分层强化学习提出候选根因，以干预增强图注意力网络预测根因可能触发的故障场景，并通过预测场景与实时状态的迭代比较主动修正多根因结果。

---

## 5. 学习式图表示、多模态融合与联合诊断（Learned Graph Representation and Multimodal Fusion）

- **方法共性**：共同模式是“构图 → 学习节点/关系表示 → 聚合多模态证据 → 联合输出定位或故障类型”。差异主要在动态图、异构图、超图、层次图和跨域对齐。

### 5.1 动态图与异构关系表示

#### 5.1.1 动态服务图＋GNN/GRU 联合预测定位

- InstantOps: A Joint Approach to System Failure Prediction and Root Cause Identification in Microservices Cloud-Native Applications（ICPE, 2024）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3629526.3645047)]：融合日志、运行事件、指标和调用链，以调用链构建动态服务图、日志与指标作为节点特征，再用 GNN＋GRU 联合完成故障提前预测和服务级根因定位。

#### 5.1.2 资源实体关系图＋掩码 GAT

- Holistic Root Cause Analysis for Failures in Cloud-Native Systems Through Observability Data（IEEE Transactions on Services Computing, 2024）[[PDF](https://ieeexplore.ieee.org/document/10713920)] [[CODE](https://github.com/baiyanquan/HolisticRCA)]：把指标、日志与调用链编码到共享空间，按主机、Pod、服务等资源实体融合，再在实体关系图上以 GAT＋掩码学习定位跨层根因实体和关键异常观测。

#### 5.1.3 告警异常关联图＋GAT/LightGBM

- Root Cause Analysis of Cloud Platform Faults Based on Anomaly Correlation Graph and Graph Neural Network（Engineering Applications of Artificial Intelligence, 2026）[[PDF](https://www.sciencedirect.com/science/article/pii/S0952197626007323)]：从告警日志和拓扑依赖学习异常事件结构，采用邻居采样与 GAT 聚合图特征，再由 LightGBM 定位根因告警节点并借历史案例生成解释。

#### 5.1.4 5G 动态邻接＋GCN/Transformer

- Root Cause Analysis of Anomalies in 5G RAN Using Graph Neural Network and Transformer（CoRR, 2024）[[PDF](https://arxiv.org/abs/2406.15638)] [[CODE](https://github.com/PINetDalhousie/Simba)]：Simba 从基站 KPI 动态学习邻接关系，以 GCN 提取空间传播特征、Transformer 建模时间依赖，联合识别异常类型并定位受影响基站。

### 5.2 超图、层次图与边云协同

#### 5.2.1 级联条件学习＋异构超图

- Root Cause Analysis for Microservice Systems via Cascaded Conditional Learning with Hypergraphs（CoRR, 2025）[[PDF](https://arxiv.org/abs/2511.17566)]：CCLH 融合指标、日志和调用链，构建含调用、部署及负载均衡关系的异构超图，以 UniGAT-HE 建模群体传播并级联完成根因实例定位和故障类型识别。

#### 5.2.2 内核故障解析＋异构动态拓扑栈

- Root Cause Localization for Microservice Systems in Cloud-edge Collaborative Environments（CoRR, 2024）[[PDF](https://arxiv.org/abs/2406.13604)][[CODE](https://github.com/WDCloudEdge/MicroCERCL)]：MicroCERCL 先从通信内核日志和网络报文定位断连/丢包等内核根因，再以服务—实例—服务器动态拓扑栈、类型特定图卷积、LSTM 与图注意力池化输出应用级排名。

#### 5.2.3 簇内节点定位＋簇级故障类型级联

- A Cascaded Graph Neural Network for Joint Root Cause Localization and Analysis in Edge Computing Environments（CoRR, 2026）[[PDF](https://arxiv.org/abs/2603.01447)]：先对通信图执行 Louvain 聚类，用 P-Net 在簇内定位根因节点，再由 O-Net 在簇级图识别故障类型，并通过多任务损失联合优化 RCL 与 RCA。

### 5.3 无标签目标系统的跨域适配

#### 5.3.1 域对抗多模态对齐＋传播规则 PageRank

- UDA-RCL: Unsupervised Domain Adaptation for Microservice Root Cause Localization Utilizing Multimodal Data（IEEE Transactions on Services Computing, 2026）[[PDF](https://ieeexplore.ieee.org/document/11304608)] [[CODE](https://github.com/xsarvin/UDA-RCL)]：把日志、指标与调用链聚合为统一事件表示，通过多模态编码器和域对抗训练对齐成熟源系统与无标签目标系统，再用嵌入异常传播规则的 PageRank 定位根因服务。

### 5.4 多模态联合诊断与多粒度输出

#### 5.4.1 异常检测—定位联合学习

- Eadro: An End-to-End Troubleshooting Framework for Microservices on Multi-source Data（ICSE, 2023）[[PDF](https://arxiv.org/pdf/2302.05092)]：联合编码日志、指标和调用链，在共享服务状态表示上共同优化异常检测与根因定位，避免两阶段方法重复处理数据和累积诊断偏差。

#### 5.4.2 根因实例与故障类型联合诊断

- Robust Failure Diagnosis of Microservice System through Multimodal Data（IEEE Transactions on Services Computing, 2023；DiagFusion）[[PDF](https://nkcs.iops.ai/wp-content/uploads/2025/09/Robust_Failure_Diagnosis_of_Microservice_System_Through_Multimodal_Data.pdf)][[CODE](https://github.com/AIOps-Lab-NKU/DiagFusion)]：将指标、日志和调用链编码为服务实例表征，结合部署/调用依赖图与图神经网络联合输出根因实例和故障类型。

#### 5.4.3 时序知识图谱统一多源诊断

- No More Data Silos: Unified Microservice Failure Diagnosis With Temporal Knowledge Graph（IEEE Transactions on Services Computing, 2024；UniDiag）[[PDF](https://nkcs.iops.ai/wp-content/uploads/2025/09/No_More_Data_Silos_Unified_Microservice_Failure_Diagnosis_With_Temporal_Knowledge_Graph.pdf)][[CODE](https://github.com/AIOps-Lab-NKU/UniDiag)]：用时序知识图谱统一指标、日志和调用链，通过动态图嵌入同时支持故障检测、根因定位与故障类型识别。

#### 5.4.4 有效跨模态融合

- FAMOS: Fault Diagnosis for Microservice Systems through Effective Multi-Modal Data Fusion（ICSE, 2025）[[PDF](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=11029848)]：用独立编码器保留各模态内部特征，再以高斯注意力和交叉注意力学习模态间关系，联合完成微服务根因服务定位和故障类型诊断。

#### 5.4.5 多粒度异构超图

- Hypergraph Neural Network-based Multi-Granular Root Cause Localization for Microservice Systems（ASE, 2025；HyperRCA）[[PDF](https://doi.org/10.1109/ASE63991.2025.00099)]：以实例为节点、以部署/隶属/依赖关系构造三类超边，再用超图神经网络同时定位主机、服务和实例粒度的根因。

#### 5.4.6 分布式 LLM 应用多层根因分析

- LLMRCA: Multilevel Root Cause Analysis for LLM Applications Using Multimodal Observability Data（ACM Transactions on Software Engineering and Methodology, 2026）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3806200)][[CODE](https://github.com/IntelligentDDS/LLMRCA)]：面向分布式 LLM 应用构建指标、日志和调用链异构因果图，以残差图注意力自编码器联合定位主机、组件、代码和应用层根因，并处理时延与回答质量静默故障。

#### 5.4.7 大模型与小分类器协作的少样本诊断

- The Potential of One-Shot Failure Root Cause Analysis: Collaboration of the Large Language Model and Small Classifier（ASE, 2024）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3691620.3695475)]：以 LLM 从极少故障样本和系统信息中提炼根因语义，再与轻量分类器协同完成 one-shot 根因识别，兼顾语义推理与低成本部署。

#### 5.4.8 Trace/日志筛选后的指标级因果定位

- MRCA: Metric-level Root Cause Analysis for Microservices via Multi-Modal Data（ASE, 2024）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3691620.3695485)]：先用 Trace 和日志重构概率得到异常服务排序，再只在高风险服务上扩展指标因果图，以奖励机制控制扩张并剪枝得到指标级根因。

#### 5.4.9 检测—分诊—定位统一无监督建模

- ART: A Unified Unsupervised Framework for Incident Management in Microservice Systems（ASE, 2024）[[PDF](https://nkcs.iops.ai/wp-content/uploads/2024/12/ASE24ART.pdf)]：依次以 Transformer、GRU 和 GraphSAGE 建模通道、时间与调用依赖，抽取可解释的统一故障表征，并在一个无监督框架中完成异常检测、故障分诊和根因定位。

---

## 6. 反事实、干预、概率责任与安全归因（Counterfactual, Intervention and Responsibility）

- **方法共性**：这类方法不只观察共同变化，而是构造“如果修复候选原因会怎样”的替代世界，或估计节点、故障类型及安全事件的责任概率。

### 6.1 概率责任与故障树方法

这两项按贝叶斯责任和故障树机制归类；即使其中一篇发表于近年，也不因年份自动标成方法前沿。

#### 6.1.1 SLA 违约责任贝叶斯网络

- Root Cause and Liability Analysis in the Microservices Architecture for Edge IoT Services（ICC, 2023）[[PDF](https://ieeexplore.ieee.org/document/10279721)]：从故障注入数据构建连接服务故障和性能指标的因果贝叶斯网络，在 SLA 违约时推断各服务及故障类型的责任概率，并识别未履约服务提供方。

#### 6.1.2 故障树最小割集＋贝叶斯后验风险

- Advancing Autonomous Vehicle Safety: A Combined Fault Tree Analysis and Bayesian Network Approach（ERAS, 2025）[[PDF](https://arxiv.org/abs/2504.08206)]：以车辆碰撞为顶事件建立故障树和最小割集，再映射为贝叶斯网络，在安全目标约束下量化并排序传感器、感知、决策、控制与交互子系统的风险贡献。

### 6.2 结构因果与修复干预

新变化是从单变量相关排序转向事件—变量联合反事实、动态 SCM 和保持分布合理性的修复干预。

#### 6.2.1 多模态事件—变量联合反事实

- A Causal Inference-based Root Cause Analysis Framework Using Multi-modal Data in Large-complex System（Reliability Engineering & System Safety, 2026）[[PDF](https://www.sciencedirect.com/science/article/pii/S0951832025007203)]：CEI-LSBN 融合日志事件与表格时序，通过反事实推理和因果追踪同时定位根因事件、触发变量及其具体取值。

#### 6.2.2 非线性动态 SCM＋近似 Shapley

- Counterfactual-based Root Cause Analysis for Dynamical Systems（ECML PKDD, 2024）[[PDF](https://link.springer.com/chapter/10.1007/978-3-031-70365-2_18)]：以残差神经网络拟合非线性动态结构因果模型，对子系统结构方程和外生扰动实施“恢复正常”干预，并以近似 Shapley 排序导致异常轨迹的子系统与时间点。

#### 6.2.3 分布内修复干预＋最小多根因集合

- Robust Root Cause Diagnosis using In-Distribution Interventions（ICLR, 2025）[[PDF](https://arxiv.org/abs/2505.00930)] [[CODE](https://github.com/nlokeshiisc/IDI_release)]：IDI 先筛选“节点异常但父节点正常”的候选，再从历史正常分布采样下游外生变量实施修复干预，以联合干预和 Shapley 完成多根因最小集合归因。

### 6.3 受控注入与赛博—物理归因

#### 6.3.1 5G 协议模糊注入＋状态转移因果分析

- RAFT: A Real-Time Framework for Root Cause Analysis in 5G and Beyond Vulnerability Detection（CCNC, 2024）[[PDF](https://ieeexplore.ieee.org/document/10454824)]：对 5G RRC 认证/授权流程执行控制指令模糊注入，从通信日志片段学习系统状态及转移，并用因果分析实时定位攻击或异常输入的类型与位置。

#### 6.3.2 物理环境与 ADS 配置协同变异

- ROCAS: Root Cause Analysis of Autonomous Driving Accidents via Cyber-Physical Co-mutation（ASE, 2024）[[PDF](https://dl.acm.org/doi/10.1145/3691620.3695530)]：在仿真中搜索可消除事故的最小环境实体修改，结合执行差分定位最早偏离模块，再以赛博变异寻找保持事故前轨迹时可消除事故的最小 ADS 配置修改。

#### 6.3.3 理想模块替换＋反事实 Delta 调试

- Towards Automated Driving Violation Cause Analysis in Scenario-Based Testing for Autonomous Driving Systems（CoRR, 2024）[[PDF](https://arxiv.org/abs/2401.10443)]：DVCA 以仿真真值构造感知、预测、定位和控制的理想替代模块，通过反事实 Delta 测试做组件级归因，再以状态匹配和二分搜索定位违规诱导消息。

---

## 7. 知识增强、LLM 与智能体推理（Knowledge, LLM and Agent-based RCA）

- **方法共性**：共同诊断链路是“收集/检索证据 → 组织上下文或知识 → 规划推理/调用工具 → 输出根因与解释”。区别在是否微调、是否主动观测、是否有图约束和多智能体协作。

### 7.1 事故文本生成与历史知识建模

#### 7.1.1 事故文本到根因/缓解步骤的生成式微调

- Recommending Root-Cause and Mitigation Steps for Cloud Incidents using Large Language Models（ICSE, 2023）[[PDF](https://arxiv.org/abs/2301.03797)]：基于微软四万余起云事故，对 GPT-3.x 进行零样本、单任务和多任务微调，使模型根据事故文本直接生成根因及缓解步骤。

#### 7.1.2 事故复盘抽取＋因果知识图谱

- Mining Root Cause Knowledge from Cloud Service Incident Investigations for AIOps（ICSE-SEIP, 2022）[[PDF](https://ieeexplore.ieee.org/document/9793994)]：用 BERT、RoBERTa 和 SpanBERT 从复盘文档抽取症状、根因与处置，构建因果知识图谱，并结合 FAISS 历史检索和 GCN 链路预测推荐根因。

### 7.2 直接推理与领域专用模型

#### 7.2.1 遥测预处理＋固定跨模态提示

- MicroRCA-Agent: Microservice Root Cause Analysis Method Based on Large Language Model Agents（CoRR, 2025）[[PDF](https://arxiv.org/abs/2509.15635)] [[CODE](https://github.com/tangpan360/MicroRCA-Agent)]：先以 Drain/多级过滤处理日志、Isolation Forest 分析调用链、统计过滤和两阶段 LLM 总结指标，再用固定跨模态提示生成根因组件、原因与推理链。

#### 7.2.2 5G CoT 蒸馏＋GRPO 推理模型

- Reasoning Language Models for Root Cause Analysis in 5G Wireless Networks（CoRR, 2025）[[PDF](https://arxiv.org/pdf/2507.21974)][[CODE](https://github.com/gsma-labs/evals/tree/main/src/evals/telelogs)]：基于面向 5G RCA 的合成/策展 TeleLogs 基准，用多智能体生成结构化推理轨迹，先对 Qwen2.5 监督微调再以 GRPO 提升根因分类和解释能力。

### 7.3 检索、知识库与置信校准

#### 7.3.1 历史案例检索＋根因置信度校准

- LM-PACE: Confidence Estimation by Large Language Models for Effective Root Causing of Cloud Incidents（FSE Companion, Industry Track, 2024）[[PDF](https://arxiv.org/pdf/2309.05833)]：检索历史事故作为参考，以提示、样本增强和两阶段信息充分度/预测可靠性评估校准根因置信度，使低置信结果可转交人工复核。

#### 7.3.2 遥测事件处理器＋相似事故 ICL

- Automatic Root Cause Analysis via Large Language Models for Cloud Incidents（EuroSys, 2024）[[PDF](https://arxiv.org/abs/2305.15778)]：通过预定义事件处理器采集日志、指标和调用链，检索相似历史事故作为上下文，再由 LLM 预测根因类别并生成解释。

#### 7.3.3 通信工单到结构化根因规则

- LLM-Augmented Knowledge Base Construction for Root Cause Analysis（IEEE Access, 2026）[[PDF](https://ieeexplore.ieee.org/document/11366685)]：TelcoInsight 从通信网络支持工单抽取并生成结构化根因关联规则，对比 LLM 微调、RAG 及两者融合方案以自动构建 RCA 知识库。

#### 7.3.4 历史日志知识图谱＋在线检索推理

- AetherLog: Log-based Root Cause Analysis by Integrating Large Language Models with Knowledge Graphs（ISSRE, 2025）[[PDF](https://ieeexplore.ieee.org/document/11229645)] [[CODE](https://github.com/ISSRE25-Submission-56/AetherLog)]：用 LLM 从历史故障日志抽取实体和关系并构建故障知识图谱，在线阶段对待诊断日志摘要、实体检索和知识增强推理以输出根因。

### 7.4 工具调用与结构约束

#### 7.4.1 ReAct 单智能体＋遥测工具闭环

- RCAgent: Cloud Root Cause Analysis by Autonomous Agents with Tool-Augmented Large Language Models（CIKM, 2024）[[PDF](https://arxiv.org/pdf/2310.16340)]：本地 LLM 在“思考—工具调用—环境观察”循环中主动采集云端诊断数据，并用动作轨迹自一致性、上下文管理和领域知识生成根因、证据与方案。

#### 7.4.2 多模态对齐/定位/分类专用工具

- TAMO: Fine-Grained Root Cause Analysis via Tool-Assisted LLM Agent With Multi-Modality Observation Data in Cloud-Native Systems（IEEE Transactions on Services Computing, 2025）[[PDF](https://ieeexplore.ieee.org/document/11229957)]：以多模态对齐、根因定位和故障类型分类三个工具处理日志、指标与调用链，由专家 Agent 整合工具结果、系统上下文和领域知识生成解释及修复建议。

#### 7.4.3 服务依赖图—程序依赖图跨层遍历

- PRAXIS: Integrating Program Analysis with Observability for Root-Cause Analysis（DSN Research Track, 2026）[[PDF](https://arxiv.org/pdf/2512.22113)][[CODE](https://doi.org/10.5281/zenodo.19163486)]：结合可观测性与静态程序分析工具，由 LLM 充当遍历策略，在服务依赖图和程序依赖图之间逐层搜索，定位微服务、代码路径或配置项；正式版本获 DSN 2026 Best Paper。

### 7.5 多智能体递归推理

#### 7.5.1 沿 Trace 图并行的 Recursion-of-Thought

- Towards In-Depth Root Cause Localization for Microservices with Multi-Agent Recursion-of-Thought（IEEE Transactions on Dependable and Secure Computing, 2026）[[PDF](https://arxiv.org/pdf/2605.14866)] [[CODE](https://github.com/LLMLog/RCLAgent)]：RCLAgent 按 Trace 图为 Span 分配专用 Agent，并沿拓扑递归、并行深入证据，最后融合根级诊断报告和全局证据图，以缓解上下文爆炸和串行推理低效。

#### 7.5.2 骨架因果图 + 记忆力增强多智能体
- KRCA: An Efficient Root Cause Analysis System in Hyper-Scale Microservice Systems via Agentic AI（ASE Industry Track，2026）[[PDF](https://nkcs.iops.ai/wp-content/uploads/2026/07/KRCA_ACM_ASE26.pdf)]: 面向超大规模动态微服务系统，先沿 API 调用依赖结合失败率与延迟执行递归下钻，缩小可疑服务范围；再将异常指标实例化为高召回骨架因果图，并通过带分层记忆和历史案例检索的多智能体协作验证因果关系，最终输出根因服务、故障类型及诊断证据。

### 7.6 代码知识、基础模型与专家协作

#### 7.6.1 代码知识增强的生成式 RCA

- COCA: Generative Root Cause Analysis for Distributed Systems with Code Knowledge（ICSE, 2025）[[PDF](https://www.cse.cuhk.edu.hk/lyu/_media/conference/yli_icse2025_coca.pdf)]：从代码仓库提取依赖和实现知识，补充信息不完整的 issue 报告，同时生成根因位置和自然语言总结。

#### 7.6.2 结构化深度思考基础模型

- FoundRoot: Towards Foundation Model for Root Cause Analysis via Structured Deep Thinking（ICSE, 2026）[[PDF](https://netman.aiops.org/wp-content/uploads/2026/01/foundroot_camera_ready.pdf)][[CODE](https://github.com/NetManAIOps/FoundRoot)]：将指标语义、统计特征和因果结构组织为结构化推理过程，训练面向跨系统 RCA 的专用基础模型以提升未见系统上的泛化能力。

#### 7.6.3 静态分析约束的错误传播路径重建

- ErrorPrism: Reconstructing Error Propagation Paths in Cloud Service Systems（ASE Industry Showcase, 2025）[[PDF](https://zbchern.github.io/papers/ase25a.pdf)]：先用静态分析构造函数调用图并把日志字符串映射到候选函数，再由 LLM Agent 反向搜索完整多跳错误传播路径。

#### 7.6.4 记忆增强递归推理

- Agentic Memory Enhanced Recursive Reasoning for Root Cause Localization in Microservices（ICSE-SEIP, 2026；AMER-RCL）[[PDF](https://arxiv.org/pdf/2601.02732)]：以多智能体递归扩展候选原因，并在告警时间窗内积累和复用 Agentic Memory，减少跨告警重复探索和推理时延。

#### 7.6.5 假设—验证并行搜索

- Hypothesize-Then-Verify: Speculative Root Cause Analysis for Microservices with Pathwise Parallelism（ICSE-NIER, 2026；SpecRCA）[[PDF](https://arxiv.org/pdf/2601.02736)]：先由轻量模块快速起草多个根因假设，再沿不同路径并行验证，以提高 LLM RCA 的探索多样性和推理效率。

#### 7.6.6 人在回路的批量云故障定位

- Aloha: Localizing Batch Failures in Large-scale Cloud Systems via Contrast Analysis and Human-in-the-Loop Agent（FSE Industry, 2026）[[PDF](https://nkcs.iops.ai/wp-content/uploads/2026/04/Yujia__Aloha_to_FSE_26.pdf)]：面向同一根因同时影响大量实例的批量故障，以对比分析和人在回路 Agent 贯通数据处理、异常定位与可解释根因模式总结。

#### 7.6.7 Azure 事故根因标签体系

- AutoARTS: Taxonomy, Insights and Tools for Root Cause Labelling of Incidents in Microsoft Azure（USENIX ATC, 2023）[[PDF](https://www.usenix.org/system/files/atc23-dogga.pdf)]：分析数年、两千余个 Azure 事故建立层次化贡献因素分类体系，并自动辅助事故复盘中的多标签根因标注。

---
## 8. 其他垂直行业与关键基础设施中的根因分析应用（Cross-Domain RCA Applications）

- **方法共性**：这些工作把根因分析从软件服务运维扩展到制造过程、电力系统、建筑设施、核系统、数据库、安全分析和轨道交通等领域，核心链路通常是“领域多源观测 → 异常或因果结构建模 → 根因实体／变量／代码位置排序 → 领域记录或干预结果验证”。

### 8.1 智能制造与流程工业

#### 8.1.1 多层预测图与根因贡献量化

- MPGE and RootRank: A Sufficient Root Cause Characterization and Quantification Framework for Industrial Process Faults（Neural Networks, 2023）[[PDF](https://www.sciencedirect.com/science/article/pii/S0893608023000424)] [[CODE](https://github.com/chunhuiz/MPGE-RootRank-for-root-cause-diagnosis)]：构建稀疏多层预测图，通过层次邻接剪枝刻画直接与间接 Granger 因果关系，并以 RootRank 将多级故障传播结构转化为根因变量贡献分数。

#### 8.1.2 非线性动态 Granger 因果诊断

- Nonlinear Dynamic Granger Causality Analysis Framework for Root-Cause Diagnosis of Quality-Related Faults in Manufacturing Processes（IEEE Transactions on Automation Science and Engineering, 2024）[[PDF](https://ieeexplore.ieee.org/document/10144440)]：以非线性动态 Granger 因果模型联合刻画过程变量和质量变量的时序作用，重建质量相关故障传播关系并定位根因变量。

#### 8.1.3 工厂级分布式直接因果分析

- Hierarchical Fault Root Cause Identification in Plant-Wide Processes Using Distributed Direct Causality Analysis（IEEE Transactions on Industrial Informatics, 2024）[[PDF](https://ieeexplore.ieee.org/document/10226535)]：先以分布式过程监测定位故障单元，再通过部分交叉映射区分直接和间接因果关系，在“单元—变量”两层逐级定位根因。

#### 8.1.4 工业知识图谱与数据联合推理

- Root-KGD: A Novel Framework for Industrial Fault Root Cause Diagnosis Based on Knowledge Graph and Data（IEEE Sensors Journal, 2026）[[PDF](https://arxiv.org/pdf/2406.13664)]：以工业知识图谱表示变量、设备和流股关系，将数据驱动的故障贡献写入实体属性，再通过故障传播推理统一排序根因变量、设备和流股。

### 8.2 电力、建筑与核能基础设施

#### 8.2.1 配电停电事件的数据融合与模式挖掘

- ORCA: Outage Root Cause Analysis in DER-Rich Power Distribution System Using Data Fusion, Hierarchical Clustering and FP-Growth Rule Mining（IEEE Transactions on Smart Grid, 2024）[[PDF](https://doi.org/10.1109/TSG.2023.3281489)]：融合 D-PMU、计量和继电保护数据，通过集成扩展卡尔曼滤波、层次聚类与 FP-Growth 依次完成停电事件识别，并细化判定植被、天气、设备、保护和计划作业等原因。

#### 8.2.2 建筑 HVAC 跨层故障的熵因果学习

- An Entropy-Based Causality Framework for Cross-Level Faults Diagnosis and Isolation in Building HVAC Systems（Energy and Buildings, 2024）[[PDF](https://www.sciencedirect.com/science/article/pii/S0378778824004948)]：提出 Eigen-Entropy Causal Learning，从跨设备症状同步变化中自动学习贝叶斯网络，在不依赖人工因果拓扑的条件下隔离 HVAC 跨层故障根因。

#### 8.2.3 核系统物理约束因果结构学习

- Physics-Constrained Causal Structure Learning for Root Cause Analysis in Nuclear System Monitoring（Reliability Engineering & System Safety, 2026）[[PDF](https://www.sciencedirect.com/science/article/pii/S0951832026005077)]：从简化核系统物理模型提取方向约束并嵌入评分式因果结构搜索，通过排除物理上不可能的边提高小样本事故监测中的根节点识别与传播解释能力。

### 8.3 数据库与数据平台服务

#### 8.3.1 文档知识、工具检索与多大模型协作诊断

- D-Bot: Database Diagnosis System Using Large Language Models（VLDB, 2024）[[PDF](https://arxiv.org/pdf/2312.01454)] [[CODE](https://github.com/TsinghuaDatabaseGroup/DB-GPT)]：从数据库诊断文档离线抽取知识，自动检索知识与工具并生成提示，再通过树搜索和多大模型协作诊断单根因及多根因异常并生成处置报告。

#### 8.3.2 慢查询多模态根因影响排序

- RCRank: Multimodal Ranking of Root Causes of Slow Queries in Cloud Database Systems（VLDB, 2025）[[PDF](https://arxiv.org/pdf/2503.04252)] [[CODE](https://github.com/decisionintelligence/RCRank)]：将 SQL、执行计划、执行日志和 KPI 进行自监督跨模态预训练，通过根因自适应 Cross-Transformer 识别慢查询原因，并按潜在加速收益进行影响感知排序。

### 8.4 软件安全与嵌入式固件

#### 8.4.1 反例奖励强化学习与漏洞根因定位

- Racing on the Negative Force: Efficient Vulnerability Root-Cause Analysis Through Reinforcement Learning on Counterexamples（USENIX Security, 2024）[[PDF](https://www.usenix.org/system/files/usenixsecurity24-xu-dandan.pdf)] [[CODE](https://github.com/0xdd96/Racing-code)]：将会显著改变程序元素—崩溃相关性估计的反例作为强化学习奖励，引导模糊变异快速区分真正致因的代码元素并加速漏洞根因定位。

#### 8.4.2 ARM 固件事件足迹与逆向数据传播

- FirmRCA: Towards Post-Fuzzing Analysis on ARM Embedded Firmware with Efficient Event-Based Fault Localization（IEEE Symposium on Security and Privacy, 2025）[[PDF](https://arxiv.org/pdf/2410.18483)] [[CODE](https://github.com/NESA-Lab/FirmRCA)]：通过事件化内存访问足迹和历史驱动逆向执行处理固件内存别名，再以数据传播跟踪和可疑度策略排序导致崩溃的 ARM 指令。

### 8.5 轨道交通运营与维护

#### 8.5.1 车队运行失稳检测与轮轨根因聚类

- iVRIDA-Fleet: Unsupervised Rail Vehicle Running Instability Detection Algorithm for Passenger Vehicle Fleet（Vehicle System Dynamics, 2025）[[PDF](https://www.tandfonline.com/doi/pdf/10.1080/00423114.2024.2335267)]：以 PCA、稀疏自编码器或 LSTM 编码器—解码器检测车队运行失稳，并在潜空间聚类根因模式，再结合车辆和轨道维护记录验证磨耗车轮、钢轨轮廓及轨距等轮轨因素。

# 9.IEEE Transactions on Services Computing 中的知识图谱相关论文

## 9.1、知识图谱构建、查询、融合与演化

### 9.11. 企业知识图谱构建与查询

- **Building and Querying an Enterprise Knowledge Graph**（IEEE TSC, 2019, 12(3): 356–369）[[DOI](https://doi.org/10.1109/TSC.2017.2711600)]：面向企业内部异构数据设计知识图谱构建、实体链接、查询和维护流程，并将能力封装为一组知识图谱服务

### 9.1.2. 编程知识图谱融合

- **1+1>2: Programming Know-What and Know-How Knowledge Fusion, Semantic Enrichment and Coherent Application**（IEEE TSC, 2023, 16(3): 1540–1554）[[DOI](https://doi.org/10.1109/TSC.2022.3207273)]：融合描述 API“是什么”的 API-KG 与描述编程任务“怎么做”的 Task-KG，并补全跨图语义关系以支持 API 与任务检索

### 9.1.3. 区块链支持的知识图谱协同演化

- **Decentralised Knowledge Graph Evolution Via Blockchain**（IEEE TSC, 2024, 17(1): 169–182）[[DOI](https://doi.org/10.1109/TSC.2023.3337873)]：利用区块链记录分布式参与者对知识图谱的新增、修改和验证操作，实现可追溯、去中心化的知识图谱演化

---

## 9.2、知识图谱增强的推荐与对话推理

### 9.2.1. 时序服务知识图谱推荐

- **Temporal Knowledge Graph Embedding for Effective Service Recommendation**（IEEE TSC, 2022, 15(5): 3077–3088）[[DOI](https://doi.org/10.1109/TSC.2021.3075053)]：构建包含用户—服务交互时间信息的时序服务知识图谱，经图补全和卷积嵌入学习实现时间感知的服务推荐

### 9.2.2. 知识图谱推理增强的对话推荐

- **Hierarchical Reinforcement Learning for Conversational Recommendation With Knowledge Graph Reasoning and Heterogeneous Questions**（IEEE TSC, 2023, 16(5): 3439–3452）[[DOI](https://doi.org/10.1109/TSC.2023.3269396)]：在知识图谱上联合执行多跳路径推理与层次强化学习，使对话推荐系统能够提出不同类型的问题并逐步缩小候选项目

### 9.2.3. 知识图谱增强的掩码自编码推荐

- **Knowledge Graph-Enhanced Masked Auto-Encoders for Recommendation Systems**（IEEE TSC, 2025, 18(6): 3946–3958）[[DOI](https://doi.org/10.1109/TSC.2025.3632319)]：将知识图谱结构注入掩码自编码器，通过重构缺失交互与知识关系提升稀疏和噪声环境下的推荐鲁棒性

### 9.2.4. 元宇宙服务知识图谱去噪

- **MSKD: A Knowledge Denoising Framework for Metaverse Service Recommendation**（IEEE TSC, 2025, 18(6): 4030–4042）[[DOI](https://doi.org/10.1109/TSC.2025.3625048)]：融合外部服务知识和交互数据构建用户偏好知识图谱，通过自适应剪枝、知识图谱瓶颈与对比学习消除无关实体和关系

---

## 9.3、知识图谱驱动的 Service 管理、发现与故障诊断

### 9.3.1. 云服务 SLA 知识图谱

- **A Semantically Rich Framework to Automate Cloud Service Level Agreements**（IEEE TSC, 2023, 16(1): 53–64）[[DOI](https://doi.org/10.1109/TSC.2022.3140585)]：从云服务 SLA 文档中抽取服务指标、约束和义务规则并组织为语义知识图谱，以自动比较供应商并辅助云服务选择

### 9.3.2. 网络保险服务语义知识图谱

- **Semantically Rich Framework to Automate Cyber Insurance Services**（IEEE TSC, 2023, 16(1): 588–599）[[DOI](https://doi.org/10.1109/TSC.2021.3113272)]：结合语义网、本体和模态逻辑从网络保险条款中抽取保障、排除项与规则，形成机器可处理的策略知识表示并支持保险服务匹配

### 9.3.3. 日志知识图谱故障诊断

- **LogKG: Log Failure Diagnosis Through Knowledge Graph**（IEEE TSC, 2023, 16(5): 3493–3507）[[DOI](https://doi.org/10.1109/TSC.2023.3293890)]：把日志模板、变量、组件、故障及处置关系构造成日志知识图谱，通过历史故障聚类和图推理定位故障根因

### 9.3.4. 微服务时序知识图谱统一诊断

- **No More Data Silos: Unified Microservice Failure Diagnosis With Temporal Knowledge Graph**（IEEE TSC, 2024, 17(6): 4013–4026）[[DOI](https://doi.org/10.1109/TSC.2024.3489444)] [[CODE](https://github.com/AIOps-Lab-NKU/UniDiag)]：用时序知识图谱统一日志、指标和调用链，并通过动态图表示同时完成故障检测、根因定位和故障类型识别

### 9.3.5. 面向服务链接预测的模式知识嵌入

- **Learning Schema Embeddings for Service Link Prediction: A Coupled Matrix-Tensor Factorization Approach**（IEEE TSC, 2025, 18(2): 883–896）[[DOI](https://doi.org/10.1109/TSC.2025.3541552)] [[CODE](https://github.com/twEErwdf/SchemaE)]：联合建模知识图谱实体—关系三元组和实体类型模式，通过耦合矩阵—张量分解提升服务链接预测、发现、推荐和组合能力

### 9.3.6. 星地协同知识服务框架

- **An Efficient and Stable Knowledge Service Framework for Satellite-Ground Collaboration**（IEEE TSC, 2025, 18(6): 3449–3462）[[DOI](https://doi.org/10.1109/TSC.2025.3607909)]：由星上轻量模型处理实时观测，地面知识图谱提供知识补充、异常原因分析和图谱推理，并结合链路预测与路由保证知识服务稳定交付

---

## 9.4、知识图谱在安全与行业数据分析中的应用

### 9.4.1. 模糊二部知识图谱医疗欺诈识别

- **Identification of Fraudulent Healthcare Claims Using Fuzzy Bipartite Knowledge Graphs**（IEEE TSC, 2023, 16(6): 3931–3945）[[DOI](https://doi.org/10.1109/TSC.2023.3296782)]：以诊断、诊疗过程和医疗服务提供者构建模糊二部知识图谱，利用关系异常识别可疑医疗索赔

### 9.4.2. 软件漏洞知识图谱

- **KG4VA: Constructing Vulnerability Knowledge Graph for Software Vulnerability Assessment**（IEEE TSC, 2025, 18(6): 3932–3945）[[DOI](https://doi.org/10.1109/TSC.2025.3607682)]：融合软件、漏洞、弱点、攻击方式和安全属性构建漏洞知识图谱，以结构化关联增强软件漏洞评估
---

## 9.5、与 Service 直接相关的知识图谱论文

| Service 研究方向 | 论文 | 知识图谱作用 |
| --- | --- | --- |
| 服务推荐 | Temporal Knowledge Graph Embedding for Effective Service Recommendation | 构建时序服务知识图谱并学习服务嵌入 |
| 云服务管理 | A Semantically Rich Framework to Automate Cloud Service Level Agreements | 将 SLA 条款、指标和义务组织为知识图谱 |
| 网络保险服务 | Semantically Rich Framework to Automate Cyber Insurance Services | 将保单保障、排除项和规则语义化 |
| 服务故障诊断 | LogKG | 组织日志、组件、故障和解决方案关系 |
| 微服务统一诊断 | No More Data Silos / UniDiag | 统一日志、指标、调用链及其时间关系 |
| 服务发现／组合 | Learning Schema Embeddings for Service Link Prediction | 通过模式约束知识嵌入预测服务链接 |
| 星地知识服务 | An Efficient and Stable Knowledge Service Framework | 地面知识图谱辅助星上推理和原因分析 |
| 元宇宙服务推荐 | MSKD | 构建并去噪用户偏好—服务知识图谱 |


## 10. 代表论文 → Related Work → 研究分支


### 10.1 代表论文一：Nezha——以单模态三分法汇聚到多模态细粒度 RCA

**代表论文：**

- Nezha: Interpretable Fine-Grained Root Causes Analysis for Microservices on Multi-modal Observability Data（ESEC/FSE, 2023）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3611643.3616249)][[CODE](https://github.com/IntelligentDDS/Nezha)]：将指标、日志和 Trace 统一成事件，比较正常期与故障期事件图模式，定位到服务、代码区域或资源类型。

**Nezha Related Work 中的指标类工作：**

- CauseInfer: Automated End-to-End Performance Diagnosis with Hierarchical Causality Graph in Cloud Environment（IEEE Transactions on Services Computing, 2019）[[PDF](https://ieeexplore.ieee.org/document/7563819)]：从服务间时滞依赖和服务内指标因果关系构造两层图，沿异常路径定位故障服务及根因指标。
- MicroRCA: Root Cause Localization of Performance Issues in Microservices（NOMS, 2020）[[PDF](https://ieeexplore.ieee.org/document/9110353)][[CODE](https://github.com/elastisys/MicroRCA)]：从服务依赖、延迟和主机资源构造属性图，在异常子图上用个性化 PageRank 排序根因。
- Groot: An Event-graph-based Approach for Root Cause Analysis in Industrial Settings（ASE, 2021）[[PDF](https://ieeexplore.ieee.org/document/9678708)]：将指标异常、状态日志和变更活动抽象成事件，以领域规则构造事件因果图并排序根因。

**Nezha Related Work 中的日志类工作：**

- Fast Dimensional Analysis for Root Cause Investigation in a Large-Scale Service Environment（Proceedings of the ACM on Measurement and Analysis of Computing Systems, 2020）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3392149)]：比较正常期与异常期的高频属性组合，寻找最能解释事故的维度和值。
- Spectrum-Based Log Diagnosis（ESEM, 2020；SBLD）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3382494.3410684)]：把日志事件视为程序元素、执行阶段视为测试，使用频谱故障定位公式排序可疑日志模板。

**Nezha Related Work 中的 Trace 类工作：**

- Graph-Based Trace Analysis for Microservice Architecture Understanding and Problem Diagnosis（ESEC/FSE, 2020；GMTA）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3368089.3417066)]：将 Trace 聚合为调用路径和业务流图，支持架构理解与问题诊断。
- T-Rank: A Lightweight Spectrum Based Fault Localization Approach for Microservice Systems（CCGRID, 2021）[[PDF](https://doi.org/10.1109/CCGrid51090.2021.00051)]：根据服务在正常和异常 Trace 中的覆盖差异计算频谱可疑度。
- MicroRank: End-to-End Latency Issue Localization with Extended Spectrum Analysis in Microservice Environments（The Web Conference, 2021）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3442381.3449905)]：将正常/异常请求频谱与调用图 PageRank 结合，区分覆盖相近的根因服务或操作。


### 10.2 代表论文二：Eadro——从单源定位推进到多源、端到端联合学习

**代表论文：**

- Eadro: An End-to-End Troubleshooting Framework for Microservices on Multi-source Data（ICSE, 2023）[[PDF](https://arxiv.org/pdf/2302.05092)]：分别编码日志、指标和 Trace，以图注意力建模服务依赖，并通过多任务学习联合异常检测与根因定位。其 Related Work 指出以往工作大多依赖 Trace，且通常把异常检测和根因定位割裂处理。

**Eadro Related Work 中直接讨论的定位工作：**

- CloudRanger: Root Cause Identification for Cloud Native Systems（CCGRID, 2018）[[PDF](https://ieeexplore.ieee.org/document/8411065)]：从 KPI 学习因果图，并通过二阶随机游走回溯异常传播源。
- Localizing Failure Root Causes in a Microservice through Causality Inference（IWQoS, 2020；MicroCause）[[PDF](https://nkcs.iops.ai/wp-content/uploads/2020/07/paper-IWQOS2020-MicroCause.pdf)]：在单个服务内部学习时序指标因果关系，结合异常传播先后顺序定位细粒度根因。
- AutoMAP: Diagnose Your Microservice-Based Web Applications Automatically（The Web Conference, 2020）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3366423.3380111)]：动态恢复服务依赖，结合异常相关性和传播方向定位根因服务。
- MicroHECL: High-Efficient Root Cause Localization in Large-Scale Microservice Systems（ICSE-SEIP, 2021）[[PDF](https://doi.org/10.1109/ICSE-SEIP52600.2021.00043)]：动态构造调用图，按照性能、可靠性和流量异常搜索并剪枝故障传播链。
- Practical Root Cause Localization for Microservice Systems via Trace Analysis（IWQoS, 2021；TraceRCA）[[PDF](https://netman.aiops.org/wp-content/uploads/2021/05/1570705191.pdf)][[CODE](https://github.com/NetManAIOps/TraceRCA)]：从异常 Trace 挖掘候选服务集合，再利用正常/异常覆盖差异进行无监督排序。
- MicroRank: End-to-End Latency Issue Localization with Extended Spectrum Analysis in Microservice Environments（The Web Conference, 2021）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3442381.3449905)]：以 Trace 为主要输入，将请求频谱和服务调用关系结合起来定位延迟根因。


### 10.3 代表论文三：BARO——将指标 RCA 细分为统计、拓扑图和因果图

**代表论文：**

- BARO: Robust Root Cause Analysis for Microservices via Multivariate Bayesian Online Change Point Detection（FSE, 2024）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3660805)][[CODE](https://github.com/phamquiluan/baro)]：用多变量贝叶斯在线变点检测确定故障窗口，再以非参数 RobustScorer 排序根因指标。其第 2.4 节明确将指标 RCA 分成 Statistical Analysis、Topology Graph-based Analysis 和 Causal Graph-based Analysis。

**BARO Related Work 中的统计分析路线：**

- ε-Diagnosis: Unsupervised and Real-Time Diagnosis of Small-Window Long-Tail Latency in Large-Scale Microservice Platforms（The Web Conference, 2019）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3308558.3313653)]：通过双样本检验比较正常与异常小窗口，以 ε-statistics 排序变化最显著的根因维度。
- Scalable Statistical Root Cause Analysis on App Telemetry（ICSE-SEIP, 2021）[[PDF](https://doi.org/10.1109/ICSE-SEIP52600.2021.00038)]：从大规模事故遥测中挖掘差异模式，以统计精确率和召回率筛选回归根因。

**BARO Related Work 中的拓扑图路线：**

- MicroRCA: Root Cause Localization of Performance Issues in Microservices（NOMS, 2020）[[PDF](https://ieeexplore.ieee.org/document/9110353)][[CODE](https://github.com/elastisys/MicroRCA)]：在监控数据恢复出的服务—主机属性图上抽取异常子图并随机游走。
- Sieve: Actionable Insights from Monitored Metrics in Distributed Systems（Middleware, 2017）[[PDF](https://ljiao.github.io/papers/middleware17.pdf)][[CODE](https://github.com/sieve-microservices)]：在已知系统拓扑上聚类降维指标，再用 Granger 因果关系筛选可操作的根因指标。
- Causal Inference-Based Root Cause Analysis for Online Service Systems with Intervention Recognition（KDD, 2022；CIRCA）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3534678.3539041)][[CODE](https://github.com/NetManAIOps/CIRCA)]：使用系统架构约束结构图，并通过回归式假设检验识别条件机制发生变化的节点。

**BARO Related Work 中的因果图路线：**

- CauseInfer: Automated End-to-End Performance Diagnosis with Hierarchical Causality Graph in Cloud Environment（IEEE Transactions on Services Computing, 2019）[[PDF](https://ieeexplore.ieee.org/document/7563819)]：构造服务间和服务内两层因果图并沿异常路径搜索。
- Localizing Failure Root Causes in a Microservice through Causality Inference（IWQoS, 2020；MicroCause）[[PDF](https://nkcs.iops.ai/wp-content/uploads/2020/07/paper-IWQOS2020-MicroCause.pdf)]：利用时序因果图和异常先后关系定位服务内部根因。
- Root Cause Analysis of Failures in Microservices through Causal Discovery（NeurIPS, 2022；RCD）[[PDF](https://dl.acm.org/doi/pdf/10.5555/3600270.3602529)][[CODE](https://github.com/azamikram/rcd)]：将故障前后数据视为观测和软干预样本，以故障指示节点和分治式 Ψ-PC 识别干预目标。
- CausalRCA: Causal Inference Based Precise Fine-Grained Root Cause Localization for Microservice Applications（Journal of Systems and Software, 2023）[[PDF](https://www.sciencedirect.com/science/article/pii/S0164121223001193)][[CODE](https://github.com/AXinx/CausalRCA_code)]：以 DAG-GNN 学习非线性因果结构，再使用 PageRank 输出根因指标。
- CMDiagnostor: An Ambiguity-Aware Root Cause Localization Approach Based on Call Metric Data（The Web Conference, 2023）[[PDF](https://nkcs.iops.ai/wp-content/uploads/2023/02/WWW23-CMDiagnostor.pdf)][[CODE](https://github.com/NetManAIOps/CMDiagnostor)]：通过流量回归消除调用指标构图歧义，再执行异常检测、传播链剪枝和候选排序。


### 10.4 代表论文四：因果 RCA 审计——从提出新方法转向验证发现器与排序器

**代表论文：**

- Root Cause Analysis for Microservice System based on Causal Inference: How Far Are We?（ASE, 2024）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3691620.3695065)][[CODE](https://github.com/phamquiluan/RCAEval/tree/ase24)]：统一评测 9 种因果发现算法和 21 种“发现器—排序器”组合，表明大图、边方向、输入窗口和超参数会显著改变 RCA 结论。

**该论文 Related Work 中直接承接的评测与综述工作：**

- Anomaly Detection and Failure Root Cause Analysis in (Micro) Service-Based Cloud Applications: A Survey（ACM Computing Surveys, 2023）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3501297)]：从异常检测和 RCA 两方面整理服务化云应用研究，但不对不同 RCA 方法开展统一实验评测。
- Evaluation of Causal Inference Techniques for AIOps（CODS-COMAD, 2021）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3430984.3431027)]：在 AIOps 场景中实验比较因果推断技术，为后续微服务因果 RCA 审计提供评测思路。
- Causal Inference Techniques for Microservice Performance Diagnosis: Evaluation and Guiding Recommendations（ACSOS, 2021）[[PDF](https://doi.org/10.1109/ACSOS52086.2021.00029)]：比较多种因果发现方法与 PageRank 组合，给出微服务性能诊断的选择建议。

**该论文重点审计的 RCA 方法：**

- Root Cause Analysis of Failures in Microservices through Causal Discovery（NeurIPS, 2022；RCD）[[PDF](https://dl.acm.org/doi/pdf/10.5555/3600270.3602529)][[CODE](https://github.com/azamikram/rcd)]：代表软干预目标发现路线。
- Causal Inference-Based Root Cause Analysis for Online Service Systems with Intervention Recognition（KDD, 2022；CIRCA）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3534678.3539041)][[CODE](https://github.com/NetManAIOps/CIRCA)]：代表已知结构约束下的机制变化检验路线。
- CausalRCA: Causal Inference Based Precise Fine-Grained Root Cause Localization for Microservice Applications（Journal of Systems and Software, 2023）[[PDF](https://www.sciencedirect.com/science/article/pii/S0164121223001193)][[CODE](https://github.com/AXinx/CausalRCA_code)]：代表连续优化因果结构学习与图排序组合路线。
- BARO: Robust Root Cause Analysis for Microservices via Multivariate Bayesian Online Change Point Detection（FSE, 2024）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3660805)][[CODE](https://github.com/phamquiluan/baro)]：代表不恢复完整因果图、直接从分布突变排序根因的对照路线。


### 10.5 代表论文五：RCACopilot——从传统遥测处理走向 LLM 事故诊断

**代表论文：**

- Automatic Root Cause Analysis via Large Language Models for Cloud Incidents（EuroSys, 2024；RCACopilot）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3627703.3629553)]：根据告警类型调用预定义事故处理器收集指标、日志和 Trace，再由 LLM 预测根因类别并生成解释。其第 7 节 Related Work 同时回顾传统多源 RCA 和 LLM 软件工程应用。

**RCACopilot Related Work 中与 RCA 直接相关的工作：**

- Recommending Root-Cause and Mitigation Steps for Cloud Incidents using Large Language Models（ICSE, 2023）[[PDF](https://www.microsoft.com/en-us/research/uploads/prod/2023/02/ICSE2023_LLM4IncidentManagement.pdf)]：基于四万余起真实云事故微调 GPT-3.x，根据事故标题和摘要生成根因及缓解步骤。
- Scalable Statistical Root Cause Analysis on App Telemetry（ICSE-SEIP, 2021）[[PDF](https://doi.org/10.1109/ICSE-SEIP52600.2021.00038)]：代表 RCACopilot 回顾的指标故障模式挖掘路线。
- Practical Root Cause Localization for Microservice Systems via Trace Analysis（IWQoS, 2021；TraceRCA）[[PDF](https://netman.aiops.org/wp-content/uploads/2021/05/1570705191.pdf)][[CODE](https://github.com/NetManAIOps/TraceRCA)]：代表其回顾的 Trace 故障服务定位路线。


### 10.6 代表论文六：传播感知基准——用新基准回看全部技术路线

**代表论文：**

- Rethinking the Evaluation of Microservice RCA with a Fault Propagation-Aware Benchmark（FSE, 2026）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3797100)][[CODE](https://zenodo.org/records/19494726)]：构建包含复杂故障传播、动态负载、可观测盲区和分层真值的新基准，重新实现并评测 11 种 RCA 方法。其 Related Work 先区分指标、日志、Trace 单模态方法和多模态方法，再把 RCAEval 视为直接前序评测工作。

**该论文 Related Work 中的单模态工作：**

- Root Cause Analysis of Failures in Microservices through Causal Discovery（NeurIPS, 2022；RCD）[[PDF](https://dl.acm.org/doi/pdf/10.5555/3600270.3602529)][[CODE](https://github.com/azamikram/rcd)]：指标因果发现路线代表。
- Causal Inference-Based Root Cause Analysis for Online Service Systems with Intervention Recognition（KDD, 2022；CIRCA）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3534678.3539041)][[CODE](https://github.com/NetManAIOps/CIRCA)]：指标机制变化与干预识别路线代表。
- MicroHECL: High-Efficient Root Cause Localization in Large-Scale Microservice Systems（ICSE-SEIP, 2021）[[PDF](https://doi.org/10.1109/ICSE-SEIP52600.2021.00043)]：调用图异常传播链搜索路线代表。
- BARO: Robust Root Cause Analysis for Microservices via Multivariate Bayesian Online Change Point Detection（FSE, 2024）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3660805)][[CODE](https://github.com/phamquiluan/baro)]：指标分布突变与稳健排序路线代表。
- MicroRank: End-to-End Latency Issue Localization with Extended Spectrum Analysis in Microservice Environments（The Web Conference, 2021）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3442381.3449905)]：Trace 频谱和调用图排序路线代表。

**该论文 Related Work 中的多模态工作：**

- Eadro: An End-to-End Troubleshooting Framework for Microservices on Multi-source Data（ICSE, 2023）[[PDF](https://arxiv.org/pdf/2302.05092)]：代表多模态编码和检测—定位联合学习。
- TrinityRCL: Multi-Granular and Code-Level Root Cause Localization Using Multiple Types of Telemetry Data in Microservice Systems（IEEE Transactions on Software Engineering, 2023）[[PDF](https://ieeexplore.ieee.org/document/10034937)]：代表多源因果图与服务—主机—指标—代码多粒度定位。
- Nezha: Interpretable Fine-Grained Root Causes Analysis for Microservices on Multi-modal Observability Data（ESEC/FSE, 2023）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3611643.3616249)][[CODE](https://github.com/IntelligentDDS/Nezha)]：代表事件统一表示和正常/故障模式对比。
- Robust Failure Diagnosis of Microservice System through Multimodal Data（IEEE Transactions on Services Computing, 2023；DiagFusion）[[PDF](https://nkcs.iops.ai/wp-content/uploads/2025/09/Robust_Failure_Diagnosis_of_Microservice_System_Through_Multimodal_Data.pdf)][[CODE](https://github.com/AIOps-Lab-NKU/DiagFusion)]：代表统一实例表征和图神经网络联合故障分类、根因定位。
- ART: A Unified Unsupervised Framework for Incident Management in Microservice Systems（ASE, 2024）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3691620.3695537)]：代表多模态事件管理的统一无监督框架。
- MRCA: Metric-level Root Cause Analysis for Microservices via Multi-Modal Data（ASE, 2024）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3691620.3695485)]：利用日志和 Trace 缩小服务范围，再扩展指标因果图并输出指标级根因。

**该论文 Related Work 中的直接前序评测：**

- Root Cause Analysis for Microservice System based on Causal Inference: How Far Are We?（ASE, 2024）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3691620.3695065)][[CODE](https://github.com/phamquiluan/RCAEval/tree/ase24)]：在既有数据集上揭示因果发现和排序组合的不稳定性。
- RCAEval: A Benchmark for Root Cause Analysis of Microservice Systems with Telemetry Data（The Web Conference Companion, 2025）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3701716.3715522)][[CODE](https://github.com/phamquiluan/RCAEval)]：统一遥测数据格式和评测协议，为跨系统比较不同 RCA 方法提供基准套件。


---

## 11. 顶尖专家、重要资深合作者与共同课题


### 11.1 张圣林院长：南开 AIOps、清华 NetMan 与产业故障诊断主线


**代表工作：**

- Localizing Failure Root Causes in a Microservice through Causality Inference（IWQoS, 2020；MicroCause）[[PDF](https://nkcs.iops.ai/wp-content/uploads/2020/07/paper-IWQOS2020-MicroCause.pdf)]：以时序因果图和时间因果随机游走定位微服务内部根因指标，是张圣林—裴丹因果诊断路线的早期代表。
- CMDiagnostor: An Ambiguity-Aware Root Cause Localization Approach Based on Call Metric Data（The Web Conference, 2023）[[PDF](https://nkcs.iops.ai/wp-content/uploads/2023/02/WWW23-CMDiagnostor.pdf)][[CODE](https://github.com/NetManAIOps/CMDiagnostor)]：用流量回归消除调用指标构图歧义，再执行异常检测、剪枝和根因排序。
- Robust Failure Diagnosis of Microservice System through Multimodal Data（IEEE Transactions on Services Computing, 2023；DiagFusion）[[PDF](https://nkcs.iops.ai/wp-content/uploads/2025/09/Robust_Failure_Diagnosis_of_Microservice_System_Through_Multimodal_Data.pdf)][[CODE](https://github.com/AIOps-Lab-NKU/DiagFusion)]：融合指标、日志、调用链与依赖图，联合定位根因实例和故障类型。
- Microservice Root Cause Analysis With Limited Observability Through Intervention Recognition in the Latent Space（KDD, 2024；LatentScope）[[PDF](https://nkcs.iops.ai/wp-content/uploads/2024/09/SIGKDD_24_LatentScope.pdf)][[CODE](https://github.com/eBay/LatentScope)]：在有限可观测条件下以潜变量干预识别处理跨服务、Pod 和主机的异构根因。
- No More Data Silos: Unified Microservice Failure Diagnosis With Temporal Knowledge Graph（IEEE Transactions on Services Computing, 2024；UniDiag）[[PDF](https://nkcs.iops.ai/wp-content/uploads/2025/09/No_More_Data_Silos_Unified_Microservice_Failure_Diagnosis_With_Temporal_Knowledge_Graph.pdf)][[CODE](https://github.com/AIOps-Lab-NKU/UniDiag)]：以时序知识图谱统一三类遥测并支持故障检测、定位和分诊。
- FoundRoot: Towards Foundation Model for Root Cause Analysis via Structured Deep Thinking（ICSE, 2026）[[PDF](https://netman.aiops.org/wp-content/uploads/2026/01/foundroot_camera_ready.pdf)][[CODE](https://github.com/NetManAIOps/FoundRoot)]：联合清华与字节研究团队训练结构化深度思考的 RCA 基础模型。


### 11.2 裴丹：AIOps 开放平台、因果推断与多模态诊断

**代表工作：**

- Causal Inference-Based Root Cause Analysis for Online Service Systems with Intervention Recognition（KDD, 2022；CIRCA）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3534678.3539041)][[CODE](https://github.com/NetManAIOps/CIRCA)]：把在线服务根因分析形式化为干预识别，利用系统架构与因果假设构建因果贝叶斯网络。
- OpenRCA: Can Large Language Models Locate the Root Cause of Software Failures?（ICLR, 2025）[[PDF](https://openreview.net/pdf?id=M4qNIzQYpd)][[CODE](https://github.com/microsoft/OpenRCA)]：与微软及何品佳团队构建真实软件故障基准和可调用数据分析工具的 RCA-Agent。
- FoundRoot: Towards Foundation Model for Root Cause Analysis via Structured Deep Thinking（ICSE, 2026）[[PDF](https://netman.aiops.org/wp-content/uploads/2026/01/foundroot_camera_ready.pdf)][[CODE](https://github.com/NetManAIOps/FoundRoot)]：与张圣林、字节资深研究人员共同推进跨系统 RCA 基础模型。


### 11.3 微软 AIOps 负责人群：真实云事故、知识与 Agent

微软[官方 AIOps 项目页](https://www.microsoft.com/en-us/research/project/aiops/groups/)表明，董美负责 Data/Knowledge/Intelligence 方向，林庆维、Saravan Rajmohan、Chetan Bansal 等长期从事云运维可靠性、开发效率和资源效率研究。这一组织关系比单篇作者表更能证明稳定合作。

**代表工作：**

- Recommending Root-Cause and Mitigation Steps for Cloud Incidents using Large Language Models（ICSE, 2023）[[PDF](https://www.microsoft.com/en-us/research/uploads/prod/2023/02/ICSE2023_LLM4IncidentManagement.pdf)]：在四万余起真实云事故上比较零样本、单任务和多任务微调，并由事故负责人评估根因与缓解步骤。
- AutoARTS: Taxonomy, Insights and Tools for Root Cause Labelling of Incidents in Microsoft Azure（USENIX ATC, 2023）[[PDF](https://www.usenix.org/system/files/atc23-dogga.pdf)]：分析数年、两千余个 Azure 事故建立层次化贡献因素分类体系，并自动辅助事故复盘中的多标签根因标注。
- Automatic Root Cause Analysis via Large Language Models for Cloud Incidents（EuroSys, 2024；RCACopilot）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3627703.3629553)]：按告警类型匹配诊断处理器、聚合运行时信息，再由 LLM 预测根因类别并生成解释。
- OpenRCA: Can Large Language Models Locate the Root Cause of Software Failures?（ICLR, 2025）[[PDF](https://openreview.net/pdf?id=M4qNIzQYpd)][[CODE](https://github.com/microsoft/OpenRCA)]：与裴丹、何品佳等合作，把微软真实系统经验转化为开放多模态 RCA 基准。
- Aloha: Localizing Batch Failures in Large-scale Cloud Systems via Contrast Analysis and Human-in-the-Loop Agent（FSE Industry, 2026）[[PDF](https://nkcs.iops.ai/wp-content/uploads/2026/04/Yujia__Aloha_to_FSE_26.pdf)]：与张圣林团队以对比分析和人在回路 Agent 处理具有共同根因的大规模批量云故障。


### 11.4 Hongyu Zhang—Huong Ha—Michael R. Lyu：智能事故管理与韧性在线服务

**代表工作：**

- BARO: Robust Root Cause Analysis for Microservices via Multivariate Bayesian Online Change Point Detection（FSE, 2024）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3660805)][[CODE](https://github.com/phamquiluan/baro)]：用多变量贝叶斯在线变点检测统一故障检测和根因排序，强调对异常窗口偏差和参数的稳健性。
- Root Cause Analysis for Microservice System based on Causal Inference: How Far Are We?（ASE, 2024）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3691620.3695065)][[CODE](https://github.com/phamquiluan/RCAEval/tree/ase24)]：统一评测因果发现算法、排序规则和数据预处理组合。
- TORAI: Multi-Source Root Cause Analysis for Blind Spots in the Microservice Service Call Graph（FSE, 2026）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3808137)][[CODE](https://github.com/phamquiluan/RCAEval/tree/fse26)]：在调用图存在黑盒服务和 Trace 盲区时，以多源异常强度、聚类、因果排序和假设检验完成定位。
- COCA: Generative Root Cause Analysis for Distributed Systems with Code Knowledge（ICSE, 2025）[[PDF](https://www.cse.cuhk.edu.hk/lyu/_media/conference/yli_icse2025_coca.pdf)]：Michael R. Lyu 团队用代码知识增强分布式系统根因定位和自然语言总结。

### 11.5 Ying Li—Gang Huang：北大—阿里多模态诊断与 Agentic RCL

**代表工作：**

- FAMOS: Fault Diagnosis for Microservice Systems through Effective Multi-Modal Data Fusion（ICSE, 2025）[[PDF](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=11029848)]：北大—阿里合作，通过独立模态编码、高斯注意力和交叉注意力完成根因服务定位及故障类型诊断。
- United We Stand: Towards End-to-End Log-based Fault Diagnosis via Interactive Multi-Task Learning（ASE, 2025；Chimera）[[PDF](https://arxiv.org/pdf/2509.24364)]：统一日志异常检测与根因定位，通过双向知识传递减少诊断偏差累积。
- Agentic Memory Enhanced Recursive Reasoning for Root Cause Localization in Microservices（ICSE-SEIP, 2026；AMER-RCL）[[PDF](https://arxiv.org/pdf/2601.02732)]：与阿里、中国电信合作，以记忆增强递归推理复用跨告警诊断经验。
- Hypothesize-Then-Verify: Speculative Root Cause Analysis for Microservices with Pathwise Parallelism（ICSE-NIER, 2026；SpecRCA）[[PDF](https://arxiv.org/pdf/2601.02736)]：与阿里合作，以并行“假设—验证”提高 LLM RCA 的探索多样性和效率。
