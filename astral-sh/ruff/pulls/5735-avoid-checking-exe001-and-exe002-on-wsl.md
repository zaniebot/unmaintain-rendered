```yaml
number: 5735
title: "Avoid checking `EXE001` and `EXE002` on WSL"
type: pull_request
state: merged
author: tjkuson
labels: []
assignees: []
merged: true
base: main
head: wsl
created_at: 2023-07-13T10:35:05Z
updated_at: 2023-07-16T11:52:13Z
url: https://github.com/astral-sh/ruff/pull/5735
synced_at: 2026-01-12T15:55:19Z
```

# Avoid checking `EXE001` and `EXE002` on WSL

---

_@tjkuson_

## Summary

Do not raise `EXE001` and `EXE002` if WSL is detected. Uses the [`wsl`](https://crates.io/crates/wsl) crate.

Closes #5445.

## Test Plan

`cargo test`

I don't use Windows, so was unable to test on a WSL environment. It would be good if someone who runs Windows could check the functionality.


---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_executable/rules/shebang_missing.rs`:49 on 2023-07-13 10:37_

Please add a sentence here as a comment explaining why this check isn't used on WSL

---

_@konstin approved on 2023-07-13 10:39_

I checked the WSL crate and it looks good (it checks `/proc/sys/kernel/osrelease`)

---

_Review requested from @charliermarsh by @konstin on 2023-07-13 10:39_

---

_@tjkuson reviewed on 2023-07-13 10:45_

---

_Review comment by @tjkuson on `crates/ruff/src/rules/flake8_executable/rules/shebang_missing.rs`:49 on 2023-07-13 10:45_

Done!

---

_Marked ready for review by @tjkuson on 2023-07-13 10:46_

---

_Renamed from "Check for WSL before checking `EXE001` and `EXE002`" to "Avoid checking `EXE001` and `EXE002` on WSL" by @tjkuson on 2023-07-13 10:47_

---

_Comment by @github-actions[bot] on 2023-07-13 11:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     10.1±0.42ms     4.0 MB/sec    1.00      9.9±0.38ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.3±0.10ms     7.2 MB/sec    1.00      2.3±0.10ms     7.3 MB/sec
formatter/numpy/globals.py                 1.01   265.0±30.98µs    11.1 MB/sec    1.00   261.6±18.06µs    11.3 MB/sec
formatter/pydantic/types.py                1.05      4.9±0.18ms     5.2 MB/sec    1.00      4.7±0.16ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     16.8±0.46ms     2.4 MB/sec    1.02     17.1±0.52ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.28ms     4.0 MB/sec    1.00      4.2±0.10ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   565.6±30.50µs     5.2 MB/sec    1.00   567.9±24.87µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.6±0.32ms     3.4 MB/sec    1.00      7.4±0.24ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.02      8.7±0.32ms     4.7 MB/sec    1.00      8.5±0.31ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1872.5±55.17µs     8.9 MB/sec    1.00  1800.2±42.99µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.01   217.3±10.92µs    13.6 MB/sec    1.00    215.1±6.89µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.09ms     6.6 MB/sec    1.01      3.9±0.15ms     6.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.5±0.04ms     4.3 MB/sec    1.02      9.6±0.05ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.06ms     7.9 MB/sec    1.01      2.1±0.02ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    229.8±3.86µs    12.8 MB/sec    1.02   234.1±12.32µs    12.6 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.05ms     5.4 MB/sec    1.01      4.8±0.14ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.10ms     2.6 MB/sec    1.00     15.9±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.2±0.05ms     3.9 MB/sec    1.00      4.2±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   430.9±11.96µs     6.8 MB/sec    1.00    427.6±5.45µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.2±0.14ms     3.5 MB/sec    1.00      7.2±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.04ms     5.0 MB/sec    1.01      8.2±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1674.5±14.01µs     9.9 MB/sec    1.01  1684.8±14.24µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.7±1.41µs    16.2 MB/sec    1.00    182.1±1.82µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.02ms     7.0 MB/sec    1.01      3.7±0.02ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-13 11:36_

---

_Closed by @charliermarsh on 2023-07-13 11:36_

---

_Branch deleted on 2023-07-16 11:52_

---
