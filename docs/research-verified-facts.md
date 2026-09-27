# 调研事实基础（已验证）

> 本文件是《从数据到生产：AI 模型的完整生命周期》技术分享的事实底稿。
> 标注说明：✅ = 3 票对抗验证逐字通过（deep-research 工作流，2026-09-27）；✅* = 本人对一手来源 curl 逐字核对；◆ = 工作流抓取代理提取的一手引文（单源，使用时注明出处）。

## A. 传统 ML 生命周期与方法论

| # | 事实 | 置信度 | 来源 |
|---|------|--------|------|
| A1 | CRISP-DM 于 1999 年发布，6 阶段（业务理解→数据理解→数据准备→建模→评估→部署），是数据科学最常用方法论（KDnuggets 调查佐证） | ✅* | datascience-pm.com/crisp-dm-2/ |
| A2 | Google Cloud 定义 ML 交付 8 步：数据抽取→数据分析→数据准备（清洗/训练验证测试集划分/特征工程）→模型训练→模型评测→模型验证（是否符合预测服务标准）→模型服务→模型监控；"这些步骤的自动化水平定义 ML 流程的成熟度" | ✅ | Google Cloud Architecture Center（页面最后审核 2024-08-28） |
| A3 | Google MLOps 成熟度 3 级：L0 全手动（notebook/脚本）→L1 ML 流水线自动化+持续训练 →L2 ML 流水线与 CI/CD 全自动化。注意：该文自述"主要适用于预测型 AI 系统" | ✅ | 同上 |
| A4 | Microsoft Azure MLOps 成熟度 5 级（L0 No MLOps→L1 有 DevOps 无 MLOps→L2 自动化训练→L3 自动化模型部署→L4 Full MLOps）。L3 特征：发布自动化、部署可完整回溯至原始数据、A/B 测试集成进部署流程；L4：漂移/回归信号自动触发重训。与 Google 的 0-2 三级编号体系不同，引用需明确归属 | ✅ | learn.microsoft.com（ms.date 2026-06-03） |
| A5 | MLOps 与 DevOps 区别：CI 不止测代码，还要测数据和数据 schema、模型；CD 部署的是整个训练流水线（再自动部署预测服务）；CT（持续训练）是 ML 独有 | ✅ | Google Cloud 同 A2 |
| A6 | 生产模型不仅因代码缺陷退化，更因数据分布持续演变而衰减（无代码变更也发生）；须跟踪数据摘要统计并监控线上性能，触发条件含性能退化与概念漂移（concept drift）；L0 典型缺陷是缺乏主动性能监控 | ✅ | Google Cloud 同 A2 |
| A7 | MLflow 传统 ML 四大能力：实验追踪、模型评测、生产级模型注册（registry）、模型部署；现已扩展为 LLM/Agent 平台：tracing（基于 OpenTelemetry）、评测、prompt registry、prompt 自动优化、AI Gateway（"production-grade"为官网自称） | ✅ | mlflow.org 及其文档页（2026-09-27 核验） |
| A8 | Uber Michelangelo 平台（2017，2016 年起生产运行）把公司级端到端 ML 工作流标准化为 6 步：manage data, train, evaluate, deploy, make predictions, monitor predictions | ◆ | uber.com/blog/michelangelo-machine-learning-platform/（2017-09-05） |

## B. 演进：深度学习 → Transformer → LLM

| # | 事实 | 置信度 | 来源 |
|---|------|--------|------|
| B1 | Transformer（Vaswani et al., 2017, "Attention Is All You Need"）：完全基于注意力、抛弃循环与卷积；并行性更好、训练时间显著缩短；WMT14 英德 28.4 BLEU（超当时最佳 2+ BLEU），训练仅用 8 张 GPU 3.5 天 | ✅* | arXiv:1706.03762 |
| B2 | 预训练数据代际增长：GPT-3(175B) 300B tokens → PaLM(540B) 780B → LLaMA(65B) 1.4T（2048 张 A100-80G，约 21 天，约 380 tokens/sec/GPU）→ Llama 3 约 15T（Llama 2 为 1.8T） | ✅ | RUC 综述 Table 4 + LLaMA/Llama 3 论文交叉确认 |
| B3 | Chinchilla（DeepMind 2022）：训练 400+ 模型（70M-16B+ 参数、5-500B tokens）实证——算力最优要求模型参数与训练 tokens 等比扩展（每翻倍同翻倍）；Chinchilla(70B, 同 Gopher 算力但 4 倍数据) 全面显著胜过 Gopher(280B)/GPT-3(175B)/Jurassic-1(178B)/MT-NLG(530B)；2022 前的大模型"显著欠训练"。注：2023 后业界为推理经济学故意"过训练"超该比例（如 Llama 3），属适用范围细化 | ✅ | arXiv:2203.15556 |
| B4 | Llama 3 405B 在最多 16K 张 H100（700W TDP、80GB HBM3、Grand Teton 服务器）上训练，3.8×10^25 FLOPs（约为 Llama 2 的 50 倍）；54 天预训练快照期内共 466 次作业中断：47 次计划内（固件升级等）+ 419 次意外，其中约 78% 归因于硬件问题（GPU 故障为最大类）；用 MAST 全局调度 | ✅ | arXiv:2407.21783（全文逐字核对） |
| B5 | 分布式训练：3D 并行=数据并行+张量并行+流水线并行，辅以 ZeRO/FSDP、激活重计算、BF16 混合精度；代表框架 DeepSpeed（微软）/Megatron-LM（NVIDIA）。案例：BLOOM-176B 在 384 张 A100 上 8 路数据×4 路张量×12 路流水线并行训练（8×4×12=384），Megatron-DeepSpeed，ZeRO stage 1 | ✅ | RUC 综述 + BLOOM 论文（arXiv:2211.05100）交叉验证 |
| B6 | Meta 建了两个 24,576 H100 数据中心级集群用于 Llama 3 训练 | ◆ | engineering.fb.com（2024-03-12） |

