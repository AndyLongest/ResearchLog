# AlphaForge zoo-30 parallel native checkpoint

Frozen: 2026-07-27 Asia/Shanghai

## Decision

- The user requested a smaller zoo and improved scheduling.
- The new run uses the unchanged upstream AlphaForge entry points with
  `zoo_size=30` and seeds 0--4.
- Its truth label is `AlphaForge native, zoo=30 reduced-budget configuration`.
  It is not interchangeable with the canonical zoo-100 result.
- Seeds are independent upstream executions and may be scheduled on separate
  GPUs. No search, proposal, training, acceptance, or portfolio logic is
  modified.
- The old zoo-100 `v15` run must be preserved and excluded from the zoo-30
  analysis.

## Status

- Protocol frozen locally before any zoo-30 worker launch.
- Environment and data staging to server 186 completed with chunk-level and
  whole-archive SHA-256 verification.
- The omitted relocatable Python 3.11 runtime was transferred separately and
  verified as an environment-only compatibility repair.
- Server 186 currently has all five RTX PRO 6000 GPUs occupied by unrelated
  workloads, so no AlphaForge worker was started there.
- The superseded zoo-100 `v15` process on server 134 was terminated only after
  preservation. Its full partial run was frozen at
  `frozen/alphaforge_zoo100_v15_partial_preserved_20260728`, hashed, marked
  read-only, and excluded from zoo-30 analysis.
- Zoo-30 seed 0 was launched on server 134 physical GPU 4 as an isolated
  unchanged upstream `train_AFF.py` process. No zoo-100 candidates were reused;
  only the market-data cache was copied into the isolated work directory.
- Its first pre-candidate launch exited because `QLIB_PATH_CN` was absent. The
  failed logs were preserved, the variable was restored to the already-frozen
  `data/qlib_cn/qlib_bin` store, and the identical seed/configuration was
  restarted as an environment-only compatibility correction.
- Staging attempt 1 created and hashed the immutable 4.12 GB environment/data
  package as 62 source chunks, then the server-186 SFTP connection dropped
  while uploading the first 64 MiB chunk. The local and remote partial files,
  logs, and source package are preserved. No formal worker was started.
- Staging attempt 2 is active with new non-overwriting local and remote
  directories. It uses independently SHA-256-checked 16 MiB chunks and a fresh
  connection per transfer to reduce the failure surface; the scientific source,
  environment, data, and package hash are unchanged.

## Monitor update

Updated: 2026-07-28 04:03 Asia/Shanghai

- Staging attempt 2 completed all 246 independently checked 16 MiB parts. The
  reconstructed archive passed its full SHA-256 check, extraction completed,
  and the remote `STAGING_VERIFIED` marker was written. No fatal error or
  credential leak was observed in the staging log.
- The transferred Python 3.11 runtime is executable and its independent
  `PYTHON_RUNTIME_VERIFIED` marker is present. Imports of Torch, NumPy, pandas,
  Qlib, and scikit-learn succeeded; CUDA is visible.
- The frozen CSI500 Qlib store passed a read-only data preflight on server 186:
  Qlib initialized from the staged path, returned 16 January 2020 trading days,
  and resolved the CSI500 instrument specification.
- No zoo-30 worker has been launched. All five server-186 GPUs are currently
  occupied by unrelated long-running processes using about 73.5 GB each, so
  the preferred physical GPU lanes 0, 2, and 3 are not presently safe to use.
- The superseded zoo-100 process remains active on server 134 and is preserved;
  it has not been terminated or mixed into the zoo-30 run. The next action is
  to wait for an isolated preferred GPU lane, recheck for duplicate workers,
  and then launch the frozen per-seed native command without changing the
  scientific protocol.

## Monitor update

Updated: 2026-07-28 04:20 Asia/Shanghai

- The superseded zoo-100 `v15` process was terminated after preservation. Its
  immutable directory retains the protocol, command, environment lock, source
  manifest, partial native outputs, complete logs, final state
  (`returncode=143`), SHA-256 manifest, and `secret_logged=false`. Its partial
  zoo remains excluded from every zoo-30 analysis.
- The first server-134 zoo-30 launch failed before native training because the
  frozen Qlib path environment variable was absent. Its empty stdout and
  failure stderr were retained under distinct attempt-1 filenames; no
  candidate output from that compatibility failure is reused.
