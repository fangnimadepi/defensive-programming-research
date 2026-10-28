# Research Feasibility Investigation — Pressure-Induced Oververification in LLM Coding Agents

## 0. 你的角色与任务

你现在是一名具有 LLM Agents、Software Engineering、AI Alignment、Mechanistic Interpretability 研究背景的科研助理。

请协助我完成一项潜在学术论文的**前期研究调查与可行性评估**。

本阶段的任务不是编写实验代码、搭建完整框架或开始大规模跑模型，而是通过系统性的文献调查、研究问题分析和实验设计，判断这一选题是否具有进一步开展学术研究的价值。

你必须采用批判性的科研思维。不要为了迎合我的设想而刻意寻找支持证据。要主动寻找已经解决相同问题的研究、可能推翻核心假设的证据，以及实验设计中的混杂因素。

调查截止时间：2026 年 10 月 19 日。优先关注 2024–2026 年的最新研究，同时适当追溯重要的基础工作。

**暂定论文题目：**

Pressure-Induced Oververification in LLM Coding Agents: Behavioral Characterization and Causal Analysis

（高压力环境下 LLM 编程智能体的过度验证行为：行为表征与因果分析）

## 1. 研究背景与动机

### 1.1 现象来源

我们在使用 Codex 等 Coding Agent 时观察到一种现象：

模型有时会表现出显著的防御性编程倾向，例如：

- 对未发生变化的文件反复计算哈希。
- 不必要地重复读取同一文件或检查同一状态。
- 已有充分验证证据，仍反复执行同一测试。
- 任务已经基本完成，却持续增加新的校验步骤。
- 遭遇失败反馈后，出现过度谨慎、效率下降或重复行为循环。

上述现象目前仅来自使用者的主观观察，尚未经过严格实验验证。

我们不应预先认定它们是模型固有的行为缺陷，也不应排除 Agent Harness、系统提示词、工具反馈和执行环境导致这些现象的可能性。

### 1.2 理论动机

在人类心理学中，持续的惩罚压力、失败经历和不确定性可能影响风险偏好及行为选择。

我们希望调查：

当 LLM Agent 面临失败成本、反复负面反馈或任务不确定性时，是否同样可能出现可观测、可量化的行为策略变化？

更进一步，这种变化是否可能涉及模型内部的特定概念表征？

本研究不尝试证明 AI 拥有主观痛苦、意识或人类意义上的心理疾病。

我们关心的是：

**某些类情绪的功能性机制，是否可能参与调节 LLM Agent 的实际行动选择？**

### 1.3 当前的三个研究层次

- L1 — Behavioral Characterization：观察、定义和量化过度验证现象。
- L2 — Behavioral Persistence：研究压力解除后的行为残留、跨任务迁移与上下文依赖。
- L3 — Causal Mechanistic Analysis：寻找与行为变化相关的内部表征，并通过干预尝试建立因果联系。

研究应由 L1 逐步推进，不应预先假设 L2 和 L3 一定成立。

## 2. 核心假设与竞争性解释

请围绕以下假设开展研究，同时尝试反驳它们。

### H1：Pressure-Induced Oververification

相比正常任务环境，高失败成本或持续负面反馈会导致 Coding Agent 增加非必要验证行为。

注意区分总验证次数增加与真正无信息收益的重复验证增加。

### H2：Behavioral Persistence

当压力条件解除后，模型可能仍保留此前形成的保守行动策略。

需要区分同一上下文中的行为残留、历史信息的提示效应，以及跨独立会话的真正持续性。

### H3：Internal Representation Hypothesis

某些内部表征可能与压力相关的行为变化有关，而且干预这些表征可能改变模型的验证决策。

需要区分相关性、线性可解码性和因果作用。

### 竞争性解释

至少评估以下因素：

