```yaml
number: 5372
title: Fix version number in playground
type: pull_request
state: merged
author: charliermarsh
labels:
  - playground
assignees: []
merged: true
base: main
head: charlie/playground
created_at: 2023-06-26T15:31:35Z
updated_at: 2023-06-26T16:16:48Z
url: https://github.com/astral-sh/ruff/pull/5372
synced_at: 2026-01-12T03:36:55Z
```

# Fix version number in playground

---

_Pull request opened by @charliermarsh on 2023-06-26 15:31_

## Summary

`v0.0.275` in the top-right was showing `v0.0.0` at all times.

## Test Plan

![Screen Shot 2023-06-26 at 11 31 16 AM](https://github.com/astral-sh/ruff/assets/1309177/e6cd0e19-6a5f-4b46-a060-54f492524737)


---

_Label `playground` added by @charliermarsh on 2023-06-26 15:31_

---

_Review comment by @MichaReiser on `crates/ruff_wasm/Cargo.toml`:3 on 2023-06-26 15:36_

How can we ensure that the version numbers stay in sync or is this taken care by our release script? Where do we read the version number? 

Should we add a public `VERSION` constant to the `ruff` crate and use that version instead (we can re-export it from the wasm crate if necessary)?

---

_@MichaReiser reviewed on 2023-06-26 15:36_

---

_Comment by @github-actions[bot] on 2023-06-26 15:41_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.7±0.02ms     6.1 MB/sec    1.00      6.7±0.03ms     6.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1546.9±2.22µs    10.8 MB/sec    1.00   1549.9±3.56µs    10.7 MB/sec
formatter/numpy/globals.py                 1.00    187.8±0.57µs    15.7 MB/sec    1.00    187.2±0.21µs    15.8 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.00ms     7.4 MB/sec    1.00      3.5±0.02ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.07ms     3.1 MB/sec    1.00     13.1±0.21ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.8±4.83µs     6.8 MB/sec    1.00    435.9±4.31µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.03ms     4.4 MB/sec    1.00      5.8±0.03ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.02ms     6.2 MB/sec    1.00      6.6±0.01ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1446.0±2.57µs    11.5 MB/sec    1.00   1448.6±4.02µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.3±0.18µs    18.2 MB/sec    1.00    161.6±0.23µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.8±0.04ms     7.0 MB/sec    1.19      7.0±0.05ms     5.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1271.6±12.82µs    13.1 MB/sec    1.14  1453.7±33.10µs    11.5 MB/sec
formatter/numpy/globals.py                 1.00    146.9±2.45µs    20.1 MB/sec    1.08    159.2±2.38µs    18.5 MB/sec
formatter/pydantic/types.py                1.00      3.0±0.04ms     8.6 MB/sec    1.13      3.4±0.03ms     7.6 MB/sec
linter/all-rules/large/dataset.py          1.00     11.0±0.04ms     3.7 MB/sec    1.00     11.0±0.07ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.0±0.02ms     5.6 MB/sec    1.00      3.0±0.02ms     5.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    314.2±4.69µs     9.4 MB/sec    1.00    315.5±3.99µs     9.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.0±0.04ms     5.1 MB/sec    1.00      5.0±0.03ms     5.1 MB/sec
linter/default-rules/large/dataset.py      1.00      5.8±0.04ms     7.0 MB/sec    1.00      5.8±0.10ms     7.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1209.1±7.05µs    13.8 MB/sec    1.00  1212.3±12.28µs    13.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    130.9±1.60µs    22.5 MB/sec    1.00    130.4±1.28µs    22.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.02ms     9.6 MB/sec    1.00      2.6±0.02ms     9.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff_wasm/Cargo.toml`:3 on 2023-06-26 15:43_

We just bulk replace them -- I'm not worried about it, but also fine to add a public `VERSION` constant :)

---

_@charliermarsh reviewed on 2023-06-26 15:43_

---

_@charliermarsh reviewed on 2023-06-26 15:43_

---

_Review comment by @charliermarsh on `crates/ruff_wasm/Cargo.toml`:3 on 2023-06-26 15:43_

(Will change.)

---

_@MichaReiser approved on 2023-06-26 15:44_

---

_@MichaReiser reviewed on 2023-06-26 15:45_

---

_Review comment by @MichaReiser on `crates/ruff_wasm/Cargo.toml`:3 on 2023-06-26 15:45_

I guess it depends on what version we want to say. The version of the ruff engine or the version of the WASM bindings.

---

_Merged by @charliermarsh on 2023-06-26 15:56_

---

_Closed by @charliermarsh on 2023-06-26 15:56_

---

_Branch deleted on 2023-06-26 15:56_

---
