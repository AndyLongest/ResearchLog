# Semantic-measurement prompt pilot - status

Last verified: 2026-08-04.

- Terminal state: GPT-5.4 paired pilot complete; Claude Sonnet 4.6 non-computable
  because the configured provider returned `model_not_supported` before formal
  pilot launch. No substitute model was used.
- Truth label: **exploratory retrospective semantic-prompt pilot**.
- Scope: 15 paired units from five fresh proposer seeds, three markets, primary
  origin 4, evidence-credit mode, and 250 candidates per arm and run.
- Validation: 30/30 runs, 7,500/7,500 unique within-run candidates, 1,835,030
  daily evidence rows, 160 prompt calls, zero parse fallbacks, and no detected
  sensitive-value leakage.
- Semantic prompt compliance: 80/80 calls; baseline semantic-marker count: 0/80.
- Primary paired locked-RankIC effect (semantic minus baseline): +0.005717,
  95% paired t interval [-0.014883, +0.026317]; **inconclusive**.
- Locked net-Sharpe effect: -0.211080, interval [-0.812155, +0.389996].
- SGG effect: +2.330214, interval [-0.983871, +5.644299]; lower is better.
- Neither arm certified a candidate at this 250-candidate, 10 bp operating
  point. Each run therefore selected its maximum-evidence fallback candidate.
- Semantic prompting increased mean prompt tokens per run from 6,655.27 to
  8,622.47 and completion tokens from 2,997.93 to 3,640.33.
- Claim boundary: already-opened 2023--2025 outcomes and an existing 26-feature
  daily basis. This pilot cannot establish prospective improvement or genuine
  high-frequency measurement invention.
- Full remote archive: 59,672,892 bytes, SHA-256
  `16ef8a24f12bb8b2f17fc6818b356d335fbb347be1bbacf13242055c71bd0057`, at
  `/mnt/SSD3_4TB/zechuan/alpha2_followups_20260731_v1/semantic_prompt_pilot_v1_archive/semantic_prompt_pilot_v1_full.tar.gz`.
- Compact recovery archive: 2,860,878 bytes, SHA-256
  `2bfbbd45b9d0c96298fce3e383eedf3be3f5a71119e5dd49d279e03387176930`.
  Local recovery verified 130 included files; the 30 omitted files are exactly
  the daily evidence parquet ledgers retained in the full remote archive.
