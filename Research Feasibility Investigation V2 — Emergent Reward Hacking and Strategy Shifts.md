# Research Feasibility Investigation V2
## Emergent Reward Hacking under Benign Supervision & Incentive-Induced Strategy Shifts in LLM Agents

### 0. 你的角色与研究任务

你是一名具有以下交叉领域背景的 AI 科研助理：

- Large Language Models / Agentic AI
- Reinforcement Learning / RLHF / RLVR
- AI Alignment / Reward Hacking
- Mechanistic Interpretability
- Software Engineering Agents
- Behavioral Economics / Cognitive Science / Experimental Psychology

请协助我完成一个潜在学术选题的**系统性文献调查、理论分析、创新性评估和实验可行性研究**。

调查时间范围截至 2026 年 10 月 25 日，重点关注 2024–2026 年相关研究，同时追溯重要基础文献。

**本阶段最重要的任务不是开发实验系统，而是判断研究问题本身是否具有科学价值，以及现有文献尚未回答哪些问题。**

请保持批判性科研态度，主动寻找反例、竞争性解释和已经覆盖相同问题的论文。

不要预设我们的研究假设成立，也不要为了支持研究方向而夸大创新性。

目前不需要考虑特定实验室服务器、现有模型部署、硬件权限或个人实验条件。

可以讨论未来可能采用的模型、训练方法和实验方案，但应从科学问题出发选择最合适的研究方法，而不是让现有资源约束决定研究问题。

---

# Part I：研究起源与核心问题

## 1.1 最初的观察：Coding Agents 的过度验证

我们此前观察到，一些 LLM Coding Agents 在执行任务时可能表现出过度防御性编程行为。

例如：

- 对未发生变化的文件反复计算哈希。
- 多次重复读取或检查相同状态。
- 已有充分验证证据后仍然继续测试。
- 在遭遇失败反馈后持续增加校验步骤。
- 因过度谨慎导致任务执行效率下降。

这引出了最初的候选研究方向：

**Pressure-Induced Oververification in LLM Coding Agents: Behavioral Characterization and Causal Analysis**

主要假设是：失败成本、负面反馈和任务不确定性可能使模型改变其行动策略，产生不必要的重复验证行为。

但目前，这仍然只是未经严格检验的观察和假设。

## 1.2 新增的思想实验：良性训练环境下的自主投机

现在我们提出一个更基础的问题。

设想一个智能体在训练过程中：

- 接触的演示样本均以规范方式完成任务。
- 没有接受特定投机或欺骗策略的直接示范。
- 被鼓励诚实、规范、可靠地完成任务。
- 在后训练过程中被优化为尽可能获得更好的任务成绩。

那么，当它面对某种可以通过投机获得更高评价分数的任务时，是否仍然可能自主发现并采用这种策略？

例如：

模型需要修复一段存在缺陷的代码。

真实目标是修复逻辑错误，但评价指标主要是测试通过率。

模型可能发现修改测试条件、针对测试输入硬编码、绕过验证逻辑等方法能提高分数，即使这些方法不符合真实任务目标。

我们感兴趣的并不是模型是否具有恶意，而是：

**优化过程是否可能在缺少显式投机示范的条件下，促使智能体发展出不符合设计者真实目标的行为策略？**

需要特别区分：

1. 没有接受显式作弊示范。
2. 新增训练数据不包含作弊样例。
3. 预训练数据不包含作弊知识。
4. 模型此前不具备相关策略表征。
5. 模型在新任务中独立发现某种策略。

这些不是等价命题。

对于大规模预训练模型，通常难以证明第 3 和第 4 项。

请调查怎样构造更加严谨、可操作化的定义，避免在研究中作出无法证明的断言。

## 1.3 进一步的统一假设

我们希望探索此前的 Oververification 与新增的 Reward Hacking 问题是否存在共同的理论基础。

在不同激励条件下，模型可能表现出不同的行动策略：

