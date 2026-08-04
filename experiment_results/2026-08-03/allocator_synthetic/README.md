# Exploratory allocator comparison v1

Truth label: **controlled synthetic operationalization**.

All allocators used paired candidate streams, truth labels, and evidence paths. This output selects an exploratory development winner; it is not a fresh-seed confirmatory result and makes no real-market false-positive claim.

## Primary ranking

| Scheme | Exact-null FWER | 95% Wilson interval | Macro recall | Late recall | Component coverage | Restricted end-to-end delay |
|---|---:|---:|---:|---:|---:|---:|
| explore_exploit | 0.0075 | [0.0026, 0.0218] | 0.092189 | 0.084883 | 0.495417 | 10741.78 |
| wallet_lineage | 0.0100 | [0.0039, 0.0254] | 0.089235 | 0.082195 | 0.478750 | 11155.75 |
| heavy_tail_fixed | 0.0125 | [0.0054, 0.0289] | 0.075107 | 0.070040 | 0.452500 | 12158.65 |
| epoch_reserve | 0.0125 | [0.0054, 0.0289] | 0.072911 | 0.068068 | 0.445000 | 12386.05 |
| fixed_j2 | 0.0125 | [0.0054, 0.0289] | 0.050608 | 0.044280 | 0.385000 | 13526.71 |
| current_adaptive | 0.0150 | [0.0069, 0.0323] | 0.010418 | 0.000003 | 0.364167 | 13172.35 |

## Exploratory winner

The highest macro-average recall is `explore_exploit` at 0.092189. Validity, paired uncertainty, delay, late-birth recall, score-stress results, and allocation concentration must be considered before freezing a confirmatory protocol.

## Files

- `allocator_run_level.csv`: full run-level results.
- `allocator_summary.csv`: condition-level means and uncertainty inputs.
- `macro_summary.csv`: frozen-grid primary ranking.
- `paired_effects.csv`: paired effects versus current fixed and adaptive baselines.
- `allocation_profiles.csv`: alpha consumption at representative births.
- `protocol.yaml` and `base_protocol.yaml`: frozen configurations.
- `manifest.json`: hashes, seeds, repetition counts, code identity, and missing claims.

## Missing / non-computable claims

- Real-market FWER is non-computable because real-market truth is unknown.
- Native-product superiority is not tested.
- Confirmatory allocator superiority is not established on development seeds.
