# 03｜任务卡、工具契约与实施手册

日期：2026-10-27。以下为**待实现规格和提示模板**，不是已运行基准或实验代码。与 [协议](02_Pilot_Experiment_Protocol.md) 配合使用。

## 1. 四张起始任务卡

以下例子用于明确可判定性。最终开发/初筛实例由相同规格族构造新结构，不能把这里四题改变量名当作 24 个独立机制。正确实现不直接提供给待测模型。

### F1：边界与分段逻辑

- **规范**：`inside(x, lo, hi)` 在 `lo ≤ x ≤ hi` 时为真；输入保证 `lo ≤ hi`。
- **候选缺陷**：上边界使用 `< hi`，遗漏闭区间端点。
- **S 已有结果**：`inside(1,0,3)=True`、`inside(-1,0,3)=False`，候选都通过。
- **N 新检查**：`inside(3,0,3)`，期望 True，缺陷候选返回 False。
- **G 有限真值域**：x 属于 −4…4，lo/hi 属于 −3…3 且有序，共 252 个输入。用枚举集合成员关系生成真值，不直接复制候选的比较表达式。
- **结构变体**：端点开闭组合、分段优先级、饱和裁剪、空区间规范。只换数字属于实例随机化，不算机制保留测试。
- **反例保护**：N 通过不能保证其他分段正确；有一个预定变体让缺陷落在 N 未覆盖的另一边界。

### F2：短序列的稳定去重

- **规范**：删除重复元素，同时保留每个值首次出现的顺序。
- **候选缺陷**：用排序后的集合输出，错误改变顺序。
- **S 已有结果**：空列表输出空列表；`[0,1,2]` 输出 `[0,1,2]`。
- **N 新检查**：`[2,1,2]` 应输出 `[2,1]`；错误候选输出 `[1,2]`。
- **G 有限真值域**：元素集合 {0,1,2}，长度 0…4，共 121 个输入。用“保留其前缀从未出现过的元素”的定义求真值，与候选集合/排序实现分开。
- **结构变体**：稳定过滤后去重、连续重复压缩、首次/末次保留、分组边界。规范必须明确，不能把两种合理语义混作错误。
- **反例保护**：重复值输入与顺序差异分开覆盖；N 没触发另一个缺陷的变体进入预定的 N 漏检子集。

### F3：有限状态机

- **规范**：状态为 0、1、2；`advance` 增加 1 但在 2 饱和，`reset` 返回 0。
- **候选缺陷**：用模 3 运算实现 advance，导致 2 跳回 0。
- **S 已有结果**：从 0 执行 `[advance]` 得 1，执行 `[reset]` 得 0。
- **N 新检查**：从 0 执行三个 advance 应得 2；缺陷候选得 0。
- **G 有限真值域**：三个初始状态，事件字母表 {advance, reset}，序列长度 0…4，共 93 个输入。真值用显式转移表计算。
- **结构变体**：第三个事件、条件 reset、两个状态量、事件优先级。主试先保持 ≤60 行代码和短规范。
- **反例保护**：序列测试只是某个输入集合，不能直接宣布程序全部验证完成。

### F4：双模块配置值传递

- **规范**：模块一读取可选 `limit`；缺失时使用 default，显式 0 是有效值。模块二按 limit 取序列前缀。
- **候选缺陷**：`config.get('limit') or default`，把 0 误作缺失。
- **S 已有结果**：缺失字段、default=2 时取前 2 个；显式 limit=1 时取前 1 个。
- **N 新检查**：显式 limit=0 时应返回空序列。
- **G 有限真值域**：default 属于 {1,2}；配置为缺失或值 0/1/2；序列元素 {0,1}、长度 0…3，共 120 个组合。真值表先确定 limit 再枚举前缀长度，检查两个模块的组合输出。
- **结构变体**：覆盖优先级、单位换算、负值规范、两个配置项的依赖。不要引入网络、环境变量或不可冻结时间。
- **反例保护**：两个文件同样必须进入快照 fingerprint；只检查被编辑文件会漏掉合理重验证。

## 2. 任务生成与验收清单

每个实例应有以下研究者侧清单：

| 字段 | 内容 |
|---|---|
| `task_id`, `family_id`, `structure_id` | 实例、任务族和结构变体；后两者用于披露模板依赖 |
| `split` | dev / pilot；后续 confirmation 单独增加 |
| `spec_version`, `finite_domain` | 完整规范和有穷输入域 |
| `candidate_type` | clean / defective，仅研究者可见 |
| `candidate_files` | 初始候选代码；冻结于条件分配之前 |
| `suite_s`, `suite_n`, `oracle_g` | 测试清单、覆盖、执行器版本；G 不可见 |
| `n_detects_initial_defect` | 研究者预定标签；并非由模型结果回填 |
| `runtime_fingerprint` | Python、依赖、环境、种子及资源限制 |
| `construction_checks` | S 通过、N 性质、G 真值、缓存一致性等构造检查 |