## C. LLM 生命周期：预训练 → 后训练 → 评测

| # | 事实 | 置信度 | 来源 |
|---|------|--------|------|
| C1 | RUC 综述（Zhao et al., 144 页，发表于 Frontiers of Computer Science 2026）按开发流程组织四大板块：预训练、适配微调（adaptation tuning=指令微调+对齐微调）、使用、能力评测（原文为 "four major aspects"，"四阶段生命周期"是解读性表述） | ✅(2-1) | arXiv:2303.18223 |
| C2 | Meta 官方："现代基础模型开发由两个主要阶段组成：(1) 预训练——用下一词预测等简单任务大规模训练；(2) 后训练——调优以遵循指令、对齐人类偏好、增强特定能力" | ✅ | arXiv:2407.21783（逐字） |
| C3 | 预训练数据标准流水线（RUC 综述 Sec IV-A）：质量过滤（分类器打分或语言/统计/关键词启发式规则）→ 去重（句子级/文档级/数据集级，实践中联合使用）→ 隐私脱敏（移除姓名/地址/电话等 PII）→ tokenization（byte-level BPE）；重复数据降低多样性、致训练不稳定（Lee et al., ACL 2022） | ✅ | arXiv:2303.18223 |
| C4 | 数据质量>数量：HuggingFace FineWeb=15T tokens（96 个 Common Crawl 快照）；其 1.3T 教育文本子集 FineWeb-Edu 在知识与推理密集基准上显著更好——同等预算（1.71B 模型/350B tokens）下 1.3T 过滤池胜 15T 母池：MMLU 33→37、ARC 46→57（时点：2024-06，后续 DCLM 已超越） | ✅ | arXiv:2406.17557 |
| C5 | InstructGPT（OpenAI 2022）确立三段式对齐流水线：(1) 标注员示范数据 SFT→(2) 人类排序数据训练奖励模型→(3) PPO 强化学习微调（带对 SFT 模型的 per-token KL 惩罚防奖励过度优化）；策略模型 175B、奖励模型 6B（175B RM 训练不稳定且为省算力） | ✅ | arXiv:2203.02155 |
| C6 | "把语言模型做大并不能使其更善于遵循用户意图"（大模型会生成不真实/有毒/无用输出=未对齐）；人类评估中 1.3B InstructGPT 输出优于 175B GPT-3（参数少 100 倍）——对齐比规模更重要 | ✅ | arXiv:2203.02155（摘要逐字） |
| C7 | 预训练+SFT 仍不足：仍会产生有害/未对齐输出，数学与代码领域表现不佳——RL 后训练的动机 | ✅ | arXiv:2407.16216（Salesforce 综述） |
| C8 | 后训练算法分流（注：此分类为该综述 2025/2026 框架，2-1 票，非业界统一术语）：偏好类 RLHF（含 DPO）与可验证奖励 RLVR（PPO/GRPO）；经典用法中 PPO 即 RLHF 标准算法 | ✅(2-1) | arXiv:2407.16216 |
| C9 | LoRA（微软 2021）：冻结预训练权重，向每层 Transformer 注入可训练低秩分解矩阵；对比 GPT-3 175B 全量微调（Adam）：可训练参数减少 10,000 倍、GPU 显存需求降 3 倍；质量持平或更好；不同于 adapters，无额外推理延迟 | ✅* | arXiv:2106.09685 |
| C10 | QLoRA（2023）：梯度穿过冻结的 4-bit 量化模型反传到 LoRA；单张 48GB GPU 微调 65B 模型并保持 16-bit 全量微调性能；创新点：NF4 数据类型、双重量化、分页优化器；Guanaco 24 小时单卡微调达到 ChatGPT 99.3% 表现水平（Vicuna 基准）；并证明 GPT-4 评测是人类评测便宜且合理的替代 | ✅* | arXiv:2305.14314 |
| C11 | LLM-as-a-judge（MT-Bench/Chatbot Arena 论文）：GPT-4 等强模型评判与人类偏好一致性超 80%，达到人类之间一致性的同等水平；已知偏差：位置偏差、冗长偏差、自我增强偏差、推理能力受限；LLM-as-judge 是可扩展、可解释地近似人类偏好的方式 | ✅* | arXiv:2306.05685 |

