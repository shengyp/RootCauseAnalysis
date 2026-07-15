# RootCauseAnalysis
## 1. 综述、数据集与评测（Surveys, Datasets and Benchmarks）

- **核心思想**：整理根因分析领域的研究现状、方法分类、公开数据集和评测基准，为后续论文阅读建立整体框架。

- **代表工作**：

  i. **综合综述（General Surveys）**

  - Intelligent Root Cause Localization in MicroService Systems: A Survey and New Perspectives（ACM Computing Surveys, 2025）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3736755)]：梳理了基于指标、日志、调用链及多模态可观测数据的微服务根因定位方法，并总结动态拓扑、多源融合、实时性、泛化性与可解释性等未来研究方向
  - A Comprehensive Survey on Root Cause Analysis in (Micro) Services: Methodologies, Challenges, and Trends（CoRR, 2024）[[PDF](https://arxiv.org/pdf/2408.00803)]：梳理了基于指标、调用链、日志、多模态数据及大模型增强的微服务根因分析方法，并总结数据质量、动态依赖、故障传播、评测标准和自动化诊断等挑战与趋势

  ii. **因果根因分析综述（Causal RCA Surveys）**

  - Root Cause Analysis for Microservice System based on Causal Inference: How Far Are We?（ASE, 2024）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3691620.3695065)] [[CODE](https://github.com/phamquiluan/RCAEval/tree/ase24)]：通过在合成数据与真实微服务数据上评测 9 种因果发现方法和 21 种根因分析方法，发现现有方法均无法兼顾准确性、效率与参数鲁棒性，且合成数据上的结果难以真实反映实际系统性能

  iii. **数据集与评测基准（Datasets and Benchmarks）**

  - LogDx-CI: Benchmarking Log Reduction Tools for LLM Root-Cause Diagnosis(CoRR, 2026)[[PDF](https://arxiv.org/pdf/2605.28876)] [[CODE](https://github.com/eyuansu62/LogDx)]: 在 35 个真实 GitHub Actions 故障上评测 11 种日志压缩方法，发现 grep+tail 混合路由在单轮诊断中具有最佳成本—质量权衡，而 Agent 虽能通过工具调用弥补低质量上下文，却会增加诊断成本
  - Cloud-OpsBench: A Reproducible Benchmark for Agentic Root Cause Analysis in Cloud Systems(CoRR，2026）[[PDF](https://arxiv.org/pdf/2603.00468)] [[CODE](https://github.com/LLM4Ops/Cloud-OpsBench)]：通过“状态快照”构建可确定性复现的 Kubernetes 数字孪生，提供覆盖 40 类根因、452 个故障案例的交互式 Agent RCA 基准，同时评估最终定位结果与诊断轨迹，并可作为 SFT 数据引擎和 RL 训练沙箱
  - Pooled Leaderboards Hide System-Specific Winners: A Reporting-Protocol Audit of Offline Root-Cause Analysis Benchmarks(CoRR, 2026)[[PDF](https://arxiv.org/pdf/2606.29159)] [[CODE](https://anonymous.4open.science/r/rca-leaderboard-audit-artifact-1FC2)]: 通过审计 OpenRCA、RCAEval 和 PetShop 的 11 个子系统、778 个案例，证明汇总 Top-1 排名会掩盖不同系统间的方法胜负反转
  - A causal inference-based root cause analysis framework using multi-modal data in large-complex system. (SCI, JCR Q1, 2026)[[PDF](https://www.sciencedirect.com/science/article/pii/S0951832025007203)]: 提出 CEI-LSBN 框架，融合日志事件与表格时序数据，通过反事实推理和因果追踪同时定位根因事件、触发变量及其具体取值
  - AnoMod: A Dataset for Anomaly Detection and Root Cause Analysis in Microservice Systems.(CCF-C, 2026)[[PDF](https://arxiv.org/pdf/2601.22881)][[CODE](https://github.com/EvoTestOps/AnoMod)]: 基于 SocialNetwork 和 TrainTicket 两个微服务系统注入性能、服务、数据库及代码级异常，采集日志、指标、调用链、API 响应和代码覆盖率五种模态，构建支持跨模态异常检测及服务/代码级根因定位的公开数据集
  - LEMMA-RCA: A Large Multi-modal Multi-domain Dataset for Root Cause Analysis.()[[PDF](https://arxiv.org/pdf/2406.05375)] [[CODE](https://github.com/KnowledgeDiscovery/rca_baselines)]: 构建覆盖微服务、云计算、水处理与配水等 IT/OT 场景的多域多模态 RCA 数据集，提供日志、指标及部分调用链数据和故障时间—根因实体标注，用于统一评测离线/在线及单模态/多模态根因定位方法
---

## 2. 可观测数据驱动的根因分析（Observability-Driven RCA）

- **核心思想**：利用微服务运行过程中产生的指标、日志、调用链和事件等可观测数据，识别异常组件并定位故障根因。

- **代表工作**：

  i. **指标与时间序列驱动（Metric and Time-Series Driven）**

  - MicroHFRCL: A History Faults Based Root Cause Localization Framework in Microservice Systems（IJCNN, 2024）[[PDF](https://ieeexplore.ieee.org/document/10650929)]：基于实例级指标数据构建因果图，使用 GCN＋Transformer 编码历史故障异常子图，通过当前故障与历史故障的相似度调整图权重，再以 PageRank 排序根因实例
  - BARO: Robust Root Cause Analysis for Microservices via Multivariate Bayesian Online Change Point Detection（CCF-A, 2024）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3660805)] [[CODE](https://github.com/phamquiluan/baro)]：提出端到端微服务故障诊断方法，以多变量贝叶斯在线变点检测联合建模指标依赖并检测异常，再通过非参数统计检验 RobustScorer 对根因排序，从而降低异常区间检测误差对 RCA 的影响
  - NRCAC: Non-Intrusive Microservice Root Cause Analysis Framework for Cloud Providers（INFOCOM, 2025）[[PDF](https://ieeexplore.ieee.org/document/11044716)] [[CODE](https://github.com/micturkey/NRCAC)]：用 eBPF 从宿主机内核无侵入采集微服务 CPU、内存、TCP 调用负载等时间序列，并提取服务依赖知识，通过 RCD-DK 将拓扑约束注入局部因果发现、缩小因果图搜索空间并定位根因
  - MHP-RCA: Multivariate Hawkes Process-based Root Cause Analysis in Microservice Systems（Information and Software Technolog, 2026）[[PDF](https://www.sciencedirect.com/science/article/pii/S0950584925002770)] [[CODE](https://github.com/WHU-AISE/MHP-RCA)]：将指标时间序列与系统审计日志转化为异常事件序列，利用多变量 Hawkes 过程学习事件间的时序因果激励图，从而将根因细化定位到具体服务进程
  - KPIRoot+: An Efficient Integrated Framework for Anomaly Detection and Root Cause Analysis in Large-Scale Cloud Systems（Empirical Software Engineering, 2026）[[PDF](https://link.springer.com/article/10.1007/s10664-025-10769-0)][[CODE](https://github.com/OpsPAI/KPIRoot)]：面向大规模云系统，以 KPI 单模态时间序列为输入，通过时序分解检测不同类型的异常、改进 SAX 表示压缩变化趋势，再融合趋势相似性与变化先后关系定位根因 KPI/VM
  - InstantOps: A Joint Approach to System Failure Prediction and Root Cause Identification in Microservices Cloud-Native Applications（ICPE, 2024）[[PDF](https://dl.acm.org/doi/pdf/10.1145/3629526.3645047)]：融合日志、运行事件、指标和调用链，以调用链构建动态服务图、日志与指标作为节点特征，并通过 GNN+GRU 联合完成故障提前预测和服务节点级根因定位
  - MicroIRC: Instance-level Root Cause Localization for Microservice Systems（Journal of Systems and Software, 2024）[[PDF](https://www.sciencedirect.com/science/article/pii/S0164121224001900)] [[CODE](https://github.com/WHU-AISE/MicroIRC)]：从实例级多维指标中提取细粒度时间序列特征，利用 MetricSage 图神经网络学习异常模式，并结合实时服务—实例—主机异构加权拓扑和个性化随机游走生成实例级根因排序

  ii. **日志驱动（Log Driven）**

  - AetherLog: Log-based Root Cause Analysis by Integrating Large Language Models with Knowledge Graphs(ISSRE, 2025)[[PDF](https://ieeexplore.ieee.org/document/11229645)] [[CODE](https://github.com/ISSRE25-Submission-56/AetherLog)]:利用 LLM 从历史故障日志中抽取实体与关系并构建故障知识图谱，在线阶段再对待诊断日志进行摘要、实体检索和知识增强推理以输出根因
  - LogRCA: Log-Based Root Cause Analysis for Distributed Services（Euro-Par, 2024）[[PDF](https://arxiv.org/pdf/2405.13599)]：采用半监督 PU Learning＋Transformer 对故障前时间窗口内的日志行计算根因相关分数，并通过自动聚类与数据重平衡处理罕见及未知故障，最终返回最能共同解释故障的最小日志集合
  - Progressing from Anomaly Detection to Automated Log Labeling and Pioneering Root Cause Analysis（CoRR, 2023）[[PDF](https://arxiv.org/abs/2312.14748)] [[CODE](https://github.com/dos-group/LogLAB)]：提出日志模板/属性/上下文异常分类体系，并利用监控告警提供的粗粒度故障时间窗和 PU 弱监督模型 LogLAB 自动标注异常日志，同时设想以日志行根因分数实现 RCA，但尚未完成和评测完整的根因定位方法

  iii. **调用链驱动（Trace Driven）**

  - MicroRCA: Root Cause Localization of Performance Issues in Microservices（NOMS, 2020）[[PDF](https://ieeexplore.ieee.org/document/9110353)][[CODE](https://github.com/elastisys/MicroRCA)]：融合服务调用关系、服务响应时间和主机资源利用率，先由调用关系构建服务—主机属性图，再通过异常传播权重、指标相关性与个性化 PageRank 排序根因服务
  - Practical Root Cause Localization for Microservice Systems via Trace Analysis（IWQoS, 2021）[[PDF](https://netman.aiops.org/wp-content/uploads/2021/05/1570705191.pdf)] [[CODE](https://github.com/NetManAIOps/TraceRCA)]：对调用链中各 Span/Invocation 的多维指标进行无监督异常检测，将请求划分为正常与异常 Trace，再通过 FP-Growth 挖掘异常 Trace 高频、正常 Trace 低频的可疑服务集合并排序根因
  - TraceDiag: Adaptive, Interpretable, and Efficient Root Cause Analysis on Large-Scale Microservice Systems（CCF-A, 2023）[[PDF](https://dl.acm.org/doi/10.1145/3611643.3613864)]：以分布式调用链构建服务依赖图及服务级时序特征，先利用强化学习自适应剪除与当前故障无关的冗余服务，再在裁剪后的图上执行基于条件独立性检验的因果根因排序
  - Trace-based Multi-Dimensional Root Cause Localization of Performance Issues in Microservice Systems（ICSE, 2024）[[PDF](https://ieeexplore.ieee.org/abstract/document/10549362)] [[Datasets](https://zenodo.org/records/12751520)]：从调用链中提取关键路径，并将 Trace 结构与各 Span 的服务、实例、版本及请求属性编码为事件序列，再通过对比序列模式挖掘与频谱分析定位多维根因

  iv. **多模态融合驱动（Multimodal and Heterogeneous Data Driven）**

  - Nezha: Interpretable Fine-Grained Root Causes Analysis for Microservices on Multi-modal Observability Data（ESEC/FSE, 2023）[[PDF](https://dl.acm.org/doi/10.1145/3611643.3616249)] [[CODE](https://github.com/IntelligentDDS/Nezha)]：融合指标、日志与调用链，将三类异构数据统一转化为事件，分别构建正常期与故障期事件图，并通过事件模式挖掘与差异排序将根因定位至服务、代码区域或资源类型
  - TrinityRCL: Multi-Granular and Code-Level Root Cause Localization Using Multiple Types of Telemetry Data in Microservice Systems（IEEE Transactions on Software Engineering, 2023）[[PDF](https://ieeexplore.ieee.org/document/10034937)] [[Third-party reproduction](https://github.com/Apex-ISET/TrinityRCL_Reproduction)]：综合使用指标、日志与调用链构建跨应用、服务、主机、指标和代码实体的因果图，再依据异常相关性进行多粒度根因排序，其中日志可将根因进一步映射到具体代码位置
  - HeMiRCA: Fine-Grained Root Cause Analysis for Microservices with Heterogeneous Data Sources（ACM Transactions on Software Engineering and Methodology, 2024）[[PDF](https://dl.acm.org/doi/full/10.1145/3674726)] [[CODE](https://github.com/zhouruixingzhu/HeMiRCA)]：先从分布式调用链的 Span 延迟构造时序异常分数，再利用 Spearman 相关性衡量该分数与各服务指标时间序列的单调关联，分层定位根因服务与根因指标
  - Holistic Root Cause Analysis for Failures in Cloud-Native Systems Through Observability Data（IEEE Transactions on Services Computing, 2024）[[PDF](https://ieeexplore.ieee.org/document/10713920)] [[CODE](https://github.com/baiyanquan/HolisticRCA)]：将指标、日志与调用链特征分别编码并映射至共享向量空间，按主机、Pod、服务等资源实体融合特征，再通过资源实体关系图上的 Graph Attention Network＋掩码学习实现跨层级根因实体及关键异常观测定位
  - UDA-RCL: Unsupervised Domain Adaptation for Microservice Root Cause Localization Utilizing Multimodal Data（IEEE Transactions on Services Computing, 2026）[[PDF](https://ieeexplore.ieee.org/document/11304608)] [[CODE](https://github.com/xsarvin/UDA-RCL)]：将日志、指标与调用链聚合为统一事件表示，通过多模态编码器和域对抗训练对齐成熟源系统与无标签目标系统的特征分布，再利用嵌入异常传播规则的 PageRank 分类器定位根因服务
---

## 3. 图结构与因果关系驱动的根因分析（Graph and Causality-Driven RCA）

- **核心思想**：将服务、指标、异常事件和依赖关系建模为图，通过图排序、因果发现和异常传播分析识别真正的故障源。

- **代表工作**：

  i. **依赖图与图排序驱动（Dependency Graph and Graph Ranking Driven）**

  - CauseInfer: Automated End-to-End Performance Diagnosis with Hierarchical Causality Graph in Cloud Environment（IEEE Transactions on Services Computing, 2019）[[PDF](https://ieeexplore.ieee.org/document/7563819)]：通过通信流量的时滞相关性构建服务层因果依赖图，并利用 PC 算法与条件独立性检验学习服务内部的指标因果图，形成两层层次因果图，再沿异常因果路径进行 DFS 搜索，依次定位故障服务和根因指标
  - Graph-based Root Cause Analysis for Service-Oriented and Microservice Architectures（Journal of Systems and Software, 2020）[[PDF](https://www.sciencedirect.com/science/article/pii/S0164121219302067)]：将服务、容器、主机及其逻辑和物理连接表示为带属性系统图，从当前异常状态提取异常子图，并与历史已诊断异常图知识库进行加权图相似度匹配，以最相似案例确定根因

  ii. **因果发现驱动（Causal Discovery Driven）**

  - Root Cause Analysis of Failures in Microservices through Causal Discovery（NeurIPS, 2022）[[PDF](https://dl.acm.org/doi/10.5555/3600270.3602529)] [[CODE](https://github.com/azamikram/rcd)]：将故障前后的指标时间序列分别建模为观测数据与软干预数据，引入故障指示节点，并通过改进的 Ψ-PC 与分治式层次学习仅发现该节点的局部因果邻域，将其干预目标识别为根因指标
  - Root Cause Analysis in Microservice Using Neural Granger Causal Discovery（AAAI, 2024）[[PDF](https://arxiv.org/abs/2402.01140)] [[CODE](https://github.com/zmlin1998/RUN)]：通过对比学习增强多变量指标时间序列的上下文表示，利用预测模型执行神经 Granger 因果发现并构建有向因果图，最后以个性化 PageRank 排序 Top-k 根因
  - MULAN: Multi-modal Causal Structure Learning and Root Cause Analysis for Microservice Systems（The Web Conference, 2024）[[PDF](https://dl.acm.org/doi/10.1145/3589334.3645442)]：将指标与日志分别转换为时序表示，通过对比学习提取模态共享与特有特征，学习各模态的因果图，并利用 KPI 感知注意力融合为最终因果图，最后通过带重启随机游走排序根因实体
  - Root Cause Analysis of Anomalies in Multivariate Time Series through Granger Causal Discovery（ICLR, 2025）[[PDF](https://openreview.net/pdf?id=k38Th3x4d9)] [[CODE](https://github.com/hanxiao0607/AERCA?)]：通过编码器—解码器联合学习多变量时间序列间的 Granger 因果结构，同时显式建模正常状态下外生变量的分布；将异常视为对外生变量的干预，并根据外生变量偏离正常分布的程度识别根因
  - Chain-of-Event: Interpretable Root Cause Analysis for Microservices through Automatically Learning Weighted Event Causal Graph（FSE Companion, 2024）[[PDF](https://dl.acm.org/doi/10.1145/3663529.3663827)] [[CODE](https://github.com/NetManAIOps/Chain-of-Event)]：将多模态可观测数据转化为事件，利用历史事故监督学习事件重要度与因果边权，构建事故特定的加权事件因果图，并通过事件链贡献的图排序定位根因

  iii. **动态因果图与超图驱动（Dynamic Causal Graph and Hypergraph Driven）**

  - Root Cause Analysis for Microservice Systems via Cascaded Conditional Learning with Hypergraphs（CoRR, 2025）[[PDF](https://arxiv.org/abs/2511.17566)]：融合指标、日志和调用链特征，构建包含调用、部署及负载均衡关系的异构超图，通过 UniGAT-HE 超图注意力网络建模故障传播，并级联执行根因实例定位与故障类型识别

  iv. **图神经网络驱动（Graph Neural Networks Driven）**

  - Root Cause Analysis of Cloud Platform Faults Based on Anomaly Correlation Graph and Graph Neural Network（Engineering Applications of Artificial Intelligence, 2026）[[PDF](https://www.sciencedirect.com/science/article/pii/S0952197626007323)]：从告警日志及拓扑依赖中学习异常事件的因果结构并构建异常关联图，随后采用邻居采样与 GAT 聚合图结构特征，再由 LightGBM 分类定位根因告警节点，并通过历史故障相似度生成解释
  - DynaCausal: Dynamic Causality-Aware Root Cause Analysis for Distributed Microservices（CoRR, 2025）[[PDF](https://arxiv.org/abs/2510.22613)]：融合指标、日志和调用链构建动态加权调用图，通过 Transformer 与混合感知图注意力网络学习时空故障传播表示，并结合动态对比学习和因果优先成对排序定位根因服务


---

## 4. 大语言模型与智能体驱动的根因分析（LLM and Agent-Driven RCA）

- **核心思想**：利用大语言模型理解故障文本、日志和运维知识，并通过检索、工具调用或多智能体协作完成根因分析。

- **代表工作**：

  i. **大语言模型直接推理（Direct LLM Reasoning）**

  - Recommending Root-Cause and Mitigation Steps for Cloud Incidents using Large Language Models（ICSE, 2023）[[PDF](https://arxiv.org/abs/2301.03797)]：基于微软4万余起云事故数据，对GPT-3.x进行零样本、单任务微调和多任务微调，使模型根据事故文本直接生成根因及缓解步骤建议
  - MicroRCA-Agent: Microservice Root Cause Analysis Method Based on Large Language Model Agents（CoRR, 2025）[[PDF](https://arxiv.org/abs/2509.15635)] [[CODE](https://github.com/tangpan360/MicroRCA-Agent)]：先通过Drain与多级过滤提取日志特征、Isolation Forest和状态码检测调用链异常，并以统计过滤及两阶段LLM分析总结指标，最后使用固定跨模态提示融合日志、指标和调用链信息，生成根因组件、原因与推理链


  ii. **知识库与检索增强驱动（Knowledge Base and Retrieval-Augmented Driven）**

  - Mining Root Cause Knowledge from Cloud Service Incident Investigations for AIOps（ICSE-SEIP, 2022）[[PDF](https://ieeexplore.ieee.org/document/9793994)]：使用BERT、RoBERTa和SpanBERT从事故复盘文档中抽取症状、根因与处置措施，构建因果知识图谱，并结合FAISS历史案例检索和GCN链路预测推荐根因
  - PACE-LM: Prompting and Augmentation for Calibrated Confidence Estimation with GPT-4 in Cloud Incident Root Cause Analysis（CoRR, 2023）[[PDF](https://arxiv.org/abs/2309.05833)]：检索历史云事故作为参考，利用GPT-4分两阶段评估当前事故的信息充分度与候选根因预测的可靠性，并融合两项得分输出校准置信度
  - Automatic Root Cause Analysis via Large Language Models for Cloud Incidents（EuroSys, 2024）[[PDF](https://arxiv.org/abs/2305.15778)]：通过预定义事件处理器自动采集日志、指标和调用链等诊断信息，检索相似历史事故作为上下文示例，再由LLM预测根因类别并生成解释
  - LLM-Augmented Knowledge Base Construction for Root Cause Analysis（IEEE Access, 2026）[[PDF](https://ieeexplore.ieee.org/document/11366685)]：提出TelcoInsight，从通信网络支持工单中抽取并生成结构化根因关联规则，对比大语言模型微调、RAG及两者融合方案以自动构建RCA知识库

  iii. **工具调用与智能体驱动（Tool-Augmented and Agent Driven）**

  - RCAgent: Cloud Root Cause Analysis by Autonomous Agents with Tool-Augmented Large Language Models（CIKM, 2024）[[PDF](https://arxiv.org/pdf/2310.16340)]：使用本地部署LLM作为自主单智能体，在“思考—工具调用—环境观察”循环中主动采集并分析云端诊断数据，并通过动作轨迹自一致性、上下文管理和领域知识注入生成根因、证据及解决方案
  - TAMO: Fine-Grained Root Cause Analysis via Tool-Assisted LLM Agent With Multi-Modality Observation Data in Cloud-Native Systems（IEEE Transactions on Services Computing, 2025）[[PDF](https://ieeexplore.ieee.org/document/11229957)]：通过多模态对齐、根因定位和故障类型分类三个专用工具处理日志、指标与调用链，由GPT-4专家智能体整合工具结果、系统上下文和领域知识，生成故障解释及修复建议
  - Adaptive Root Cause Localization for Microservice Systems with Multi-Agent Recursion-of-Thought（CoRR, 2025）[[PDF](https://arxiv.org/abs/2508.20370)] [[CODE](https://github.com/LLMLog/RCLAgent)]：RCLAgent由调用链与指标数据智能体、递归思维智能体及中央协调器组成，通过初始推理、批判反思和最终复核三阶段递归整合跨模态证据与工具分析结果，定位根因服务
  - Agentic Structured Graph Traversal for Root Cause Analysis of Code-related Incidents in Cloud Applications（CoRR, 2025）[[PDF](https://arxiv.org/html/2512.22113v1)]：PRAXIS将可观测性分析与静态程序分析工具结合，由LLM充当遍历策略，在服务依赖图（SDG）与程序依赖图（PDG）之间执行跨层结构化遍历，逐步定位引发事故的微服务、代码路径或配置项

---

## 5. 边缘计算与通信网络根因分析（Edge and Communication Network RCA）

- **核心思想**：面向云边协同、边缘计算、移动通信和 5G 网络，分析跨节点、跨层和动态拓扑中的故障传播过程。

- **代表工作**：

  i. **云边协同与边缘计算驱动（Cloud-Edge and Edge Computing Driven）**

  - Root Cause and Liability Analysis in the Microservices Architecture for Edge IoT Services（ICC, 2023）[[PDF](https://ieeexplore.ieee.org/document/10279721)]：监测边缘物联网微服务的响应时间、内存和可用性等指标，通过故障注入构建连接服务故障与性能指标的因果贝叶斯网络，在发生SLA违约时推断各服务及故障类型的责任概率，并结合TRAILS责任描述符识别未履约的服务提供方
  - ML Driven Root Cause Analysis in Telco Microservices Continuum（ICC Workshops, 2024）[[PDF](https://ieeexplore.ieee.org/document/10615702)] [[CODE](https://github.com/ZHAW-BA-2023-FS-RCA-Mon/maleaf)]：MALEAF扩展GRALAF，周期采集延迟、可用性、CPU负载和内存占用等边云微服务指标，先检测SLA异常，再通过SVM与随机森林估计根因服务及其流量、可靠性或性能故障类型概率
  - Root Cause Localization for Microservice Systems in Cloud-edge Collaborative Environments（CoRR, 2024）[[PDF](https://arxiv.org/abs/2406.13604)][[CODE_1](https://github.com/WDCloudEdge/HybridCloudConfig)] [[CODE_2](https://github.com/WDCloudEdge/MicroCERCL)]：MicroCERCL先解析云边通信内核日志并匹配网络报文以定位断连、丢包等内核级根因，再基于指标异常构建由服务、实例和服务器组成的异构动态拓扑栈，通过类型特定图卷积、LSTM与图注意力池化直接生成应用级根因排名
  - A Decentralized Root Cause Localization Approach for Edge Computing Environments（IEEE Transactions on Services Computing, 2026）[[PDF](https://ieeexplore.ieee.org/document/11414199)]：根据通信与共置关系将边缘微服务划分为多个集群，在各集群内基于分层异常评分独立执行个性化PageRank，并通过集群间点对点交换近似异常分数处理跨集群故障传播
  - A Cascaded Graph Neural Network for Joint Root Cause Localization and Analysis in Edge Computing Environments（CoRR, 2026）[[PDF](https://arxiv.org/abs/2603.01447)]：以通信依赖图执行Louvain聚类，利用P-Net在簇内进行根因节点定位，再由O-Net在簇级图上识别故障类型，并通过多任务损失联合优化RCL与RCA

  ii. **5G 与无线接入网驱动（5G and RAN Driven）**

  - Root Cause Analysis of Anomalies in 5G RAN Using Graph Neural Network and Transformer（CoRR, 2024）[[PDF](https://arxiv.org/abs/2406.15638)] [[CODE](https://github.com/PINetDalhousie/Simba)]：Simba以基站KPI序列为输入，通过图学习模块动态生成基站邻接关系，由GCN提取空间故障传播特征、Transformer建模时间依赖，最终联合识别异常类型并定位受影响基站
  - STRCA: A Lightweight and Accurate Root Cause Analysis System Based on 5G Signalling Trace（ICIC, 2024）[[PDF](https://dl.acm.org/doi/10.1007/978-981-97-5672-8_4)]：从海量5G核心网信令报文中重构端到端信令轨迹，以轻量异常检测识别异常轨迹，再挖掘可疑网元集合并利用专门设计的评分指标排序定位根因网元
  - RAFT: A Real-Time Framework for Root Cause Analysis in 5G and Beyond Vulnerability Detection（CCNC, 2024）[[PDF](https://ieeexplore.ieee.org/document/10454824)]：对5G RRC认证与授权流程执行控制指令模糊注入以触发潜在漏洞，从通信日志的随机片段中利用CBOW提取系统状态及状态转移，并通过因果分析实时定位攻击或异常输入的类型与位置
  - Automated, Cross-Layer Root Cause Analysis of 5G Video-Conferencing Quality Degradation（ACM Internet Measurement Conference, 2025）[[PDF](https://dl.acm.org/doi/10.1145/3730567.3764434)] [[CODE][github.com/PrincetonUniversity/Domino-IMC]]：Domino同步采集5G PHY／MAC／RLC、网络与传输层以及WebRTC应用层遥测，以滑动窗口和预定义事件条件生成36维跨层事件向量，并在人工构建的因果图中匹配24条由信道劣化、交叉流量、上行调度、HARQ／RLC重传及RRC状态等底层原因传播至视频卡顿和码率下降的因果链
  - Reasoning Language Models for Root Cause Analysis in 5G Wireless Networks（CoRR, 2025）[[PDF](https://arxiv.org/pdf/2507.21974)] [[Datasets](https://huggingface.co/datasets/netop/TeleLogs)] [[Evaluation code](https://github.com/gsma-labs/evals/tree/main/src/evals/telelogs)]：基于公开的TeleLogs 5G路测故障数据集，训练阶段通过LLM多智能体生成结构化思维链轨迹，先对Qwen2.5执行监督微调，再利用GRPO强化学习提升因果推理能力，使模型根据网络工程参数、用户面观测与吞吐量劣化症状直接识别预定义根因并生成分步骤诊断解释

---

## 6. 面向车联网与自动驾驶的根因分析（IoV and Autonomous Driving RCA）

- **核心思想**：面向车辆、道路基础设施、通信网络、边缘节点和云端服务组成的复杂系统，分析事故、违规行为、系统配置和跨层故障的根本原因。

- **代表工作**：

  i. **自动驾驶事故根因分析（Autonomous Driving Accident RCA）**

  - ROCAS: Root Cause Analysis of Autonomous Driving Accidents via Cyber-Physical Co-mutation（ASE, 2024）[[PDF](https://dl.acm.org/doi/10.1145/3691620.3695530)]：先在仿真器中重放事故，利用物理变异搜索能够消除事故的最小环境实体修改以识别事故触发对象，再通过执行记录差分分析定位最早发生偏差的感知、预测、规划或控制模块并压缩配置搜索空间，最后以赛博变异寻找在保持事故前轨迹基本不变时可消除事故的最小ADS配置修改，从而定位内部误配置

  ii. **自动驾驶测试与违规原因分析（Testing and Violation Cause Analysis）**

  - Towards Automated Driving Violation Cause Analysis in Scenario-Based Testing for Autonomous Driving Systems（CoRR, 2024）[[PDF](https://arxiv.org/abs/2401.10443)]：提出DVCA，在Apollo与LGSVL场景测试中以仿真真值构造感知、预测、定位和控制模块的理想化替代组件，通过反事实delta测试判断替换某组件后违规是否消失以完成组件级归因，再结合车辆动态状态匹配与二分搜索定位具体违规诱导消息

  iii. **车辆安全与概率图模型（Vehicle Safety and Probabilistic Graphical Models）**

  - Advancing Autonomous Vehicle Safety: A Combined Fault Tree Analysis and Bayesian Network Approach（ERAS, 2025）[[PDF](https://arxiv.org/abs/2504.08206)]：以车辆碰撞为顶事件，将传感器、感知、决策、运动控制及外部交互故障分解为故障树，通过最小割集识别关键失效组合，再将故障树的事件与逻辑门映射为贝叶斯网络，在ASIL-B的100 FIT安全目标约束下利用后验推断量化并排序各子系统对碰撞风险的贡献

  iv. **可迁移的反事实与干预方法（Transferable Counterfactual and Intervention Methods）**

  - Counterfactual-based Root Cause Analysis for Dynamical Systems（CCF-B, 2024）[[PDF](https://link.springer.com/chapter/10.1007/978-3-031-70365-2_18)]：以残差神经网络拟合非线性动态结构因果模型并生成反事实轨迹，分别对指定子系统在特定时刻的结构方程与外生扰动实施“恢复正常”干预，再通过可扩展的近似Shapley评分排序导致异常轨迹的子系统与时间点
  - Robust Root Cause Diagnosis using In-Distribution Interventions（ICLR, 2025）[[PDF](https://arxiv.org/abs/2505.00930)] [[CODE](https://github.com/nlokeshiisc/IDI_release)]：IDI先依据“节点自身异常且其父节点正常”的异常条件筛选候选根因，再将候选节点设为正常修复值，并仅从历史正常分布采样下游外生变量，通过结构因果模型的分布内干预检验目标异常是否消失，从而规避异常样本导致的分布外反事实估计误差,多根因场景进一步结合联合干预与Shapley值完成最小根因集合归因
