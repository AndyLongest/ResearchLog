# S&P 500 allocator gate replay v1

Truth label: **retrospective gate-only allocator replacement on previously observed market outcomes**.

This experiment froze all selections and score objects from the 2017--2022 ledgers before the evaluation phase loaded 2023--2024 outcomes. The outcomes had been observed in earlier project work, so this remains a retrospective version comparison.

## Selection effect

- Evidence-credit seeds with a changed selected factor: 0/10.
- Evidence-credit new certificates: 0 across ten seeds.

## Primary 10 bp comparison

| Endpoint | Current | New allocator | Difference | 95% paired interval | Holm p |
|---|---:|---:|---:|---:|---:|
| net_annualized_return | 0.114474 | 0.114474 | +0.000000 | [0.000000, 0.000000] | 1.000000 |
| net_sharpe | 0.724183 | 0.724183 | +0.000000 | [0.000000, 0.000000] | 1.000000 |
| max_drawdown | -0.149068 | -0.149068 | +0.000000 | [0.000000, 0.000000] | 1.000000 |
| turnover | 0.098942 | 0.098942 | +0.000000 | [0.000000, 0.000000] | 1.000000 |

## Claim limits

- Gate-only replay; the current operationalization does not feed certification into proposer or skill updates.
- Previously observed locked market outcomes; not prospective confirmation.
- Real-market outcomes do not identify true null factors and cannot establish FWER.
- Negative net return or Sharpe, if present, remains negative.