- Seed 0 was relaunched on server 134 physical GPU 4 with the frozen staged
  Qlib path and unchanged upstream command (`zoo_size=30`). It is active in
  generator epoch 12 of 200, its output is growing, and no fatal error is
  present in the latest logs.
- No zoo-30 process is running on server 186. Its five physical GPUs remain
  occupied by unrelated jobs at about 73.5 GB each. Seeds 1--3 therefore remain
  gated on isolated preferred GPU lanes, and seed 4 remains queued behind the
  first completed shard.

## Monitor update

Updated: 2026-07-28 15:41 Asia/Shanghai

- Server 134 refused the pinned SSH endpoint on three consecutive connection
  attempts. The active seed-0 process and its latest zoo progress therefore
  could not be verified from the monitor; no duplicate or replacement worker
  was launched.
- Server 186 remains reachable. It has no AlphaForge zoo-30 worker, and all
  five GPUs remain occupied by unrelated workloads at about 73.5 GB with
  100 percent utilization. Seeds 1--4 remain queued rather than sharing those
  lanes.
- The next safe action is to re-establish server-134 access and inspect the
  preserved seed-0 process and logs before any restart or reassignment.

## Monitor update

Updated: 2026-07-28 16:17 Asia/Shanghai

- Server 134 became briefly reachable after a host reboot. A fresh,
  non-overwriting seed-0 recovery attempt was already active with the frozen
  zoo-30 protocol; it explicitly reuses no partial candidates from the
  interrupted attempt.
- The recovered seed-0 worker is the only AlphaForge process on server 134.
  Its log and native zoo output are growing, with zoo progress at 2 of 30.
  The process is GPU-resident, no fatal error was found, and the checked logs
  contained no credential pattern.
- Four additional physical GPUs on server 134 were observed idle. Before
  seeds 1--4 could be launched in fresh isolated directories, the SSH endpoint
  again stopped completing its protocol banner. The launch transaction had
  not begun, so no new directories or duplicate workers were created.
- Server 186 remains reachable but fully occupied by unrelated workloads.
  Continue by reconnecting to server 134, rechecking seed-0 and duplicate
  state, then launching the queued independent shards only on verified idle
  lanes.

## Monitor update

Updated: 2026-07-28 16:24 Asia/Shanghai

- The unique recovery chain detected that seed-0 recovery attempt 2 had used
  physical GPU 0 because the upstream script applies its CUDA visibility
  setting after CUDA initialization. Attempt 2 was terminated, preserved,
  marked `excluded_device_mapping`, and made ineligible for analysis; its two
  partial zoo entries will not be reused.
- Seed 0 was restarted again in the new non-overwriting
  `attempt3_physical_gpu4` directory. The external GPU visibility mask is set
  before Python starts, so the unchanged upstream `cuda:0` now maps to physical
  GPU 4. The state records that no partial candidates were reused.
- Attempt 3 is the only AlphaForge worker on server 134. It is active on
  physical GPU 4 and has begun native training; the zoo is currently 0 of 30.
  Logs are growing, with no fatal error or credential pattern detected.
- Physical GPUs 0--3 on server 134 are idle, while server 186 remains fully
  occupied. Seeds 1--4 remain unlaunched pending the existing scheduler's
  duplicate check and isolated-lane assignment.

## Monitor update

Updated: 2026-07-29 16:33 Asia/Shanghai

- Seed 0 attempt 3 remains the only AlphaForge worker on server 134 and has
  reached zoo progress 23 of 30. Its logs and native outputs continue to grow,
  with no fatal error or credential pattern detected.
- Server 186 physical GPUs 0, 2, and 3 became idle. After duplicate and
  isolation checks, fresh non-overwriting seed 1--3 workers were launched on
  those three preferred lanes with the frozen zoo-30 commands.
- All three server-186 workers exited before candidate generation with
  `CUDA error: no kernel image is available for execution on the device`.
  The staged Torch 2.6.0+cu124 environment provides kernels only through
  `sm_90`, while the RTX PRO 6000 Blackwell devices require `sm_120`.
  The failed attempts are preserved, marked environment-incompatible and
  ineligible for analysis, and contain no reusable candidates or credential
  pattern.
