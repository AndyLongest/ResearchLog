# 2026-08-03 实验结果

## 实验名：错误预算分配器的合成市场比较

- 日期：2026-08-03
- 实现标签：**controlled synthetic operationalization**
- 实验性质：exploratory development，不是 fresh-seed confirmatory result。
- 协议：总错误预算 `alpha=0.05`，每条流 1,000 个候选、600 期 evidence horizon；exact-null 400 次重复；每个信号强度 200 次重复，信号网格为 `0.10/0.20/0.35/0.50`。
- 数值来源：`allocator_synthetic/`。该目录保留 run-level、条件汇总、配对效应、分配剖面、协议与 manifest。

### 实验数据表格

| 分配方案 | Exact-null FWER | 95% Wilson CI | Macro recall | Late recall | Component coverage | 限制均值端到端延迟 |
|---|---:|---|---:|---:|---:|---:|
| explore_exploit | 0.0075 | [0.0026, 0.0218] | 0.092189 | 0.084883 | 0.495417 | 10741.78 |
| wallet_lineage | 0.0100 | [0.0039, 0.0254] | 0.089235 | 0.082195 | 0.478750 | 11155.75 |
| heavy_tail_fixed | 0.0125 | [0.0054, 0.0289] | 0.075107 | 0.070040 | 0.452500 | 12158.65 |
| epoch_reserve | 0.0125 | [0.0054, 0.0289] | 0.072911 | 0.068068 | 0.445000 | 12386.05 |
| fixed_j2 | 0.0125 | [0.0054, 0.0289] | 0.050608 | 0.044280 | 0.385000 | 13526.71 |
| current_adaptive | 0.0150 | [0.0069, 0.0323] | 0.010418 | 0.000003 | 0.364167 | 13172.35 |

| 预先指定的配对比较 | 指标 | 平均效应 | 95% CI |
|---|---|---:|---|
| explore_exploit − fixed_j2 | Recall | +0.041581 | [0.040401, 0.042761] |
| explore_exploit − fixed_j2 | Late recall | +0.040604 | [0.039226, 0.041982] |
| explore_exploit − fixed_j2 | Component coverage | +0.110417 | [0.098727, 0.122106] |
| explore_exploit − fixed_j2 | 限制均值端到端延迟 | -2784.93 | [-3286.00, -2283.86] |

### 实验结论

在这组受控合成开发数据上，`explore_exploit` 的 macro recall、late recall 和 component coverage 最高，exact-null FWER 仍低于 0.05，并且相对 `fixed_j2` 缩短了端到端认证延迟。该结果说明新的分配规则在受控合成环境中改变了认证行为并提高了开发集功效，但它只是在已打开的开发种子上选出的探索性 winner，不能据此宣称 fresh-seed superiority、真实市场 FWER 或原生产品优势。

## 实验名：S&P 500 allocator gate-only replay

- 日期：2026-08-03
- 实现标签：**retrospective gate-only allocator replacement on previously observed market outcomes**
- 实验性质：仅替换错误预算 gate；候选、分数与 2017–2022 selection inputs 已冻结。2023–2024 市场结果此前已被项目查看，因此不是 prospective confirmation。
- 样本：10 个 evidence-credit frozen seeds；共同外部评估使用 `sm20_rb10_k20_drop2`、一天 lag、10 bps 主成本。
- 数值来源：`sp500_allocator_gate_replay/`。保留 240 行 run-level 结果、selection manifest、完整四指标 family、Holm 校正、协议和 manifest。

### 实验数据表格

| 选择层指标 | Current allocator | Explore-exploit allocator |
|---|---:|---:|
| 改变最终 selected factor 的 seeds | — | 0/10 |
| 新产生的 certificates | 0 | 0 |

