# 2026-08-04 实验结果

## 实验名：Semantic-measurement prompt paired pilot

- 日期：2026-08-04
- 实现标签：**exploratory retrospective semantic-prompt pilot**
- 实验目的：检验在相同候选预算和共同外部评估下，要求 Agent 显式描述测量对象、时间聚合和经济状态的语义提示词，是否优于普通 baseline prompt。
- 可计算模型：GPT-5.4。
- 不可计算模型：Claude Sonnet 4.6。配置的服务商在正式 pilot 启动前返回 `model_not_supported`，未使用替代模型。
- 样本：5 个 fresh proposer seeds × 3 个市场，共 15 个配对单元；使用 primary origin 4、evidence-credit mode，每个 arm、每次运行 250 个候选，主成本为 10 bps。
- 真值边界：2023–2025 locked outcomes 此前已经被项目查看，因此该实验不是 prospective confirmation。
- 表达能力边界：候选只能使用已有的 26 个日频 typed features；实验检验的是既有特征上的语义选择，不能证明 Agent 发明了真实高频或订单簿测量。
- 数值来源：`semantic_prompt_pilot/`。目录保存两组冻结运行、协议、环境锁、run-level 数据、配对统计、决策文件、状态说明和校验文件。

## 实验数据表格

### 两组汇总

| 指标 | Baseline prompt | Semantic-measurement prompt | Semantic − Baseline |
|---|---:|---:|---:|
| Mean locked RankIC | 0.004295 | 0.010013 | +0.005717 |
| Mean locked net Sharpe | -1.043227 | -1.254307 | -0.211080 |
| Mean locked annualized net return | -0.082112 | -0.099586 | -0.017474 |
| Mean SGG（越低越好） | -1.145601 | 1.184612 | +2.330214 |
| Mean certified count | 0.000000 | 0.000000 | 0.000000 |
| Mean prompt tokens/run | 6655.27 | 8622.47 | +1967.20 |
| Mean completion tokens/run | 2997.93 | 3640.33 | +642.40 |
| Mean parse fallbacks | 0.000000 | 0.000000 | 0.000000 |

### 配对效应与不确定性

| 指标 | 配对数 | Semantic − Baseline | 95% paired t interval | 解释 |
|---|---:|---:|---:|---|
| Locked RankIC | 15 | +0.005717 | [-0.014883, 0.026317] | 区间跨 0，结论不确定 |
| Locked net Sharpe | 15 | -0.211080 | [-0.812155, 0.389996] | 区间跨 0；点估计更差 |
| Locked annualized net return | 15 | -0.017474 | [-0.067961, 0.033014] | 区间跨 0；两组均为负收益 |
| SGG（越低越好） | 15 | +2.330214 | [-0.983871, 5.644299] | 区间跨 0；点估计未改善可靠性 |
| Prompt tokens/run | 15 | +1967.20 | [1348.21, 2586.19] | 明显增加输入成本 |
| Completion tokens/run | 15 | +642.40 | [403.90, 880.90] | 明显增加输出成本 |

## 实验结论

语义测量提示词把 mean locked RankIC 从 0.004295 提高到 0.010013，但配对效应的 95% 区间跨越 0，因此主结论是 **inconclusive**，不能声称语义提示词提高了未来预测能力。

该提示词也没有改善当前可靠性—性能边界：SGG 的点效应为 +2.330214，而该指标越低越好；locked net Sharpe 和净年化收益的点效应均为负，且两个 arm 的平均净收益和净 Sharpe 都为负。两组都没有候选获得认证，因此不能声称它提高了证书数量或盈利能力。

可以确认的是执行层行为发生了变化：semantic arm 的 80/80 次 prompt calls 均满足语义约束，baseline arm 的 semantic-marker count 为 0/80，且两组均无 parse fallback。代价是每次运行平均多使用约 1967 个 prompt tokens 和 642 个 completion tokens。

因此，这个 pilot 证明提示词约束被模型遵守，但没有提供统计充分的证据证明它改善 locked RankIC、SGG、认证能力或净交易表现。后续若要形成确认性结论，需要在未打开的 locked outcomes、预先冻结的主要指标和 fresh seeds 上重新运行。

## 完整性说明

- 本目录包含论文表格和结论所需的完整 compact recovery 数据。
- 30 个 daily evidence parquet ledgers 未放入 GitHub；它们保存在状态文件记录的完整远程归档中。
- `semantic_prompt_pilot/STATUS.md` 记录完整归档与 compact recovery archive 的 SHA-256。
- `semantic_prompt_pilot/validation.json`、各 arm 的 `SHA256SUMS` 和 analysis manifest 用于完整性校验。