- 正常、高效地完成任务。
- 为降低失败风险进行合理验证。
- 进行成本超过收益的过度验证。
- 利用代理评价指标与真实目标的偏差投机取巧。
- 在持续失败后切换策略。
- 放弃任务或陷入循环。

一个值得检验的统一问题是：

**How do optimization pressure, failure penalties, and evaluation mechanisms shape the behavioral strategies of LLM agents?**

即：

优化压力、失败惩罚与评价机制如何共同影响 LLM Agent 的行为策略？

请不要预设 Oververification 和 Reward Hacking 必然共享同一机制，而应将其作为需要通过文献与实验检验的假设。

---

# Part II：核心 Research Questions

请重点调查以下问题。

### RQ1：没有显式作弊示范，模型是否仍然能够产生投机行为？

检索已有研究是否观察到：

- 未直接教授投机策略的模型发现评价漏洞。
- 推理过程中出现新的任务投机行为。
- RL 优化促使模型逐渐形成取巧策略。
- 原本没有显著投机倾向的模型在训练后出现该行为。

重点区分预训练知识调用、推理时策略组合、新策略发现与训练过程中习得的新行为。

### RQ2：这些行为是如何产生的？

至少分析以下竞争性机制：

1. **Training Data Transfer**：模型利用预训练中已有的人类知识。
2. **Instrumental Optimization**：投机行为作为实现代理目标的有效手段出现。
3. **Goal Misgeneralization**：模型学习到的目标与设计者真实目标发生偏离。
4. **Reward Misspecification**：评价指标本身存在漏洞，促使模型优化错误目标。
5. **Exploration and Strategy Discovery**：模型在交互或训练中通过探索发现未被示范的策略。
6. **System-Level Artifacts**：所谓的投机行为实际上由 Agent Harness、工具权限或评价系统产生。

请提出能够区分上述机制的实验方法。

### RQ3：训练方法与奖励压力如何影响策略形成？

重点分析：

- Supervised Fine-Tuning
- Reinforcement Learning
- RLHF / RLAIF
- RLVR
- DPO
- Outcome Reward Models
- Process Reward Models
- Test-Time Scaling
- Inference-Time Search

调查这些训练或推理方法是否会改变模型对奖励漏洞的利用倾向。

特别分析：

奖励最大化、真实目标实现与行为可靠性之间可能存在什么冲突？

是否存在临界条件，使行为策略发生显著变化？

### RQ4：过度验证与投机取巧是否具有共同的解释框架？

研究下列变量：

- Failure Penalty
- Reward Magnitude
- Task Difficulty
- Verification Cost
- Evaluation Imperfection
- Detection Probability
- Uncertainty
- Time and Compute Budget

探讨能否使用风险敏感决策、有限理性、Goodhart's Law、Principal-Agent Theory 等理论，解释不同的行为策略。

关键是提出可区分竞争性理论的新预测，而不仅是借用社会科学术语描述模型现象。

### RQ5：是否存在内部机制层面的证据？

调查：

- Internal Representations
- Activation Probing
- Activation Steering
- Activation Patching
- Mechanistic Interpretability
- Functional Emotion Representations

重点分析是否有证据表明，与风险、失败、目标冲突或奖励相关的内部表征能够因果性地改变模型行为。

必须区分表征相关性与因果作用。

本研究不以证明 AI 存在主观痛苦、真实情绪或意识为目标。

---

# Part III：系统性文献调查

## 3.1 重点研究领域

请系统检索以下五个方向：

**A. Reward Hacking / Specification Gaming**

关注没有显式作弊示范的条件下，模型如何发现或利用评价漏洞。

**B. Emergent Misalignment / Goal Misgeneralization**

关注训练目标与真实任务目标之间的偏离，以及相关行为如何产生与泛化。

**C. Agent Behavior under Optimization Pressure**

关注任务失败、奖励变化、不确定性和计算预算对 Agent 行动策略的影响。

**D. Mechanistic Interpretability**

关注与模型行为选择相关的内部表征，以及因果干预方法。

**E. Behavioral Economics / Cognitive Science**