## D. 应用层：RAG 与 Agent

| # | 事实 | 置信度 | 来源 |
|---|------|--------|------|
| D1 | RAG 原始论文（Lewis et al., Meta 2020）：结合参数记忆（预训练 seq2seq）与非参数记忆（维基百科稠密向量索引），用预训练神经检索器访问 | ✅* | arXiv:2005.11401 |
| D2 | RAG 标准流程（Anthropic 官方表述）：知识库切块（通常不超过几百 tokens）→ embedding 模型转向量 → 存向量数据库（语义相似检索）→ 运行时按查询检索并传入 prompt 生成 | ✅* | anthropic.com/news/contextual-retrieval |
| D3 | Anthropic Contextual Retrieval：给每个块prepend上下文说明（Contextual Embeddings + Contextual BM25），检索失败率降低 49%，结合重排（reranking）降低 67%；top-20 优于 top-10/top-5；重排优于不重排；各优化可叠加 | ✅* | 同上 |
| D4 | Anthropic 的架构区分：workflows=LLM 与工具通过预定义代码路径编排；agents=LLM 动态主导自身过程与工具使用、掌控任务完成方式；统称 agentic systems。基本构建块是 augmented LLM（检索/工具/记忆增强）；常见 workflow 模式：prompt chaining、routing、orchestrator-workers、evaluator-optimizer；"最成功的实现使用简单、可组合的模式而非复杂框架" | ✅* | anthropic.com/engineering/building-effective-agents（2024-12-19） |
| D5 | MCP（Model Context Protocol）：开源标准，连接 AI 应用与外部系统——数据源（本地文件/数据库）、工具（搜索引擎/计算器）、工作流；官方类比"AI 应用的 USB-C 接口" | ✅* | modelcontextprotocol.io |
| D6 | ReAct（2022）：LLM 交错生成推理轨迹与任务动作——推理帮助归纳/跟踪/更新计划与处理异常，动作连接外部知识源/环境获取信息；在 HotpotQA/Fever 上通过与 Wikipedia API 交互减少幻觉与错误传播；ALFWorld/WebShop 上绝对成功率 +34%/+10% | ✅* | arXiv:2210.03629 |

## E. 上线与持续迭代（LLMOps）

| # | 事实 | 置信度 | 来源 |
|---|------|--------|------|
| E1 | KV cache 特性（vLLM 博客）：大（LLaMA-13B 单序列可达 1.7GB）且动态（随序列长度变化，难以预测）；现有系统因碎片化与过度预留浪费 60%-80% 显存；PagedAttention（虚拟内存式分页管理）把浪费降到 4% 以下；吞吐比 HF Transformers 高至多 24 倍、比 TGI 高至多 3.5 倍（LLaMA-7B@A10G / 13B@A100-40GB，ShareGPT 长度分布，无需改模型结构） | ✅* | blog.vllm.ai（2023-06-20） |
| E2 | 量化目的（vLLM 文档）：以模型精度换取更小内存占用，使大模型能运行于更多设备 | ◆ | docs.vllm.ai |
| E3 | Eugene Yan：典型事实不一致/无关率 5-10%——即使已有 RAG 接地与良好 prompt engineering；从 LLM 供应商了解到的信息：降到 2% 以下极其困难——因此输出侧需要事实不一致护栏 | ✅* | eugeneyan.com/writing/evals/ |
| E4 | Datadog LLM Observability 提供生产级 RAG 应用实时幻觉检测（可评估应用表现、联系受影响用户、迭代提升可靠性）——LLMOps 质量监控产品化案例 | ◆ | datadoghq.com/blog/ai/llm-hallucination-detection/ |
| E5 | LinkedIn 在 vLLM 服务栈中启用 n-gram 投机解码（无辅助 draft 模型）加速其招聘助手（长结构化输出场景） | ◆ | linkedin.com/blog（工程博客） |
| E6 | Anthropic 多智能体研究系统内部评测：多智能体（Opus 4 主控+Sonnet 4 子代理）比单智能体 Opus 4 高 90.2%（breadth-first 查询收益最大）；token 消耗约 15 倍——"多智能体系统是广度优先搜索问题的吞吐量优化" | ◆ | anthropic.com/engineering/multi-agent-research-system |

## 表述红线（调研特别提醒）

1. Google 8 步/3 级自述"主要适用于预测型 AI 系统"，不要说成 Google 对 LLM 生命周期的处方。
2. Google（0-2 三级）与 Microsoft（0-4 五级）编号体系不同，引用必须带归属。
3. RUC 综述原文是 "four major aspects"（四大板块），说"生命周期"时要说明是按开发流程组织的解读。
4. DPO=RLHF 族 / PPO=RLVR 族的分类是单一综述框架，与 InstructGPT 时代"PPO 即 RLHF"的经典用法不同，须注明。
5. FineWeb"优于其他开放数据集"是 2024-06 时点表述；Chinchilla 最优比被 2023 后"过训练"实践细化（非否定）。
6. MLflow "production-grade" 等为厂商自称，只断言功能存在。
