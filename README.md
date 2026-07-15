# Root Cause Analysis 文献整理

根因定位方法可以按它在诊断链条中解决的核心问题区分：

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

- Pooled Leaderboards Hide System-Specific Winners: A Reporting-Protocol Audit of Offline Root-Cause Analysis Benchmarks（CoRR, 2026）[[PDF](https://arxiv.org/pdf/2606.29159)] [[CODE](https://anonymous.4open.science/r/rca-leaderboard-audit-artifact-1FC2)]：审计 OpenRCA、RCAEval 和 PetShop 的 11 个子系统、778 个案例，证明汇总 Top-1 排名会掩盖不同系统之间的方法胜负反转。

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

- Trace-based Multi-Dimensional Root Cause Localization of Performance Issues in Microservice Systems（ICSE, 2024）[[PDF](https://ieeexplore.ieee.org/abstract/document/10549362)] [[DATASET](https://zenodo.org/records/12751520)]：TraceContrast 从调用链提取关键路径，把服务、实例、版本和请求属性编码为事件序列，再通过对比序列模式挖掘与频谱分析定位多维根因。

#### 2.2.5 5G 信令轨迹重构与模式评分

- STRCA: A Lightweight and Accurate Root Cause Analysis System Based on 5G Signalling Trace（ICIC, 2024）[[PDF](https://link.springer.com/chapter/10.1007/978-981-97-5672-8_4)]：从海量 5G 核心网信令报文重构端到端信令轨迹，检测异常轨迹后挖掘可疑网元集合，并以专门评分函数定位根因网元。

#### 2.2.6 Trace 症状—指标相关的分层定位

- HeMiRCA: Fine-Grained Root Cause Analysis for Microservices with Heterogeneous Data Sources（ACM Transactions on Software Engineering and Methodology, 2024）[[PDF](https://dl.acm.org/doi/full/10.1145/3674726)] [[CODE](https://github.com/zhouruixingzhu/HeMiRCA)]：从 Span 延迟构造时序异常分数，再用 Spearman 相关性衡量该分数与各服务指标的单调关联，分层定位根因服务和根因指标。

---

## 3. 依赖图传播、随机游走与案例匹配（Dependency Graph and Random Walks）

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

- Automated, Cross-Layer Root Cause Analysis of 5G Video-Conferencing Quality Degradation（ACM Internet Measurement Conference, 2025）[[PDF](https://dl.acm.org/doi/10.1145/3730567.3764434)] [[ARTIFACT/TESTBED](https://github.com/PrincetonUniversity/Domino-IMC)]：Domino 将 5G PHY/MAC/RLC、传输层和 WebRTC 遥测转成跨层事件，并在专家因果图中匹配从底层无线或网络因素传播至视频卡顿、码率下降的因果链。

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

- TrinityRCL: Multi-Granular and Code-Level Root Cause Localization Using Multiple Types of Telemetry Data in Microservice Systems（IEEE Transactions on Software Engineering, 2023）[[PDF](https://ieeexplore.ieee.org/document/10034937)] [[THIRD-PARTY REPRODUCTION](https://github.com/Apex-ISET/TrinityRCL_Reproduction)]：融合指标、日志与调用链构建跨应用、服务、主机、指标和代码实体的因果图，并按异常相关性进行多粒度排序，日志可进一步映射到代码位置。

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

### 5.2 超图、层次图与云边协同

#### 5.2.1 级联条件学习＋异构超图

- Root Cause Analysis for Microservice Systems via Cascaded Conditional Learning with Hypergraphs（CoRR, 2025）[[PDF](https://arxiv.org/abs/2511.17566)]：CCLH 融合指标、日志和调用链，构建含调用、部署及负载均衡关系的异构超图，以 UniGAT-HE 建模群体传播并级联完成根因实例定位和故障类型识别。

#### 5.2.2 内核故障解析＋异构动态拓扑栈

- Root Cause Localization for Microservice Systems in Cloud-edge Collaborative Environments（CoRR, 2024）[[PDF](https://arxiv.org/abs/2406.13604)] [[CODE_1](https://github.com/WDCloudEdge/HybridCloudConfig)] [[CODE_2](https://github.com/WDCloudEdge/MicroCERCL)]：MicroCERCL 先从通信内核日志和网络报文定位断连/丢包等内核根因，再以服务—实例—服务器动态拓扑栈、类型特定图卷积、LSTM 与图注意力池化输出应用级排名。

#### 5.2.3 簇内节点定位＋簇级故障类型级联

- A Cascaded Graph Neural Network for Joint Root Cause Localization and Analysis in Edge Computing Environments（CoRR, 2026）[[PDF](https://arxiv.org/abs/2603.01447)]：先对通信图执行 Louvain 聚类，用 P-Net 在簇内定位根因节点，再由 O-Net 在簇级图识别故障类型，并通过多任务损失联合优化 RCL 与 RCA。

### 5.3 无标签目标系统的跨域适配

#### 5.3.1 域对抗多模态对齐＋传播规则 PageRank

- UDA-RCL: Unsupervised Domain Adaptation for Microservice Root Cause Localization Utilizing Multimodal Data（IEEE Transactions on Services Computing, 2026）[[PDF](https://ieeexplore.ieee.org/document/11304608)] [[CODE](https://github.com/xsarvin/UDA-RCL)]：把日志、指标与调用链聚合为统一事件表示，通过多模态编码器和域对抗训练对齐成熟源系统与无标签目标系统，再用嵌入异常传播规则的 PageRank 定位根因服务。

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

- Reasoning Language Models for Root Cause Analysis in 5G Wireless Networks（CoRR, 2025）[[PDF](https://arxiv.org/pdf/2507.21974)] [[DATASET](https://huggingface.co/datasets/netop/TeleLogs)] [[EVALUATION CODE](https://github.com/gsma-labs/evals/tree/main/src/evals/telelogs)]：基于面向 5G RCA 的合成/策展 TeleLogs 基准，用多智能体生成结构化推理轨迹，先对 Qwen2.5 监督微调再以 GRPO 提升根因分类和解释能力。

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

- PRAXIS: Integrating Program Analysis with Observability for Root-Cause Analysis（DSN Research Track, 2026）[[PDF](https://arxiv.org/pdf/2512.22113)] [[ARTIFACT](https://doi.org/10.5281/zenodo.19163486)]：结合可观测性与静态程序分析工具，由 LLM 充当遍历策略，在服务依赖图和程序依赖图之间逐层搜索，定位微服务、代码路径或配置项；正式版本获 DSN 2026 Best Paper。

### 7.5 多智能体递归推理

#### 7.5.1 沿 Trace 图并行的 Recursion-of-Thought

- Towards In-Depth Root Cause Localization for Microservices with Multi-Agent Recursion-of-Thought（IEEE Transactions on Dependable and Secure Computing, 2026）[[PDF](https://arxiv.org/pdf/2605.14866)] [[CODE](https://github.com/LLMLog/RCLAgent)]：RCLAgent 按 Trace 图为 Span 分配专用 Agent，并沿拓扑递归、并行深入证据，最后融合根级诊断报告和全局证据图，以缓解上下文爆炸和串行推理低效。

---

## 8. 跨场景与输出粒度索引


| 场景 | 代表方法 | 主要机制 |
| --- | --- | --- |
| 微服务/云原生 | BARO、MicroRCA、RUN、MULAN、Holistic RCA、RCAgent | 变点、图传播、因果发现、多模态与工具推理 |
| 云边/边缘 IoT | MALEAF、MicroCERCL、Decentralized RCL、Cascaded GNN、Liability RCA | 轻量分类、异构层次图、去中心化与责任归因 |
| 5G/通信 | STRCA、Simba、RAFT、Domino、TeleLogs RLM | 信令模式、时空图、协议注入、跨层链与领域推理 |
| 自动驾驶 | ROCAS、DVCA、Fault Tree＋BN | 赛博—物理变异、理想模块替换、概率安全责任 |
| 通用动态系统 | AERCA、Counterfactual Dynamical RCA、IDI | Granger 干预、动态 SCM、分布内修复 |

| 输出粒度 | 代表方法 |
| --- | --- |
| KPI/指标 | CauseInfer、HeMiRCA、AERCA、KPIRoot+ |
| 服务 | MicroRCA、RUN、MULAN、RCAgent |
| 实例/主机 | MicroIRC、Holistic RCA、Hypergraph RCA |
| 进程/代码 | MHP-RCA、TrinityRCL、PRAXIS |
| 多根因/责任 | IDI、Liability RCA、Fault Tree＋BN |
| 根因＋解释/建议 | Recommending RCA、TAMO、RCLAgent |