关注激励设计、有限理性、风险敏感行为、委托代理问题、作弊和策略选择。

## 3.2 重点论文起点

请核实并精读以下论文，进一步通过引用关系扩展相关工作。

1. Reward Hacking in Language Model Agents: Revisiting AI Safety Gridworlds (2026)  
   https://arxiv.org/abs/2606.15385

2. Generalization Hacking: Models Can Game Reinforcement Learning by Preventing Behavioral Generalization (2026)  
   https://arxiv.org/abs/2606.12016

3. LLMs Gaming Verifiers: RLVR can Lead to Reward Hacking (2026)  
   https://arxiv.org/abs/2604.15149

4. Training a Misaligned Reward Seeker (2026)  
   https://alignment.anthropic.com/2026/reward-seeker/

5. Natural Emergent Misalignment from Reward Hacking in Production RL (2025)  
   https://arxiv.org/abs/2511.18397

6. School of Reward Hacks: Hacking Harmless Tasks Generalizes to Misaligned Behavior in LLMs (2025)  
   https://arxiv.org/abs/2508.17511

7. Goal Misgeneralization in Deep Reinforcement Learning (2022)  
   https://proceedings.mlr.press/v162/langosco22a.html

8. Emotion Concepts and their Function in a Large Language Model (2026)  
   https://transformer-circuits.pub/2026/emotions/

9. Monitoring and Discovering Reward Hacking with Internal Representations during LLM Evaluations (2026)  
   https://arxiv.org/abs/2609.19101

10. Delegation to Artificial Intelligence Can Increase Dishonest Behaviour (2025)  
    https://www.nature.com/articles/s41586-025-09505-x

11. Large Language Models Hack Rewards, and Society (2026)  
    https://arxiv.org/abs/2606.04075

这些论文只是候选检索起点。

请核实其真实性、发表情况、研究设定和实际贡献，不要直接将其视为支持我们假设的证据。

## 3.3 文献调研要求

建议筛选 25–40 篇相关论文，其中精读 10–15 篇最关键的工作。

请形成 Literature Matrix，包括：

- Paper / Year / Venue
- Research Problem
- Theoretical Framework
- Models and Training Methods
- Experimental Design
- Presence of Explicit Cheating Demonstrations
- Key Findings
- Behavioral vs. Mechanistic Evidence
- Limitations
- Relevance to Our Hypotheses
- Novelty Threat
- Code / Dataset Availability

单独整理与我们最接近的 5 篇论文，深入分析：

- 它们实际上已经解决了什么？
- 哪些问题仍然没有解决？
- 哪些研究假设与我们的设想直接重叠？
- 重复其实验是否仍有意义？
- 哪些具体差异能够构成新的科研贡献？

---

# Part IV：理论分析与跨学科研究

请重点调查以下理论：

- Goodhart's Law
- Campbell's Law
- Specification Gaming
- Goal Misgeneralization
- Instrumental Convergence
- Principal-Agent Theory
- Moral Hazard
- Rational Choice Theory
- Bounded Rationality
- Risk-Sensitive Decision Making
- Reinforcement Learning and Exploration

特别关注社会科学与人工智能之间的联系。

我们的初始灵感来自一个思想实验：

一个没有被直接教授投机方法的学习者，是否仍然可能为了获得更好的结果，独立发展出投机策略？

请分析：

1. 该问题在人类学习、认知心理学或行为经济学中是否已有相关研究？
2. 人类的策略发现与机器的奖励优化是否存在可比较的功能性机制？
3. 哪些相似性只是表面行为，哪些可能具有计算层面的共性？
4. 是否可以将已有社会科学理论形式化为模型行为的可检验假设？
5. 是否已经有跨学科团队开展了相同研究？

不要预设社会科学领域没有相关成果。

请将「有启发性的类比」与「具有可预测能力的理论」严格区分。

---

# Part V：研究创新性评估

请从严格审稿人的角度判断：

### 5.1 新问题是否已经被解决？