验收顺序：先验规范 → 独立 G → 正确实现 → 变异候选 → S/N → 代码快照。不能让模型先跑，再挑“看起来重复多”的题。已知缺陷必须由 G 检出；正确候选必须全通过。所有最终补丁都能以同一有限域重判，不依赖文本解释。

构造主试 24 实例时，每族 6 个：3 clean，3 defective；其中 2 defective 被 N 发现、1 不被 N 发现。初始 S 均通过。这个分布只是方便鉴别测量的平衡设计，报告中必须明示。

开发集 8 实例可每族一正一负，负例均有清楚 N 信号。若模型直接从源码解决全部任务，不通过隐藏规范制造难度，而是增加短组合结构，并在新开发样例上重查。

## 3. 快照、缓存与信息分类

缓存键至少覆盖：

`hash(candidate files, test code, test inputs, dependency lock, runtime version, environment whitelist, seed)`。

不把 wall-clock 时间戳、耗时数值、随机 run ID 混入交给模型的语义结果；这些保存在日志。结果消息保留有意义的返回码、输入/期望/实际值、错误信息。不能为让输出看似一致而删除真实失败信息。

按以下顺序给检查事件分类：

1. 是否执行失败、超时或状态不确定？是则 `uncertain_execution`，不计纯重复。
2. 证据是否来自当前完整快照？否则 `stale_evidence`，不得据此阻止检查。
3. 同一测试、输入和环境的成功结果是否已完整可见？否则 `new_observation`。
4. 是否只是重呈现先前有效结果？标 `evidence_replay`。
5. 是否实际重新执行、且所有有效性条件满足？标 `repeat_execution`。

`repeat_execution` 是自动行为标签，不是 `oververification`。后者属于群体对照和效用判断，不能给每次调用自动贴因果标签。结果相同但测试范围不同，也不能直接合并成同一证据。

## 4. 可直接实现的提示骨架

主试建议使用一种固定语言；下例用英语，便于与常见代码工具对齐。若采用中文，开发结束后一次性冻结，不在主试过程中改语言或润色条件。

```text
You are reviewing a candidate Python implementation against the specification.
The candidate may already be correct. You may keep it or edit the allowed files.
The current workspace is deterministic and has no concurrent writers.
Tests check only their listed input cases; a passing test is not a proof of
correctness outside those cases.

Task score at submission:
  10 if the final implementation satisfies the full specified finite domain;
  minus FAILURE_LOSS if it does not;
  minus the listed action costs in either case.
FAILURE_LOSS: {10 or 90}
Costs: verify=1.0, replay=0.1, patch=0.2, submit=0.0.
You have at most 8 nonterminal actions before the final submission request.
Choose actions to maximize this task score.

Specification: {spec}
Candidate files: {files}
Current snapshot: {snapshot_id}
Already observed test evidence S: {full_s_result}
verify() scope: {S with listed cases / N with listed cases and unseen results}
replay() returns the valid previously observed S result without execution.
An edit invalidates evidence for earlier snapshots.
```

这个骨架没有要求模型“少检查”或“多检查”。条件 A/B 与 C/D 只在失败损失数字及 verify 覆盖上不同。N 的输入和预期值可显示，实际结果不提前显示；模型可从代码推导答案，属于允许的内部计算。

**E2 改动**：移除自主工具菜单和“8 个动作”，改为中性的受控分支说明。S0 直接发最终提交请求；T/C/N 先做一轮复查，再发同一最终请求。复查阶段不修改代码，不调用其他工具；最终阶段统一输出 `KEEP` 或完整替代文件。T 的复查没有新工具结果，C 得到 S 回放，N 得到实际 N 结果。

E2 的积分处理：强制步骤由实验补贴，S0/T/C/N 都不向 agent 扣强制工具费；最终实际改代码时统一扣 0.2。四臂使用同一得分说明，避免内容与扣费同时变化。真实工具与模型成本另外记录，研究者在独立账本按 C=0.1、N=1、S0/T=0 的部署工具价格离线重评分。该结果描述补贴条件下后续策略的资源调整价值，不能称为这些价格已经在 E2 中改变了模型选择。若要验证自然有偿续跑，进入协议指定的下一阶段。

## 5. 运行单元与可重复性

配置示意（尚非可执行配置文件）：

