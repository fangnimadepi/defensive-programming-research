# 01｜重复验证的价值：聚焦调研与创新性复核

调查日期：2026-10-27。范围限定为低成本方案 A；编号 OV01–OV17 独立于旧报告的 L 编号。以下区分论文报告、项目自述与本次设计推论。没有复现这些论文的实验。

## 1. 检索与阅读范围

检索包括 `LLM agents redundant verification repeated tool calls`、`coding agents verification counterfactual`、`value of information stopping`、`agent-generated tests`、`oververification`、`prompt repetition`、`cost-optimal tool use`。使用公开网络检索定位，再查 arXiv 正文、ACL Anthology、TMLR 官方论文目录和作者仓库。围绕最接近论文扩展其引用和相邻工作。

这是**聚焦综述，不是穷尽性系统综述**，没有声称完整的数据库召回率或 PRISMA 筛选数量。Reddit 和聚合站只用于发现线索，不承担科学结论。正文可访问的工作主要阅读摘要、方法、关键结果和局限；未逐行核查全部附录或原始数据。下表单独标明访问限制。

两个版本问题值得保留：

- 检索得到匿名稿 *Do More Agent-Generated Tests Help LLM Code Agents?*，其 OpenReview 页面受浏览器验证限制。本报告用可读的 OV01 arXiv v2 为主要依据，不把相似匿名稿另算一篇独立证据，也不推定其录用状态。
- OV06 的搜索摘要仍展示早期四 episode 版本；作者仓库已是 v8.2，且 TMLR 官方目录列为 2026 年 8 月论文。因此不能用旧摘要的四例概括当前全部实验。下面对现有证据范围依据作者的最新版证据映射，避免混用版本。

## 2. 相关工作矩阵