| 10 bps endpoint | Current | New allocator | New − Current | 95% paired CI | Holm p |
|---|---:|---:|---:|---|---:|
| Net annualized return | 0.114474 | 0.114474 | 0.000000 | [0, 0] | 1.000000 |
| Net Sharpe | 0.724183 | 0.724183 | 0.000000 | [0, 0] | 1.000000 |
| Max drawdown | -0.149068 | -0.149068 | 0.000000 | [0, 0] | 1.000000 |
| Turnover | 0.098942 | 0.098942 | 0.000000 | [0, 0] | 1.000000 |

### 实验结论

在现有 S&P 500 operationalization 中，新 allocator 没有改变 10 个 evidence-credit seeds 的任何最终选择，也没有产生 certificate，因此四个共同评估指标完全不变。这与手写日志中“新的方案对模型行为没什么影响”对应。它不否定合成实验中的 recall 改善；两者共同说明：当前系统中 certification 尚未真正反馈进 proposer，而且没有候选越过 gate，所以仅替换预算分配器无法改变真实市场输出。真实市场真值未知，本实验不能估计 FWER。

## 实验名：AlphaAgent 统一外部 locked evaluator

- 日期：2026-08-03
- 实现标签：**AlphaAgent native-compatible**；Alpha2 行为 frozen operationalization；QuantaAlpha 为单个 frozen reduced-budget library。
- 共同协议：point-in-time S&P 500，locked interval 为 2023-01-03 至 2024-12-30，`sm20_rb10_k20_drop2`，一天 lag，10 bps 主成本。
- AlphaAgent：16 个 eligible frozen v5 trials；原始 4 个 failed/timeout trials 未被替换。
- 数值来源：`alphaagent_common_evaluator/`。保留正式 panel、16 个 trial-level evaluation JSON、聚合结果、协议和 hashes。

### 实验数据表格

| 系统 | 状态 / n | Net annualized return（95% CI） | Net Sharpe（95% CI） | Max drawdown（95% CI） | Turnover（95% CI） |
|---|---|---|---|---|---|
| Alpha2 | complete / 10 | 0.114474 [0.077868, 0.151080] | 0.724183 [0.542511, 0.905855] | -0.149068 [-0.173871, -0.124265] | 0.098942 [0.087386, 0.110498] |
| AlphaAgent | complete exact operating point / 16 | 0.046006 [0.021199, 0.070813] | 0.238169 [0.104776, 0.371563] | -0.221918 [-0.240458, -0.203378] | 0.101613 [0.092147, 0.111078] |
| QuantaAlpha | complete / 1 frozen library | 0.091220 [-0.086987, 0.267977] | 0.724573 [-0.690000, 2.192700] | -0.139943 [-0.275701, -0.076998] | 0.107600 [0.102600, 0.111600] |
| AlphaForge | non-computable | — | — | — | — |
| AlphaPROBE | non-computable | — | — | — | — |
| RD-Agent(Q) | non-computable | — | — | — | — |

### 实验结论

AlphaAgent 首次在完全一致的 S&P 500 external locked evaluator 下形成可计算行；其 16 个 eligible trials 的平均净年化收益和净 Sharpe 均为正。Alpha2 的点估计高于 AlphaAgent，但 Alpha2 是 evidence-credit frozen operationalization，不能包装成外部 native product；本表也未提供 Alpha2−AlphaAgent 的预注册配对 superiority test，因此不能据此宣称产品级显著胜出。QuantaAlpha 只有一个 frozen library，区间很宽，不能代表 proposer-seed 稳定性。其余三种 baseline 因缺少兼容的冻结 score object，继续保持 non-computable，未用 proxy 填充。

## 当日未形成正式结果的工作

AlphaPROBE 的新 S&P 500 upstream-native common-evaluator run 在 seed `2026080302` 遇到原生表达式 shape mismatch 后，按 frozen stop-on-failure 规则停止。Seed 1 有一个可评估输出，但整组五种子实验未完成，seeds 3–5 未启动；该 checkpoint 因而不作为完成实验上传，也不能替代此前 CSI-market AlphaPROBE v18 结果。