- A non-overwriting environment-only compatibility repair is now active under
  `blackwell_torch_repair_v1`. It clones the frozen environment, minimally
  upgrades Torch to the CUDA 12.8 build, and must pass an actual `sm_120`
  tensor-kernel preflight before any seed is relaunched. The original staged
  environment remains unchanged.
- Seed 4 remains queued. No formal server-186 worker will be restarted until
  the repaired environment, package inventory, kernel execution, and hashes
  are verified.

## Monitor update

Updated: 2026-07-29 16:52 Asia/Shanghai

- `blackwell_torch_repair_v1` completed successfully without overwriting the
  original staged environment. The repaired environment uses Torch
  2.7.1+cu128, advertises `sm_120`, and passed an actual tensor-kernel test on
  an RTX PRO 6000 Blackwell device. Its package inventory, preflight, marker,
  and SHA-256 records are frozen.
- Fresh non-overwriting seed 1--3 attempt-2 workers were launched on server 186
  physical GPUs 0, 2, and 3 after duplicate and lane-isolation checks. They use
  the unchanged upstream zoo-30 command and reuse no candidates; the only
  variation is the verified environment-only Torch/CUDA compatibility repair.
- All three workers remained active beyond the point where their incompatible
  attempt-1 predecessors failed. Their logs are growing, with no fatal error or
  credential pattern detected.
- Seed 0 remains active on server 134 at zoo progress 23 of 30. Seed 4 remains
  queued behind the first completed shard as frozen in the scheduling
  protocol.

## Monitor update

Updated: 2026-07-28 16:08 Asia/Shanghai

- Server 134 became reachable again and host records confirmed two reboots,
  with the current boot at 15:56 Asia/Shanghai. The seed-0 native process did
  not survive the reboot. No OOM, NVIDIA Xid, fatal Python exception, or
  credential leak was found in the retained logs.
- The interrupted seed-0 directory was frozen read-only with a SHA-256
  manifest and an explicit `interrupted` state. Its partial native zoo contains
  seven entries and is ineligible for analysis; none of those candidates is
  reused.
- A fresh non-overwriting seed-0 recovery attempt was launched on server 134
  physical GPU 4 with the same frozen protocol, source, seed, `zoo_size=30`,
  epochs, thresholds, and candidate logic. Only the immutable market-data
  cache was reused, while the native output directory started empty.
- The recovery attempt is running under a detached supervisor and records its
  own state and exit code. Seeds 1--4 remain queued because all server-186 GPU
  lanes are still occupied by unrelated workloads.

## Monitor update

Updated: 2026-07-28 16:21 Asia/Shanghai

- The reboot-recovery process was found on physical GPU 0 rather than the
  protocol-assigned physical GPU 4. Inspection confirmed that upstream sets
  `CUDA_VISIBLE_DEVICES` inside `train_AFF.py`, after CUDA had already been
  initialized in this environment. The attempt was stopped, hashed, marked
  read-only, and excluded with its two partial zoo entries; no candidates are
  reused.
- A third fresh non-overwriting seed-0 attempt now sets
  `CUDA_VISIBLE_DEVICES=4` before Python starts, while retaining the unchanged
  upstream `--cuda=4` command and all frozen scientific settings. NVIDIA's
  process UUID confirms that the worker is now on physical GPU 4.
- The corrected attempt is running, its native output directory began empty,
  and its state records `partial_candidates_reused=false`. No fatal error or
  credential-leak indicator was observed at launch.

## Monitor update

Updated: 2026-07-30 09:53 Asia/Shanghai

- Seed 0 attempt 3 completed normally with return code zero and exactly 30
  native zoo entries. Its retained logs contain no fatal-error or credential
  pattern, and no candidates from either excluded seed-0 predecessor are used.
- Seed 3 attempt 2 on server 186 also completed normally with return code zero
  and exactly 30 native zoo entries. Seeds 1 and 2 remain active at 26 and 24
  entries respectively, with growing logs and no fatal-error or credential
  pattern.
- In accordance with the frozen queue rule, seed 4 was launched only after the
  first completed shard freed server 134 physical GPU 4. It runs in the fresh,
  non-overwriting `seed4_v1_attempt1_physical_gpu4` directory with the unchanged
  upstream command, `zoo_size=30`, and no candidate reuse. Physical-GPU
  residency is confirmed; it has reached 16 of 30 entries with no fatal-error
  or credential pattern.