1. 系统提示词或 Agent Harness 默认要求更多验证。
2. 工具响应异常、超时或状态不可见，导致合理重试。
3. 模型能力不足、任务理解失败或规划循环。
4. 高失败成本下，额外检查本身就是理性的成本优化策略。
5. 先前失败记录留在上下文中，导致上下文条件化行为。
6. 模型习得人类情绪相关语义，但不存在独立的类情绪决策机制。
7. Reward Optimization 或训练偏好导致验证策略过度泛化。

请讨论哪些实验设计可以区分这些解释。

## 3. 任务 A：系统性文献调查

这是本阶段最高优先级任务。

### 3.1 文献检索范围

使用 Google Scholar、Semantic Scholar、arXiv、ACL Anthology、OpenReview、会议论文网站和研究机构官方页面等真实来源。

围绕以下方向进行检索：

**方向 A：LLM Functional Emotions**

- Emotional representations in LLMs
- Functional emotions
- Persona vectors
- Emotion-related activation steering
- Stress-related behavior in language models

**方向 B：LLM Agent 行为与鲁棒性**

- Defensive coding agents
- Redundant tool calls
- Excessive verification
- Verification loops
- Agent overthinking / inefficient tool use
- Tool-use uncertainty and reliability
- Test-time compute efficiency

**方向 C：Reward Pressure 与失败反馈**

- Reward hacking
- Agentic misalignment
- Failure feedback
- Risk-sensitive decision making
- Optimization pressure
- Feedback-induced behavior change

**方向 D：Mechanistic Interpretability**

- Activation probing
- Activation steering
- Activation patching
- Sparse autoencoders
- Linear representation hypothesis
- Causal interventions on model behavior

**方向 E：Software Engineering Agent Evaluation**

- SWE-bench 相关研究
- Coding Agent trajectory analysis
- Tool-use evaluation
- Agent efficiency benchmarks
- Reproducible agent evaluation

### 3.2 必须阅读的起点

请先检查以下真实研究，之后通过引用网络扩展：

1. **Emotion Concepts and their Function in a Large Language Model** — Anthropic, 2026
   https://transformer-circuits.pub/2026/emotions/
2. **Persona Vectors: Monitoring and Controlling Character Traits in Language Models** — Anthropic, 2025
   https://www.anthropic.com/research/persona-vectors
3. **Beyond the Black Box: Interpretability of Agentic AI Tool Use** — 2026
   https://arxiv.org/abs/2605.06890
4. **How to Speculate about Uncertainty in Agentic Coding? A Draft-Model Gate Method** — 2026
   https://arxiv.org/abs/2609.05274
5. **Agentic Coding in the Wild: Characterizing GitHub Copilot at Production Scale** — 2026
   https://arxiv.org/abs/2608.00101

这些论文仅作为检索种子，不代表其结果已经支持我们的假设。

尤其要分析 Anthropic 2026 年关于 Functional Emotions 的研究是否已经覆盖我们的创新点。

### 3.3 文献调查要求

目标筛选 15–25 篇高相关论文，其中选出至少 5–8 篇进行重点分析。

每篇论文尽可能提供：

- 标题、作者、年份、会议或平台
- 可访问的论文地址
- 研究问题
- 研究使用的模型、数据集及实验方法
- 主要发现
- 与本课题的关系
- 是否构成直接的创新性威胁
- 开源代码或实验数据情况

严格区分已证实的论文结果、作者推测和我们自己的推论。

如果无法访问全文，应明确标记为仅基于摘要分析。严禁编造论文、数据、GitHub 地址或实验结果。

最终形成一份 Related Work Matrix。

## 4. 任务 B：研究创新性与可发表性评估

重点回答以下问题：

**Q1.** 是否已有研究明确提出 Pressure-Induced Oververification 或本质相同的问题？

**Q2.** 当前文献有没有研究失败压力与 Coding Agent 重复工具调用之间的因果关系？

