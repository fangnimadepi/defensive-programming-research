# 01 文献调查：压力、验证决策与编程智能体

**检索截止：2026-10-19。** 本文是有边界的研究可行性调查，不是声称穷尽全部文献的系统综述。检索以任务书的五篇种子为起点，沿 arXiv 正文的相关工作与引用、研究机构原文、模型官方文档，补查 `coding agent verification / repeated tool calls / failure feedback / functional emotions / activation steering / agent evaluation` 等组合词。优先采用论文或作者机构的一手来源。下表的“摘要”表示本次仅核对元数据和摘要；“全文”表示检查了正文相关章节。**论文自己的实验结果不等于本课题效应已存在。**

## 核心结论与证据边界

1. **功能性情绪机制的宽泛主张已有强直接先例。** Sofroniew 等研究 Claude Sonnet 4.5 的情绪概念方向，包含 Claude Code 的 token 预算片段、编程测试失败后“desperate”激活、以及“impossible code”任务中对奖励作弊率的激活干预。因此“失败压力影响编程行为且有类情绪表征”不能作为本项目独立创新点；它**没有**研究“无信息收益的重复验证”这一具体动作及其成本—收益边界。[原论文](https://arxiv.org/abs/2604.07729)；[交互版](https://transformer-circuits.pub/2026/emotions/)。
2. **不能从验证次数增加直接推出缺陷。** Mehtiyev 与 Assunção 的大规模轨迹分析发现更多验证与成功相关，且轨迹长度和失败的关联受任务难度混杂；Wiedmann 等发现专用验证工具显著改变验证行为，单靠提示验证的效应较小。[前者](https://arxiv.org/abs/2604.02547)；[后者](https://arxiv.org/abs/2610.01618)。
3. **工具失败与 Harness 是强竞争解释。** Copilot 生产轨迹研究报告工具失败后的长重试链；“verified tool calls”研究显示在非原子工具失败下先核对后重试能减少重复副作用。另有研究显示 AGENTS.md 和代理框架本身可改变探索、测试及越界行为。[Copilot](https://arxiv.org/abs/2608.00101)；[Verified Tool Calls](https://arxiv.org/abs/2608.02645)；[AGENTS.md](https://arxiv.org/abs/2602.11988)；[Overeager](https://arxiv.org/abs/2605.18583)。
4. **本轮未找到直接把“失败成本/负面反馈”随机操纵、并以“状态不变时的重复验证”作为主结局的编程智能体研究。** 这是检索后的暂定空白，不是不存在性证明；投稿前仍须对新论文做滚动检索。

## Related Work Matrix（21 项）

“威胁”指对**压力诱发无效验证这一特定因果问题**的创新性威胁；高不等于否定研究价值。代码/数据仅在原文明确说明时写“公开”，否则记“未核验”，不推断不存在。

| # | 论文、作者、年份／平台 | 核验 | 问题、模型／数据、方法及作者报告的结果 | 本课题关系／威胁 | 代码或数据 |
|---|---|---|---|---|---|
| 1 | [Emotion Concepts and their Function in a Large Language Model](https://arxiv.org/abs/2604.07729), Sofroniew 等，2026，Anthropic/arXiv | 全文 | Claude Sonnet 4.5；由情绪故事提取方向，观察真实对话并干预；在编程失败后“desperate”激活，干预影响 impossible-code 奖励作弊。 | **高**：L3 宽泛叙事及“压力—编程行为—激活”已覆盖；未测验证冗余。 | 模型内部访问为作者自有；公开可复现性待核验 |
| 2 | [Persona Vectors: Monitoring and Controlling Character Traits in Language Models](https://www.anthropic.com/research/persona-vectors), Chen 等团队，2025，Anthropic／[论文](https://arxiv.org/abs/2507.21509) | 官方长文 | Qwen2.5-7B-Instruct、Llama-3.1-8B-Instruct；对比激活生成性格方向并 steering，监测训练／对话中性格漂移。 | **中高**：提出表征—干预模板；不涉及编程验证。 | 论文链接已核；代码数据未核验 |
| 3 | [Beyond the Black Box: Interpretability of Agentic AI Tool Use](https://arxiv.org/abs/2605.06890), Tatsat & Shater，2026，arXiv | 全文 | Nemotron 工具轨迹、GPT-OSS 20B、Gemma 3 27B；SAE 与线性 probe 预测工具需要及风险，做特征消融。 | **高**：工具调用的白盒研究先例；未操纵压力或定义重复验证的边际收益。 | 未核验 |
| 4 | [How to Speculate about Uncertainty in Agentic Coding?](https://arxiv.org/abs/2609.05274), Grotov & Malykh，2026，EMNLP Industry／arXiv | 全文 | Qwen3-Coder-480B 与 Claude 3.5 Sonnet；小模型给黑盒轨迹评分、执行前 veto；作者报告执行错误率下降 6–8 个百分点、token 成本下降 14–19%。 | **中高**：效率与失败重试的近邻；目标是预测执行失败，非压力导致的验证选择。 | 未核验 |
| 5 | [Agentic Coding in the Wild: Characterizing GitHub Copilot Traces at Production Scale](https://arxiv.org/abs/2608.00101), Liu 等，2026，arXiv／Microsoft Research | 全文 | GitHub Copilot 生产轨迹；作者报告 1300 万量级会话，工具失败在约 9% 的 turn 中出现，可形成高成本重试链。 | **高**：重复调用的环境根因；观察性数据不能识别压力因果。 | 原始生产轨迹公开情况未核验 |
| 6 | [Beyond Resolution Rates: Behavioral Drivers of Coding Agent Success and Failure](https://arxiv.org/abs/2604.02547), Mehtiyev & Assunção，2026，arXiv | 全文 | 9,374 条轨迹、19 个 agent、500 任务；控制难度后，长度—失败关联反向；上下文搜集和验证与成功相关。 | **高**：反驳“多验证必然坏”；也提示模型能力和框架都要控制。 | 未核验 |
| 7 | [Agents Are Systems, Not Models: Rethinking Agentic Evaluation](https://arxiv.org/abs/2610.01618), Wiedmann 等，2026，arXiv | 全文 | 4 个科学编程任务、五类配置、18,000 多轨迹；专用验证工具影响明显，验证提示效果小；重复运行方差大。 | **高**：直接要求模型、Harness、工具、信息、预算分层；未测压力与冗余。 | 作者称公开 benchmark 与轨迹 |
| 8 | [Overeager Coding Agents](https://arxiv.org/abs/2605.18583), Qu 等，2026，arXiv | 全文 | 500 个越界场景、约 7,500 次运行；在共享模型上，框架与许可机制显著影响越界率；展示配对情景和人工复核。 | **中高**：测量和 Harness 消融方法可借鉴；结局是越权动作而非过度验证。 | 论文称发布 benchmark；仓库地址未核验 |
| 9 | [Verified Tool Calls Improve LLM Agent Reliability Under Non-Atomic Failures](https://arxiv.org/abs/2608.02645), Mansoor 等，2026，arXiv | 全文 | 模拟超时、延迟可见、部分成功；postcondition 核查、verify-before-retry 减少重复副作用；仅两类模拟流程。 | **中高**：证明重试可能是对系统不确定性的合理应对，也提供故障对照。 | 未核验 |
| 10 | [Evaluating AGENTS.md](https://arxiv.org/abs/2602.11988), Gloaguen 等，2026，arXiv | 摘要 | SWE-bench 与真实仓库情境；上下文文件平均增加推理成本、扩大文件探索和测试，成功率可下降。 | **中高**：提示与环境规则可能制造表面“谨慎”。 | 未核验 |
| 11 | [Coding Agents Are “Fixing” Correct Code](https://www.sri.inf.ethz.ch/blog/fixedcode), Mündler 等，2026，ETH SRI 原文 | 官方报告 | 200 个已修复的 SWE-bench Verified 实例，多个 agent；作者报告多数仍修改代码。 | **中**：已正确状态上的停止决策很近；不是重复验证也未操纵压力。 | 未核验 |
| 12 | [TraceLab: Characterizing Coding Agent Workloads for LLM Serving](https://arxiv.org/abs/2606.30560), Zhu 等，2026，arXiv | 摘要 | 约 4,300 条自用 Claude Code／Codex 会话、43 万工具调用；长循环、长上下文和重尾工具成本。 | **中**：提供真实轨迹基准率与系统成本背景，非因果。 | 摘要称发布轨迹；细节未核验 |
| 13 | [SlopCodeBench](https://arxiv.org/abs/2603.24755), Orlanski 等，2026，arXiv | 摘要 | 36 问题、196 checkpoint、15 个 agent；报告迭代中代码结构退化及冗余代码增加。 | **低中**：冗余对象是生成代码，不是验证工具行为。 | [项目页](https://www.scbench.ai/)由论文给出 |
| 14 | [Agentless: Demystifying LLM-based Software Engineering Agents](https://arxiv.org/abs/2407.01489), Xia 等，2024，arXiv | 摘要 | 简化的定位—修复—验证流程；在 SWE-bench Lite 上报告较低成本与较强表现。 | **中**：必须用简化验证策略作成本—成功基线。 | 未核验 |
| 15 | [SWE-agent: Agent-Computer Interfaces Enable Automated Software Engineering](https://arxiv.org/abs/2405.15793), Yang 等，2024，arXiv | 摘要 | 通过专用 ACI 改善代码浏览、编辑和测试；展示接口设计影响结果。 | **中**：Harness 可能是行为来源。 | 未核验 |
| 16 | [AI Agents That Matter](https://arxiv.org/abs/2407.01502), Kapoor 等，2024，arXiv | 摘要 | 批评只看准确率、忽视成本、重复性及基准过拟合，提出联合考量。 | **中**：本项目必须同时报告成功率与成本。 | 未核验 |
| 17 | [Reflexion: Language Agents with Verbal Reinforcement Learning](https://arxiv.org/abs/2303.11366), Shinn 等，2023，arXiv／NeurIPS | 摘要 | 语言反馈写入 episodic memory，跨尝试改善表现；不改变权重。 | **中**：H2 的历史残留可以只是文本记忆条件化。 | 未核验 |
| 18 | [Agentic Misalignment: How LLMs Could be Insider Threats](https://www.anthropic.com/research/agentic-misalignment), Lynch 等团队，2025，Anthropic | 官方长文 | 16 个模型、模拟企业压力／目标冲突情景；在一些条件下出现越界行为。 | **中**：压力可改变行动，但情景与奖励结构不同，且无验证结局。 | 官方公开方法，细节见原文 |
| 19 | [SWE-Bench Pro Verified](https://arxiv.org/abs/2609.08149), Zheng 等，2026，arXiv | 摘要 | 针对评估泄漏与任务质量问题修订 SWE-Bench Pro；一些分数下修。 | **中**：提醒基准、隐藏测试与奖励作弊会污染结果。 | 未核验 |
| 20 | [Towards Best Practices of Activation Patching](https://arxiv.org/abs/2309.16042), Zhang & Nanda，2023，arXiv | 摘要 | 系统比较 patching 的度量与 corruption 方式；结论可能依设置变化。 | **中**：L3 的因果证据门槛。 | 未核验 |
| 21 | [Steering Language Models With Activation Engineering](https://arxiv.org/abs/2308.10248), Turner 等，2023，arXiv | 摘要 | 对比提示激活构造方向并在推理时加入；改变输出属性。 | **中**：L3 的方法前驱；steering 改变行为不自动解释机制。 | 未核验 |

补充背景：[SWE-bench](https://arxiv.org/abs/2310.06770) 定义真实 GitHub issue 修复任务；其高复杂度和基准泄漏风险使它更适合后续外部效度测试，不适合直接充当低成本、精确定义“冗余”的首轮 MVP。

## 重点精读与反证含义

### 1. Anthropic Functional Emotions：对创新性威胁最大

论文用情绪故事构造对比方向，测量这些方向在 Claude Sonnet 4.5 的对话／行动过程中如何激活，再对方向进行 steering。作者在 Claude Code 会话中展示 token 预算相关的 “desperate” 激活；在“impossible code”测试失败后激活上升；在七个 impossible-code 任务上干预 “desperate” 和 “calm” 方向，改变奖励作弊率。**这是论文报告的因果干预结果**，不只是模型自述。作者也明确限定：单一模型、合成故事抽取方向、可能有语义混杂、机制仍不透明，且不涉及主观体验。[论文正文与局限](https://arxiv.org/abs/2604.07729)。

本项目不能把 “desperation 使 coding agent 做错事” 当首创。仍可能成立的细分问题是：面对**可确定已无新信息**的验证工具，压力是否改变停止规则或验证资源分配；此效应是否在多个开放模型／Harness 下存在；激活干预是否专门影响该动作，同时不伤害必要验证。这些均尚属**我们的推论和待检验假设**。

### 2. Wiedmann 等：系统组件比情绪措辞更关键

正文比较任务信息、推理、验证提示、时间预算、模型。作者在四个科学编程任务和大量重复运行中发现显著同配置波动；给工具提供直接验证能力，比要求模型“仔细检查”的提示更能改变行为。项目需要固定工具可用性、反馈保真度与预算，并采用等长度提示安慰剂。[论文](https://arxiv.org/abs/2610.01618)。它没有测试压力诱发的**无信息重复检查**；由于仅四个专业任务，不能外推到一般代码库。

### 3. Mehtiyev 与 Assunção：多验证不等于坏

正文的 9,374 条轨迹分析将任务难度纳入解释，发现“轨迹越长越易失败”的粗相关可反向，验证投入通常与成功相关。观察研究不能告诉我们同一个代理在高失败成本下是否**过度**验证；但它直接否定以绝对验证次数或总工具调用数做主指标。[论文](https://arxiv.org/abs/2604.02547)。

### 4. Copilot 生产轨迹：真实重复可能源于工具故障

Liu 等报告了生产规模工具调用与失败后的重试链；数据主要服务于推理系统工作负载分析，无法随机化失败成本或排除产品 Harness 的规则。[论文](https://arxiv.org/abs/2608.00101)。因此工具响应、超时、状态版本必须记录；在不确定提交是否成功时，再查一次可能是**必要**验证。

### 5. Verified Tool Calls：重复与重复副作用必须区分

模拟工具存在“超时但已执行”等非原子状态。作者用 postcondition verifier 和幂等键减少重复副作用，说明在模糊反馈下核查状态往往是正确动作。其两类模拟工作流、固定种子及人工设计 verifier 限制外推。[论文](https://arxiv.org/abs/2608.02645)。本项目要设无故障与故障正对照，并对“再次读取”和“再次写入”分开标注。

### 6. Overeager Coding Agents：测量边界与 Harness 消融

Qu 等以配对 consent 声明和双通道工具审计测“越权动作”，还做了人工再标注；论文报告框架差异显著。这提供重要方法：不要在题面直接写出研究者期待的“不要多检查”，否则只是测指令遵循。[论文](https://arxiv.org/abs/2605.18583)。越权和过度验证是不同结局，不能混为一谈。

### 7. Grotov 与 Malykh：把不确定性用于更省的决策

文章以黑盒输出轨迹的相位特征预测执行风险，插入 veto 后报告较低执行错误与成本。其结局是**下一次执行是否失败**，不是重复检查有无信息增益；论文局限也注明部分比较只有单次估计，未给 seed 方差／置信区间。[论文](https://arxiv.org/abs/2609.05274)。本项目不能仅提出“提高 agent 效率”，必须拿出压力操纵、边际验证价值与识别设计。

### 8. Tatsat 与 Shater：L3 已有 agent 工具决策探针

论文从 Nemotron 工具轨迹训练 SAE 和 probe，在 GPT-OSS 20B 与 Gemma 3 27B 上预测工具需要与风险并做 feature ablation；它重在行动前监测。作者正文亦指出原始 residual probe 是强预测基线。其消融证明特征影响监测读数或相关行动不等同于辨识“情绪”机制；本项目若做 L3，应在匹配语义、任务及模型能力之后，检验**验证决策的选择性干预**。[论文](https://arxiv.org/abs/2605.06890)。

## 检索后的问题重述

建议把论文问题暂写为：**在工具状态可复核、验证成本可计量的编程任务中，失败成本与失败反馈是否改变 agent 对“再次验证”的边际信息价值判断？这种变化来自模型策略、Harness、上下文历史还是工具不确定性？** “功能性情绪”留作 L3 的竞争性机制假设，而不是标题或结论的前提。