- The reporting truth label remains **AlphaForge native, zoo=30 reduced-budget
  configuration**. The preserved zoo-100 `v15` output and all excluded recovery
  attempts remain outside this analysis.

## Monitor update

Updated: 2026-07-30 14:38 Asia/Shanghai

- Seed 2 attempt 2 on server 186 completed normally with return code zero and
  exactly 30 native zoo entries. Its retained logs contain no fatal-error or
  credential pattern.
- Seeds 0, 2, and 3 are now complete at 30 of 30. Seed 1 remains active at 29
  of 30 and seed 4 remains active at 19 of 30; both processes and their logs
  continue to grow normally with no fatal-error or credential pattern.
- Per-seed native combination remains gated until all five frozen stage-1
  shards are complete. No completed shard has been restarted or mixed with the
  preserved zoo-100 run.

## Monitor update

Updated: 2026-07-30 15:38 Asia/Shanghai

- Seed 1 attempt 2 on server 186 completed normally with return code zero and
  exactly 30 native zoo entries. Its retained logs contain no fatal-error or
  credential pattern.
- Seeds 0--3 are now complete at 30 of 30. Seed 4 remains the only active
  stage-1 shard and has reached 20 of 30; its process and logs continue to grow
  normally with no fatal-error or credential pattern.
- Native combination remains gated on seed 4. No completed shard was
  restarted, and the zoo-100 `v15` run remains preserved and excluded.

## Scope update

Updated: 2026-07-31 Asia/Shanghai

- All five zoo-30 stage-1 shards completed normally at 30 of 30 entries, but
  the user subsequently restricted the formal manuscript analysis to seeds
  1--3. Seeds 0 and 4 remain preserved and are excluded from the formal
  aggregate.
- Unchanged native `combine_AFF.py` completed normally for seeds 1--3 on
  server 186, producing the expected prediction artifacts with no fatal-error
  or credential pattern.
- A seed-0 combination attempt on server 134 first exposed a launcher quoting
  error and then an isolated-device OOM caused by an unrelated process
  occupying the assigned GPU. Both attempts and logs are preserved. Later
  transferred seed-0 and seed-4 combination workers were stopped after the
  user's scope restriction; their inputs, transfer hashes, logs, and exclusion
  states remain preserved and are not eligible for analysis.
- The formal AlphaForge row is therefore a three-seed result and must be
  labeled **AlphaForge native, zoo=30 reduced-budget configuration (seeds
  1--3)**. Native-gate/common-evaluator assembly and manuscript replacement
  remain to be completed.

## Monitor update

Updated: 2026-07-30 09:47 Asia/Shanghai

- Seed 0 attempt 3 completed the unchanged upstream native search with zoo
  progress 30 of 30 and return code 0. Its protocol, command, environment
  inventory, native stage-1 output, logs, terminal state, and SHA-256 records
  were copied into a non-overwriting read-only `stage1_freeze_v1` snapshot;
  the snapshot manifest verifies successfully.
- Seed 3 attempt 2 also completed at zoo progress 30 of 30 with return code 0
  in the verified Blackwell compatibility environment. Its corresponding
  non-overwriting read-only `stage1_freeze_v1` snapshot and SHA-256 manifest
  verify successfully.
- After seed 0 released server 134 physical GPU 4, the queued seed 4 shard was
  launched there in a fresh isolated attempt with no candidate reuse. It is
  active at zoo progress 14 of 30.
- Seeds 1 and 2 remain active on server 186 at zoo progress 26 and 24 of 30,
  respectively. All active logs continue to grow, and the checked eligible
  runs contain no fatal error or credential pattern.
- These runs retain the formal label `AlphaForge native, zoo=30 reduced-budget
  configuration`; the preserved zoo-100 v15 run and every excluded failed or
  device-mismapped attempt remain outside the analysis.

## Monitor update

Updated: 2026-07-30 14:27 Asia/Shanghai

- Seed 2 attempt 2 completed the unchanged upstream native stage-1 search with
  return code 0 and exactly 30 zoo entries. Its protocol, command, verified
  Blackwell compatibility environment inventory, native output, logs, and
  terminal state were copied into a new non-overwriting read-only
  `stage1_freeze_v1` snapshot.