**Q3.** 与传统的 Agent Efficiency、Tool-Use Reliability、Overthinking 工作相比，我们的真正差异是什么？

**Q4.** 与 Anthropic Functional Emotions 研究相比，我们是否只是更换模型、任务和实验环境？

**Q5.** 如果只发现 L1 行为效应，没有发现内部功能性情绪机制，是否仍有足够科研价值？

**Q6.** 如果行为可以完全由理性的风险敏感决策解释，这是否使研究失去意义？

**Q7.** 哪些研究内容最可能具有创新性，哪些已经高度同质化？

请给出至少 3 条潜在研究路线：

- 路线 A：偏 Agent Behavioral Evaluation
- 路线 B：偏 AI Alignment / Behavioral Robustness
- 路线 C：偏 Mechanistic Interpretability

分别评估创新性、技术难度、实验成本、证据要求和研究风险。

研究定位上可参考 ACL、EMNLP、NAACL、ICLR、NeurIPS、ICSE、FSE 等相关方向，但不要预先承诺投稿层级或录用概率。

## 5. 任务 C：低成本实验可行性

我们可能借用实验室服务器，服务器上据称部署了一款约 30B 参数的 Qwen 模型。

当前尚未确认具体型号和访问权限。

可能涉及 Qwen3-30B-A3B、Qwen3-Coder-30B-A3B、Qwen3-32B 或其他变体。

### 5.1 模型适配性

调查：

- 哪些 Qwen 30B/32B 变体适合多轮 Coding Agent 实验？
- 不同模型的工具调用能力有何区别？
- 是否支持读取隐藏层激活？
- 如果只能使用兼容 OpenAI API 的服务，能够完成哪些实验？
- 是否需要模型权重访问才能进行 Activation Steering？
- 模型量化、MoE 结构、推理框架会如何影响内部分析？
- 应该使用 vLLM、SGLang、Transformers，还是其他工具？

不要假设「服务器上部署了模型」就意味着我们拥有模型权重访问权限。

请列出向实验室管理员确认的最小信息清单，包括模型名称、量化方式、GPU 型号与显存、推理框架、API / 权重权限、资源配额等。

### 5.2 最小可行实验（MVP）

设计一个尽可能轻量的 L1 行为预实验。

初步设想：

- 选择约 10–20 个可自动判定结果的小型编程任务。
- 设置正常环境、高失败成本环境等实验条件。
- 使用同一 Agent Harness、工具集与运行预算。
- 收集每次工具调用、状态变化、验证操作与最终结果。
- 每组任务进行多次独立运行。
- 采用配对比较和适当的统计分析。

以上数字仅为初步预算，请根据统计效能、模型能力和计算成本评估是否合理。

实验设计必须明确：

1. 如何定义并自动识别过度验证。
2. 如何区分合理的重复检查和冗余检查。
3. 如何控制模型能力、任务难度、提示词长度与工具可靠性。
4. 如何让失败成本成为可信的环境约束，而不只是情绪化措辞。
5. 如何保证不同条件下工具调用机会和预算可比。
6. 如何防止模型仅仅因为提示词变长而增加行动。
7. 如何控制随机种子、temperature、上下文长度及最大执行步数。
8. 如何通过状态快照或信息增益判断某次验证是否真正冗余。
9. 如何验证模型是否已经具备完成基准任务的能力。
10. 如何避免执行真实危险操作或破坏服务器文件。

尤其关注指标设计。

可考虑：

- Redundant Verification Rate
- Repeated Tool Call Rate
- Verification Cost
- Task Success Rate
- Verification Utility
- Latency / Token Overhead
- Failure Recovery Efficiency

不要简单认为验证次数越少越好。

请设计合理的指标定义，并讨论自动评判与人工标注的一致性问题。

### 5.3 第二阶段扩展实验

如 L1 出现可重复现象，提出研究压力解除后的行为残留的方案。

需要比较：