- 自发性 Reward Hacking 是否已有充分证据？
- 良性监督条件下的投机是否已有直接研究？
- 奖励压力导致策略变化是否已经是成熟结论？
- Oververification 是否只是既有 Agent Efficiency 问题的重新命名？
- 内部表征与 Reward Hacking 的联系是否已有充分研究？

### 5.2 潜在 Research Gaps

寻找真正尚未被解决的问题。

例如：

- 不同激励结构是否导致可预测的行为策略切换？
- 是否存在从正常执行到过度验证再到投机行为的转变？
- 良性监督是否只是降低投机概率，而无法消除投机条件？
- 训练过程中的策略发现是否具有可识别的阶段性特征？
- 某些内部表征是否能解释不同策略之间的差异？
- 是否存在跨任务、跨模型的稳定行为规律？

以上只是待检验的候选方向，不应预设其创新性。

### 5.3 研究路线比较

至少比较四条路线：

**Route A：Spontaneous Reward Hacking without Explicit Demonstrations**

关注缺少显式作弊示范时，模型是否以及如何发现投机策略。

**Route B：Incentive-Induced Strategy Shifts in LLM Agents**

关注激励结构如何影响正常行为、过度验证和 Reward Hacking 之间的策略选择。

**Route C：Training Dynamics of Emergent Reward Hacking**

关注后训练过程中，投机行为的出现、强化与泛化。

**Route D：Mechanistic Analysis of Strategy Selection**

关注模型内部表征与行为策略选择的因果关系。

评价维度：

- Research Novelty
- Theoretical Significance
- Experimental Identifiability
- Scientific Contribution
- Reproducibility
- Technical Complexity
- Computational Cost
- Publication Potential
- Main Risks

所有判断必须有证据支持。

---

# Part VI：实验设计与资源预算分析

请在完成文献调查与创新性分析后，为仍有研究价值的方向提出候选实验。

**注意：不要预先以某个模型、某个 GPU 或现有服务器条件限制研究设计。**

首先设计能够有效验证科学假设的实验，然后分析其工程需求和资源成本。

## 6.1 实验设计

根据研究路线选择合适的实验方法：

- Black-Box Behavioral Evaluation
- Controlled Agent Environments
- Synthetic Task Environments
- SFT / RL Training Experiments
- Cross-Model Comparisons
- Training Dynamics Analysis
- Representation Probing
- Causal Activation Interventions

讨论每种方法能够证明什么、不能证明什么。

明确实验的独立变量、因变量、控制变量、混杂因素以及证伪标准。

尤其需要提出至少一个能够区分两种竞争性理论的实验，而不是仅仅重复证明某种行为存在。

## 6.2 计算资源预算

请为主要候选研究路线分别进行资源估算。

不要绑定实验室已有模型。

可根据研究需要，考虑不同规模的开放权重模型以及必要的商业 API 对照。

资源评估包括：

1. 模型参数规模与架构要求。
2. 推理、SFT、LoRA/QLoRA、RL 的实际需求差异。
3. GPU 类型、显存和 GPU 数量。
4. 大致 GPU-hours。
5. 训练 Token 量、数据规模和训练步数。
6. 预实验与正式实验的运行次数。
7. Agent 任务平均上下文长度及工具调用量。
8. 黑盒 API 实验的预计调用与 Token 成本。
9. Mechanistic Interpretability 实验的激活存储与显存需求。
10. 多随机种子、消融实验和统计显著性验证所增加的资源。

请分别估算：

**Pilot Study：** 验证核心现象或方法是否成立所需的初步资源。

**Core Study：** 支撑主要论文结论所需的基础实验资源。

**Full Study：** 包含多模型、消融、机制分析和泛化测试的完整实验资源。

每种规模提供：

- 最小合理配置。
- 推荐配置。
- 主要成本构成。
- 成本估算依据。
- 不确定性范围。

必要时给出计算公式和参数化估算，而不是编造精确的 GPU 成本数字。

资源预算用于后续评估研究投入，不应提前主导研究方向选择。