- The seed-2 snapshot contains 12 files, has no writable files, and its complete
  SHA-256 manifest verifies successfully. No partial or excluded candidates
  were reused.
- Seed 1 remains active on server 186 at 29 of 30 entries, and seed 4 remains
  active on server 134 at 19 of 30 entries. Their logs continue to grow; the
  eligible runs contain no detected fatal-error or credential-leak pattern.
- The formal truth label remains `AlphaForge native, zoo=30 reduced-budget
  configuration`. The preserved zoo-100 v15 run and all excluded attempts
  remain outside this analysis.

## Monitor update

Updated: 2026-07-31 10:25 Asia/Shanghai

- Seed 4 completed native stage 1 with return code 0 and exactly 30 zoo
  entries. Its new non-overwriting read-only `stage1_freeze_v1` snapshot
  contains 12 files, has no writable files, and its complete SHA-256 manifest
  verifies successfully. All five eligible seeds are therefore complete and
  frozen at stage 1.
- Native `combine_AFF.py` completed successfully for seeds 1, 2, and 3 in the
  verified Blackwell environment, with return code 0 and the expected
  per-seed prediction artifacts.
- The first two seed-0 combine attempts on server 134 were preserved as failed
  operational attempts. The latest failed because an unrelated process left
  insufficient memory on the assigned physical GPU; no candidate, threshold,
  training, acceptance, combination, or backtest logic was changed.
- The frozen seed-0 and seed-4 stage-1 inputs were transferred through
  `D:\Alpha2WorkspaceData` to fresh, non-overwriting server-186 stage-2
  directories. The source freeze manifests and transfer SHA-256 hashes were
  verified before launch. Native combine workers for seeds 0 and 4 are now
  running on isolated, otherwise idle Blackwell GPU lanes using the unchanged
  upstream command and algorithm.
- No eligible log contains a detected credential-leak pattern. The formal
  truth label remains `AlphaForge native, zoo=30 reduced-budget
  configuration`; zoo-100 v15 and every failed or excluded attempt remain
  outside the analysis.

## Monitor update

Updated: 2026-07-31 10:33 Asia/Shanghai

- The recovered native combine workers for seeds 0 and 4 completed with return
  code 0 and produced both expected prediction tensors per seed. Together with
  seeds 1--3, all five frozen zoo-30 seeds have now completed the unchanged
  upstream `combine_AFF.py` stage.
- A new non-overwriting read-only `stage2_freeze_v1` snapshot was created for
  every seed. Each snapshot contains its protocol, exact command, environment
  inventory, state, logs, native zoo and prediction artifacts, and a complete
  SHA-256 manifest. All five manifests verify successfully and no frozen file
  is writable.
- The common-evaluator handoff must preserve the protocol difference: current
  native AlphaForge predictions are for the upstream CSI500 2022 test split,
  whereas the frozen external evaluator requires point-in-time S&P500 scores
  over 2023-01-03 through 2024-12-30. Unless a pre-period frozen compatible
  score object can be derived without changing the native algorithm or using
  locked outcomes, that panel entry will be retained as non-computable rather
  than replaced by a proxy.
- No eligible log contains a detected credential-leak pattern. The formal
  truth label remains `AlphaForge native, zoo=30 reduced-budget
  configuration`; zoo-100 v15 and every failed or excluded attempt remain
  outside the analysis.

## Monitor update

Updated: 2026-07-30 15:37 Asia/Shanghai

- Seed 1 attempt 2 completed the unchanged upstream native stage-1 search with
  return code 0 and exactly 30 zoo entries. Its protocol, command, verified
  Blackwell compatibility environment inventory, native output, logs, and
  terminal state were copied into a new non-overwriting read-only
  `stage1_freeze_v1` snapshot.
- The seed-1 snapshot contains 12 files, has no writable files, and its complete
  SHA-256 manifest verifies successfully. No partial or excluded candidates
  were reused.
- Seed 4 remains active on server 134 at 20 of 30 entries. Its process and log
  remain active, with no detected fatal-error or credential-leak pattern.
- The formal truth label remains `AlphaForge native, zoo=30 reduced-budget
  configuration`. The preserved zoo-100 v15 run and all excluded attempts
  remain outside this analysis.
