# 2026-07-31 实验结果

## 实验名：匹配 family budget 的可靠性—功效比较

- 日期：2026-07-31
- 实现标签：paper-aligned synthetic follow-up
- 比较方法：Fixed Bonferroni、Alpha2 fixed gate、Alpha2 adaptive gate。
- 共同预算：family-level `alpha = 0.05`。
- 数值来源：`matched_family_budget/matched_family_budget_panel.csv` 与 `matched_family_budget/matched_family_budget_contrasts.csv`。

### 实验数据表格

| 信号强度 | 方法 | FWER | Recall | Precision | 首次认证限制均值延迟 |
|---:|---|---:|---:|---:|---:|
| 0.00 | Fixed Bonferroni | 0.043 | 0 | 0 | 600.000 |
| 0.00 | Alpha2 fixed | 0.019 | 0 | 0 | 600.000 |
| 0.00 | Alpha2 adaptive | 0.020 | 0 | 0 | 600.000 |
| 0.50 | Fixed Bonferroni | 0.630 | 0.463810 | 0.995707 | 600.000 |
| 0.50 | Alpha2 fixed | 0.118 | 0.173326 | 0.998709 | 117.346 |
| 0.50 | Alpha2 adaptive | 0.162 | 0.035122 | 0.991219 | 139.154 |

### 实验结论

在 exact-null 条件下，三种方法的 FWER 都低于 0.05，Alpha2 两个 gate 更保守。在强信号条件下，Alpha2 能更早出现第一次真实认证，但召回率显著低于 Fixed Bonferroni；其中 fixed gate 的召回率明显高于 adaptive gate。因此结果支持“Alpha2 更谨慎且可持续监控”，不支持“Alpha2 在匹配预算后支配 Bonferroni 的功效”。信号条件行中的 FWER 是混合真假候选流里的错误发现事件，不能替代 exact-null 校准。

## 实验名：证据反馈对搜索效率的影响

- 日期：2026-07-31
- 实现标签：paper-aligned paired synthetic ablation
- 比较：完整 evidence feedback 与 no-feedback；每个条件 300 个配对运行。
- 数值来源：`skill_feedback/skill_feedback_paired_effects.csv`。

### 实验数据表格

| 信号强度 | 指标 | Full | No feedback | 配对差值 | 95% CI |
|---:|---|---:|---:|---:|---|
| 0.10 | Recall | 0.000041 | 0.000067 | -0.000026 | [-0.000045, -0.000007] |
| 0.20 | Recall | 0.000577 | 0.000408 | +0.000170 | [0.000058, 0.000282] |
| 0.35 | Recall | 0.031003 | 0.007768 | +0.023235 | [0.021602, 0.024868] |
| 0.35 | 首次认证限制均值延迟 | 233.370 | 334.153 | -100.783 | [-113.998, -87.569] |
| 0.50 | Recall | 0.169510 | 0.040232 | +0.129278 | [0.122556, 0.136000] |
| 0.50 | 首次认证限制均值延迟 | 120.060 | 163.970 | -43.910 | [-50.263, -37.557] |

### 实验结论

证据反馈在中强信号下提高 recall 并缩短首次认证延迟，但在最弱信号 `delta=0.10` 下反而降低 recall。结果支持“evidence-aware skill feedback 在部分信号区间改善搜索效率”，不支持无条件、全信号强度的性能提升。

## 实验名：三市场 fresh-seed 反馈复现

- 日期：2026-07-31
- 实现标签：**paper-aligned three-market retrospective fresh-seed replication**
- 规模：300 runs、150 个完整配对；市场为 CSI500、CSI1000、S&P500。
- 主决策范围：locked 2023–2025。
- 数值来源：`fresh_seed_replication/paired_effects.csv`、`absolute_arm_summary.csv` 与 `decision.json`。为便于程序读取，原始多层表头另展开为 `absolute_arm_summary_flat.csv`，数值未改动。

### 实验数据表格

| 主指标 | Evidence − Adaptation | 95% CI | 判定 |
|---|---:|---|---|
| SGG | -1.738578 | [-2.623510, -0.842571] | superiority 通过 |
| Locked RankIC | -0.000475 | [-0.006174, 0.004980] | 在 -0.005 margin 下 non-inferiority 未通过 |
| Locked net Sharpe | +0.300395 | [0.089572, 0.515409] | 在 0.2 margin 下 non-inferiority 通过 |
| Locked annualized net return | +0.023160 | [0.006843, 0.039811] | 相对改善 |
| Certified count | 0.000000 | [0, 0] | 两臂均无认证 |

| 绝对指标 | Adaptation credit | Evidence credit |
|---|---:|---:|
| Locked net Sharpe | -1.208484 | -0.908089 |
| Locked annualized net return | -0.095611 | -0.072451 |
| SGG | 2.465902 | 0.727324 |

### 实验结论

Evidence credit 显著降低 SGG，并相对改善净 Sharpe 与年化净收益；但 RankIC 的预设非劣检验未通过，所以联合主张失败。两个 arm 的绝对净 Sharpe 和绝对年化净收益都为负，不能描述为盈利。该实验是回顾性 fresh-seed robustness，不是新的 post-registration prospective confirmation；两臂均未产生正式认证。

## 实验名：原生 baseline 结果装配

- 日期：2026-07-31
- 实现标签：各系统 truth label 见原始 CSV。
- 数值来源：`native_baselines/native_gate_panel.csv` 与 `native_baselines/common_evaluator_panel.csv`。

### 实验数据表格

| 系统 | 状态 | 完成/计划 | 原生或可用主指标 | 估计值 | 关键限制 |
|---|---|---:|---|---:|---|
| Alpha2 | complete | 10/10 | common-evaluator net Sharpe, 10 bps | 0.724183 | 参考 operationalization，不是外部 native product |
| AlphaAgent | partial terminal | 16/20 | S&P500 RankIC | 0.000232 | 精确共同 operating point 未冻结 |
| QuantaAlpha | complete | 1/1 | common-evaluator net Sharpe, 10 bps | 0.724573 | 仅一个 frozen reduced-budget library，无 proposer-seed 区间 |
| AlphaForge | complete eligible scope | 3/3 | native CSI500 2022 RankIC | 0.060333 | zoo=30 reduced budget；与 S&P500 2023–2024 不同协议 |
| AlphaPROBE | complete 5/5 native | 5/5 | 不可计算 | — | 无兼容的冻结 score object |
| RD-Agent(Q) | complete 5/5 native | 5/5 | 不可计算 | — | 原生 quant loop 在合格环境中不可执行 |

### 实验结论

原生 baseline 已按各自真实状态装配，但多数系统不能在完全相同的 S&P500 2023–2024 common evaluator 下形成数值。Alpha2 与 QuantaAlpha 的 common-evaluator net Sharpe 点估计接近，但 QuantaAlpha 只有一个 frozen library、区间极宽；AlphaForge 的数值来自不同市场和时期。因此该表用于透明呈现已完成、部分完成和不可计算项，不支持简单的跨产品排名或 Alpha2 全面优于外部 baseline 的结论。