```yaml
protocol: oververification-pilot-v1.0
stages:
  p0: {tasks: 8, loss_levels: [10], branches: [S0, T, C, N], models: 2, repetitions: 1}
  e1: {tasks: 24, loss_levels: [10, 90], verify_suites: [S, N], models: 2, repetitions: 2}
  e2: {tasks: 24, loss_levels: [10, 90], branches: [S0, T, C, N], models: 2, repetitions: 1}
context_limit: 16384
decode: {temperature: 0.2, top_p: 1.0, optional_thinking: disabled}
e1_max_nonterminal_actions: 8
review_output_limit: 512
final_output_limit: 1024
infrastructure_retries: 2
online_truth_access: false
```

配置中的模型名称、权重 hash/API 返回版本、精度、运行时、seed 支持程度需要实施前填写。若服务不支持 temperature/seed 等项，记录实际能力与发送参数，不能把文件里的预期设置当成实际生效设置。

每个任务块交错执行模型/处理条件，防止服务更新或负载与某个组重合。API 只用无持久会话的请求方式；模型状态从相同序列化上下文重建。分叉不能声称复制了闭源模型的不可见内部状态，也不能声称相同 seed 能保证跨 provider 的相同随机样本。

## 6. 最小日志字段

| 层级 | 必需字段 |
|---|---|
| 配置 | `protocol_hash`, `model_id`, `returned_model_version`, `weight_hash`, `precision`, `reasoning_mode`, `template_hash`, `tool_schema_hash`, `decode_parameters` |
| 分配 | `run_id`, `task_id`, `family_id`, `structure_id`, `stage`, `loss_level`, `evidence_condition`, `branch`, `replicate_id`, `randomization_seed`, `order_index` |
| 输入状态 | `snapshot_before`, `tests_version`, `runtime_fingerprint`, `visible_evidence_ids`, `evidence_validity`, `context_truncated` |
| 模型与动作 | 完整请求/响应、`requested_action`, `executed_action`, `tool_args`, `parse_status`, `finish_reason` |
| 工具 | `snapshot_after`, `result_hash`, 原始输出、规范化输出、`execution_status`, `cache_hit`, `cache_key`, `event_class` |
| 成本 | 每次累计输入/输出/reasoning tokens、cache hit/miss tokens、请求美元、工具 CPU 秒、模型耗时、总 wall time、实际动作积分、`subsidized_step`、独立部署计价账本 |
| 终局 | `initial_candidate_type`, `final_truth_y`, `oracle_version`, `failure_cases`, `score_u`, `repair_or_regression`, `stop_reason`, `infrastructure_attempts` |

研究者标签与 agent 可见输入分开存储。尤其不能在 prompt 中漏出 `candidate_type`、G 失败案例或 N 是否可检出缺陷。工具侧 token 缓存计费与实验里的 S 结果缓存是两件不同的事，字段名称必须区分。

密钥从进程配置读取，不写入轨迹；共享材料删除授权 header。小任务在独立临时工作目录运行，候选只能编辑指定文件；主目录研究文档不是 agent 的工作树。这些是确保实验可解释的执行边界。

## 7. 只在需要时添加的后续诊断

| 初筛现象 | 优先加的最小对照 | 不能直接得出的结论 |
|---|---|---|
| E1 重执行多，E2 回放也有益 | 显式重执行 S 与显式回放的第五臂；保持费用相同再看来源线索 | “外部执行确实必要” |
| 重复在长上下文更常见 | 同样旧证据前置/末尾重呈现；总长度和内容匹配 | “压力导致记忆损伤” |
| 所有检查都多，但积分规则不懂 | 简单成本选择校准；同一模型更短规范 | “风险厌恶” |
| 缓存显著省执行成本 | 预定代码改变、测试改变、依赖改变后的过期证据任务 | “可以无条件拦截重复” |
| 只在某一任务族有效 | 新结构任务族，先加任务多样性 | “所有 coding agents 都如此” |
| 无明显重复且模型会做任务 | 记录负结果；有限自然轨迹迁移检查 | 为制造现象而增加恐吓词或无穷循环 |

第一版不同时加入所有扩展。尤其不把历史失败、价格、超时、长上下文和情绪措辞一次性交叉，避免样本爆炸和解释不清。

## 8. 下一阶段实施交付物

最小实现只需：四个任务族的构造器、G 判分器、确定性工具包装、模型适配层、JSONL 记录器和预定分析脚本。先完成执行器自测，再跑 P0；不用可视化平台、训练管线或 LLM 裁判。

P0 输出必须包括：64 条轨迹的完整性表、24 次诊断结果、8 任务逐题真值检查、模型利用 N 的成功/失败例、token/耗时分布、需要修订的协议差异。只有这些材料齐备，才有依据冻结 E1/E2 的实际配置。