- 保留完整历史的同一会话。
- 对历史进行摘要或上下文压缩。
- 新任务但保留部分失败信息。
- 完全独立的新会话。
- 可能存在的外部持久记忆。

明确哪些实验只能证明上下文依赖，哪些结果才支持更强的机制性解释。

### 5.4 第三阶段机制实验

如果开放权重及算力条件允许，评估：

- Contrastive Activation Analysis
- Linear Probes
- Activation Steering
- Activation Patching

尝试识别 risk、uncertainty、failure、caution、desperation 等相关概念表征，但不要假设这些概念对应独立的神经元或唯一线性方向。

必须说明：

- 表征如何提取。
- 如何验证跨任务泛化。
- 如何控制语义泄漏。
- 如何做随机方向或无关方向对照。
- 如何通过干预而非相关性证明因果作用。
- MoE 模型是否增加了实验难度。

## 6. 任务 D：风险评估与证伪标准

请主动从反面论证，列举至少 5 个可能导致研究不值得继续的因素。

重点分析：

- 现象是否可能仅来自商业产品的 Agent Harness？
- Qwen 30B 的编程能力是否足以支撑稳定实验？
- 模型是否可能压根不会出现过度验证？
- 研究问题是否已经被 2025–2026 年的相关论文覆盖？
- 压力干预能否与普通提示词影响区分？
- 是否难以设计客观、可复核的冗余行为指标？
- 内部表征是否只能预测行为，而无法解释行为？

为每项主要假设提出明确的反例、证伪条件和实验停止标准。

请不要将不显著结果包装成成功。

## 7. 最终交付物

请在当前工作目录创建 `research_feasibility/`，输出下列 Markdown 文档：

**01_Literature_Review.md**

相关研究综述、文献矩阵、关键论文精读、引用及源链接。

**02_Novelty_and_Research_Gaps.md**

创新性分析、最接近的已有工作、具体 Research Gaps、备选论文路线。

**03_Experimental_Feasibility.md**

Qwen 模型适配性、服务器要求、黑盒与白盒实验可行性、实验风险。

**04_MVP_Experiment_Proposal.md**

可供下一阶段实施的最小实验设计，包括任务、控制组、处理组、指标、日志结构、统计分析、资源估算与停止条件。

**05_Go_NoGo_Assessment.md**

以批判性角度总结：

- 哪些已有事实支持进一步研究？
- 哪些核心假设尚未得到证据？
- 哪些技术或资源条件是硬门槛？
- 哪条研究路线最值得优先尝试？
- 哪些发现会促使我们终止或转向课题？

最终给出明确判断：

**GO / CONDITIONAL GO / PIVOT / NO-GO**

附上理由、证据等级和下一阶段最值得回答的 3 个问题。

另提供一份不超过 1000 字的中文 Executive Summary，方便我拿去和师兄或导师讨论。

## 8. 执行原则

1. 本次优先完成文献调查、可行性评估和实验设计，不要直接开发完整实验框架。
2. 可以检索互联网和公开代码仓库，但不要擅自访问实验室服务器、部署模型、下载大型权重或运行昂贵实验。
3. 重要结论必须有可追溯来源。
4. 对无证据或证据不足的判断明确标注不确定性。
5. 对学术创新性采取保守态度，主动搜索反例。
6. 区分模型层、Agent Harness 层和系统环境层的行为来源。
7. 区分推理阶段施加的任务压力与训练过程中真实的惩罚信号。
8. 不将语言中的痛苦表达解释为模型存在主观痛苦。
9. 优先考虑能够在有限 GPU 资源下复现、具有明确工程意义的研究问题。
10. 如果某项研究方向已被充分覆盖，请提出新的差异化切入点，而非强行维持原假设。

请首先开展文献调查，再根据实际文献结果动态调整研究问题和实验方案。

**最终目的不是证明我的想法正确，而是判断这个想法是否值得真正投入科研资源。**