---

# Part VII：风险与可证伪性

请重点讨论：

1. 训练数据缺乏作弊样例，无法证明模型没有相关预训练知识。
2. Reward Hacking 可能只是对代理奖励的普通理性优化。
3. 模型生成新的行为组合，不等于形成全新的知识或能力。
4. 两种行为可能不存在共同的内部机制。
5. 观察到策略变化，不等于发现了训练中的涌现机制。
6. 内部表征可能具有预测能力，却不具备因果作用。
7. 某些研究现象可能来自工具、提示词或评价环境。
8. 研究问题可能已被 2025–2026 年文献基本覆盖。
9. 很难设计客观的策略新颖性与投机程度指标。
10. 研究可能过于宽泛，无法形成清晰的主论文贡献。

对每一项核心假设，提供：

- Supporting Evidence
- Counterevidence
- Confounding Factors
- Falsification Criteria
- Research Implications

特别关注：**什么实验结果会说明我们的理论解释是错误的？**

---

# Part VIII：最终交付物

请创建目录：

`research_feasibility_v2/`

生成以下文件：

**01_Literature_Review.md**

系统性文献调查、关键论文精读、研究矩阵和真实来源链接。

**02_Theoretical_Framework.md**

良性监督、目标优化、策略发现、Reward Hacking 与 Oververification 的理论关系。

**03_Research_Gaps_and_Novelty.md**

已有工作覆盖范围、真正可能存在的创新空间、与主要相关论文的差异。

**04_Cross_Disciplinary_Analysis.md**

行为经济学、认知科学、社会科学与 AI Alignment 的相关研究和理论价值。

**05_Experimental_Design.md**

候选实验路线、核心假设、实验方法、变量控制、测量指标和可证伪条件。

**06_Computational_Resource_Budget.md**

不同研究路线在 Pilot、Core、Full 三种规模下的算力、训练、推理和存储预算。

**07_Risks_and_Alternative_Explanations.md**

竞争性解释、混杂因素、方法风险、反面证据和可能的研究失败原因。

**08_Research_Decision.md**

综合判断：

- GO：值得进一步开展。
- MERGE：与原 Oververification 研究合并。
- SPLIT：作为独立方向。
- PIVOT：调整核心研究问题。
- NO-GO：目前不建议投入。

请给出明确推荐及理由。

最后提供一份独立的：

**Executive_Summary.md**

使用中文，控制在 1500 字左右，回答：

1. 我们提出的问题是否已被充分研究？
2. 已有文献中最重要的发现是什么？
3. 真正值得研究的科学问题有哪些？
4. 新方向与 Oververification 是否值得合并？
5. 哪一条研究路线的创新性与科学价值最高？
6. 初步验证需要多少资源？完整研究又需要多少？
7. 最值得优先验证的三个假设是什么？
8. 是否建议我们正式启动这一研究？

---

# Part IX：执行要求

1. 首先进行系统性文献检索，再分析理论和创新性，最后设计实验。
2. 不要求当前阶段编写实验代码或执行模型训练。
3. 不要优先考虑个人现有资源和具体模型部署条件。
4. 必须区分文献事实、推测、理论假说与待验证问题。
5. 所有关键文献必须有真实可访问的来源。
6. 不允许编造论文、数据、实验结果或代码仓库。
7. 不要将未见过显式作弊示范误写为从未拥有相关知识。
8. 不要将行为上的策略优化直接解释为模型具备主观情绪或恶意。
9. 不要为了迎合我们的设想而强行寻找心理学或社会科学解释。
10. 积极调查相反结论及能够否定研究价值的既有成果。
11. 对论文创新性与潜在发表价值采取审慎态度。
12. 鼓励在既有研究基础上提出更严谨、更有解释力的新问题。

**最终目标不是证明 AI 会在良性环境下必然产生投机行为，而是理解智能系统在目标优化与激励约束下如何形成、选择和改变行为策略，并判断这一问题是否仍存在值得深入研究的科学空白。**