| 编号、工作 | 已核查来源/范围 | 对本研究最重要的事实 | 创新威胁或限制 |
|---|---|---|---|
| OV01 Zhi Chen 等，2026，*Rethinking the Value of Agent-Generated Tests for LLM-Based Software Engineering Agents* | [arXiv v2](https://arxiv.org/abs/2602.07900v2)，正文 §2、§5、§6 | 六模型 SWE-bench 轨迹；四模型提示干预改变写测试行为并比较结果与成本 | **非常高**：已做“多测试是否值得”；干预是写测试提示，不是同一状态下旧/新证据替换 |
| OV02 Theodore Papamarkou 等，2026，*Bayesian Control for Coding Agents* | [正文](https://arxiv.org/html/2606.24453)，§3–6 | 六生成模型、九编码基准；以正确性信念、critic 和修复转移选择验证/修复/停止 | **非常高**：成本敏感验证和 VOI 已不是新理论；本项目需研究模型自身的选择偏差及旧证据效应 |
| OV03 Luis Wiedmann 等，2026，*Agents are systems, not models* | [正文](https://arxiv.org/html/2610.01618)，§4.3 与系统定义 | 科学任务中，验证提示与系统提供验证工具的效果不同 | 高：必须控制 harness，不能把配置效果归于模型压力 |
| OV04 Tural Mehtiyev、Wesley Assunção，2026，*Beyond Resolution Rates* | [正文](https://arxiv.org/html/2604.02547)，§3–5 | 9,374 轨迹、19 agents；控制任务后，长轨迹与失败的关系反转，验证努力与成功相关 | 高：不能用长轨迹或重复数量推导浪费；相关性不等于验证的因果收益 |
| OV05 Grace Liu 等，2025，*CaRT: Teaching LLM Agents to Know When They Know Enough* | [正文](https://arxiv.org/html/2510.08517)，§3–5 | 用最小信息变化构成继续/停止反事实对，训练终止策略；任务是诊断与数学 | 高：反事实信息充分性和学会停止均已有；不照搬训练路线 |
| OV06 Yueh Tang、Justin Zhan，2026，*Verify What Matters* | [TMLR 目录](https://www.jmlr.org/tmlr/papers/)、[官方仓库](https://github.com/tang03130313/verify-what-matters)、最新版证据映射；论文页面被验证阻挡 | 将局部错误概率与下游损失分开讨论验证预算；作者把现有量化证据限定为受控 sandbox | 高：失败后果进入验证价值已有；不能称本项目首次把风险和检查结合 |
| OV07 Yicheng Feng 等，2026，*Scores Are Not Decisions* | [正文](https://arxiv.org/html/2607.27083)，§3–7 | 用停止与继续收益差学习工具获取深度；主要是执行前选择工具集合 | 中高：成本阈值和停止收益差已有；尚不同于观察结果后重复调用同一检查 |
| OV08 Dipkumar Patel，2026，*Anytime Verified Agents* | [官方仓库](https://github.com/llmsresearch/AVA)、[TMLR 目录](https://www.jmlr.org/tmlr/papers/)；论文全文页面受限 | 作者框架按预算分配搜索、采样、验证，采用不确定性与选择性验证 | 高：不能把一般“自适应验证”称为新方法；具体效果未在本次独立核实 |
| OV09 SignalLayerLabs，MARGINAL，工程项目 | [README](https://github.com/SignalLayerLabs/Marginal/blob/main/README.md)，2026-10-27 读取 | 已按同一动作、状态未变和无新证据检测重复；公开 README 明示小样本不能支持性能归因 | **工程新意威胁高**；项目不等于压力机制的受控实验证据 |
| OV10 Yaniv Leviathan、Matan Kalman、Yossi Matias，2025，*Prompt Repetition Improves Non-Reasoning LLMs* | [正文](https://arxiv.org/html/2512.14982)，§1–2、附录概述 | 多个非 reasoning 模型中，重复输入可改善表现；有长度对照 | 关键反例：没有新增外部事实，仍可能改变模型计算和读取效果 |
| OV11 Jie Huang 等，2023/ICLR 2024，*Large Language Models Cannot Self-Correct Reasoning Yet* | [正文](https://arxiv.org/html/2310.01798)，§3–5 | 当时模型的内在自纠正不稳定；指出 oracle 停止和额外推理成本混杂 | 方法基线；不能把旧模型结果外推为当代模型永不受益 |
| OV12 Zhibin Gou 等，2023/ICLR 2024，*CRITIC* | [正文](https://arxiv.org/html/2305.11738)，工具消融与实验 | 外部工具反馈可改善自纠正，且不同任务/模型表现不同 | 有用验证的阳性依据，不支持“一律少查” |
| OV13 Isham K. Mansoor 等，2026，*Verified Tool Calls Improve LLM Agent Reliability Under Non-Atomic Failures* | [正文](https://arxiv.org/html/2608.02645)，§4–6 | 在注入非原子故障的环境中，后置状态验证与幂等处理改善可靠性 | 合理重复的反例：超时、延迟可见、部分提交不能算纯重复 |
| OV14 Jiayu Liu 等，2025/ACL 2026，*CostBench* | [ACL 正式页面](https://aclanthology.org/2026.acl-long.584/)、[正文](https://arxiv.org/html/2511.02734) | 旅行工具任务中评估动态成本与重规划，模型未必能做最优成本选择 | 经济理解需独立检查；非编码重复验证证据 |
| OV15 Kaikai Zhang 等，2026，*Cheap to Hypothesize, Costly to Verify* | [v4 正文](https://arxiv.org/html/2609.35909v4)，§3–5 | 漏洞搜索中的安全诱饵可消耗验证资源 | 已有验证预算浪费研究，但诱饵场景不同于有效旧证据重查 |
| OV16 Renwei Meng，2026，*Policy-Conditioned Counterfactual Credit…* | [正文](https://arxiv.org/html/2606.05263)，§3.2 | 为不同步骤替换定义冻结后续策略下的反事实收益，服务于 RL | 反事实续跑本身并非新方法；必须说明本项目估计的是哪一种替换 |
| OV17 Daniël Lakens，2017，*Equivalence Tests: A Practical Primer…* | [作者机构保存的论文](https://pure.tue.nl/ws/portalfiles/portal/80918653/lakeequi2017.pdf)、[出版页面](https://doi.org/10.1177/1948550617697177) | “未发现显著差异”与“排除实质差异”是不同推断 | 支持事前设定等效边界；二项聚类结果需适配方法，不能直接套 t 检验 |

## 3. 八个最影响设计的发现

### 3.1 OV01：必须超过“测试频率干预”

其提示干预同时改变 agent 的计划、写测试、执行和调试路径；即使任务配对，也不是单次检查的局部效果。论文未发现显著结果差异，不等于完成了等效性论证。[来源](https://arxiv.org/html/2602.07900)

**本次设计推论：** 把实验起点固定为同一候选程序与证据记录，直接分配后续证据通道；另外保留自主行为试验，避免把强制给证据的价值误当成模型主动检查的价值。新研究应报告缓存回放和内部复查对照，而非只加一句“多写测试”。

### 3.2 OV02：VOI 与成本敏感停止只能做基线

该工作已用贝叶斯控制器处理正确性、critic 信息和修复带来的好坏转移，并展示简单策略也可能更优的条件。[来源](https://arxiv.org/html/2606.24453)

**本次设计推论：** 不再以“引入 VOI”作为贡献。可检验的增量是：冻结模型的实际选择与受控续跑得到的价值是否一致，且偏差是否随失败损失变化。第一版先用简单经验边际收益表；没有稳定信号，不建设复杂控制器。

### 3.3 OV05：同一前缀的继续/停止对已经有人做

CaRT 以信息变化构造停止标签，并通过 SFT 等方式训练模型。其反事实点的构造使用任务成功变化，服务于训练数据筛选。[来源](https://arxiv.org/html/2510.08517)

**本次设计推论：** 本项目检查点由任务生成规则预先固定，不能先挑“验证后会成功”的样本再估计平均验证收益。首轮只做冻结模型推理，任务生成器而非 LLM judge 提供真值。贡献需来自验证通道分解和风险响应，不是“构造反事实对”。

### 3.4 OV06：损失大小已经进入验证分配研究

最新版作者[证据映射](https://github.com/tang03130313/verify-what-matters/blob/main/docs/CLAIM_TO_EVIDENCE_MAP.md)限定了结论：部分收益置信区间跨零，结构损失代理不是校准后的真实损失估计，跨机制泛化有限。[范围说明](https://github.com/tang03130313/verify-what-matters/blob/main/docs/SCOPE_FOR_REVIEWERS.md)

**本次设计推论：** 不以“风险大时多检查”自称发现。重点看风险提高后是否仍选择已知旧证据的昂贵重执行，并检验真实收益能否解释。正式比较前需获取其当前最终论文；本次未声称读到被阻挡的完整最终稿。

### 3.5 OV09：缓存与重复检测也不是工程空白

MARGINAL 已明确区分重复与浪费，按状态和证据追踪重复操作。其 README 的早期三任务 smoke 没有解决任务、也没有实施 deny，所以作者自己不把观察到的 token 差归因于治理器；当前窄范围阻断也不能概括为所有测试已受控。[来源](https://github.com/SignalLayerLabs/Marginal/blob/main/README.md)

**本次设计推论：** 一个“重复调用拦截器”不够成为论文。缓存只是实验对照和潜在低成本修复。必须证明它在独立任务中保留必要验证，并量化误拦截风险；本轮不安装该项目、不改变用户的实际 agent 行为。

### 3.6 OV10：重复不一定没有认知价值

输入事实不变，也可能因重新呈现、位置与注意力变化改善输出。该论文的主结果针对非 reasoning 设置，不能直接代替编码 agent 实验。[来源](https://arxiv.org/html/2512.14982)

**本次设计推论：** 至少需要“内部多想一轮”“回放旧结果”“提供新结果”三种继续条件。观察到回放有益时，结论是旧证据重新读取有价值；不应强行保留“无信息所以无用”的解释。

### 3.7 OV03–04：行为数量与 agent 整体结果不能直接归因

工具可得性会改变验证，任务难度也会改变长轨迹与成功的关系。[系统配置](https://arxiv.org/html/2610.01618)、[任务混杂](https://arxiv.org/html/2604.02547)

**本次设计推论：** 相同任务状态、相同动作预算、完整输出和独立会话先于大样本。E1 的主终点放在第一次选择，避免“高损失组先产生不同补丁”造成后续状态不可比。整条轨迹统计仅作为另一层结果。

### 3.8 OV11–13：保护有用验证，同时避免 oracle 泄漏

外部反馈可能有益，工具故障后的核实可能必要；隐藏真值若参与在线停止则会改变研究对象。[内在自纠正审计](https://arxiv.org/html/2310.01798)、[外部反馈](https://arxiv.org/html/2305.11738)、[故障恢复](https://arxiv.org/html/2608.02645)

**本次设计推论：** 主实验无异步变动与故障，最终隐藏判分只在提交后执行；后续独立压力测试再加入过时证据。任务中保留有缺陷候选，避免所有候选都正确导致“不检查”天然最优。

## 4. 可以主张与不能主张的研究空间

| 主张 | 本次判断 |
|---|---|
| 更多验证可能浪费资源 | 已有，不能单独立题 |
| 用成本、损失、信息价值决定何时停止 | 已有理论与编码实证 |
| 按状态 hash 阻止重复调用 | 已有工程方案，单独贡献弱 |
| 反事实续跑估计步骤价值 | 已有，需要具体、正确的 estimand |
| 在同一编码状态下，分离额外计算、旧证据重呈现、新证据，再检验失败损失对自主选择的影响 | **值得验证的组合问题**，本轮未找到完整覆盖同一设计的工作；不声称绝对首创 |
| 高损失必然造成过验证/类情绪 | 当前无证据；实验不得预设 |
| 缓存与证据可见性解释了原观察 | 完全允许的结果；工程价值可能高于心理机制叙事 |

建议工作题目：**When Is Rechecking Worth It? Evidence Reuse, Failure Costs, and Verification Decisions in Coding Agents**。先不在标题中写 “Pressure-Induced Oververification”，避免用标题预设结果。

## 5. 文献可复用性与剩余核查

可复用的主要是设计，不是整套大基准：OV01 提供过程/成本区分；OV02 提供成本敏感基线；OV05 提供前缀对照；OV06 提供损失和验证效力区分；OV09 提供状态/证据记录思路。作者仓库可访问不等于运行可复现；本轮未安装依赖、下载数据集或执行其代码。

进入论文级实验前，需再次核对 OV01 的发表版本、OV06 最终全文与 MARGINAL 更新；这些可能继续压缩新颖性。当前有理由投入 P0，但没有理由先投入训练或大规模超算。
