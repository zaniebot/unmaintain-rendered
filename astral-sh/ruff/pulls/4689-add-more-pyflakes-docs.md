```yaml
number: 4689
title: Add more Pyflakes docs
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: F6-rules
created_at: 2023-05-27T22:00:48Z
updated_at: 2023-05-29T15:34:32Z
url: https://github.com/astral-sh/ruff/pull/4689
synced_at: 2026-01-12T03:50:03Z
```

# Add more Pyflakes docs

---

_Pull request opened by @tjkuson on 2023-05-27 22:00_

## Summary

Add docs for Pyflakes string rules (F6) apart from `F621` which I don't think I can do justice at the moment.

Related to #2646.

## Test Plan

Ran `scripts/check_docs_formatted.py` on local machine.


---

_Comment by @github-actions[bot] on 2023-05-27 22:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.5±0.63ms     2.2 MB/sec    1.01     18.7±0.48ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.5±0.23ms     3.7 MB/sec    1.00      4.4±0.16ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   550.0±13.80µs     5.4 MB/sec    1.05   576.0±41.07µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.6±0.21ms     3.4 MB/sec    1.03      7.8±0.25ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.17ms     4.8 MB/sec    1.01      8.6±0.19ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1915.3±78.35µs     8.7 MB/sec    1.00  1881.9±58.95µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    224.9±7.15µs    13.1 MB/sec    1.01   227.7±11.75µs    13.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.11ms     6.5 MB/sec    1.00      3.9±0.09ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.5±0.11ms     6.2 MB/sec    1.01      6.6±0.15ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1309.3±40.11µs    12.7 MB/sec    1.03  1345.7±64.29µs    12.4 MB/sec
parser/numpy/globals.py                    1.00    130.5±3.62µs    22.6 MB/sec    1.01    131.6±4.95µs    22.4 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.08ms     8.9 MB/sec    1.01      2.9±0.08ms     8.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.9±0.25ms     2.4 MB/sec    1.00     16.9±0.22ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.3±0.08ms     3.9 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.02    507.2±7.95µs     5.8 MB/sec    1.00    495.1±5.75µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.08ms     3.6 MB/sec    1.00      7.0±0.16ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.09ms     4.9 MB/sec    1.00      8.2±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1764.1±18.36µs     9.4 MB/sec    1.00  1755.8±22.31µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    203.6±3.83µs    14.5 MB/sec    1.00    202.3±3.89µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.05ms     6.8 MB/sec    1.00      3.7±0.03ms     6.9 MB/sec
parser/large/dataset.py                    1.01      6.5±0.04ms     6.3 MB/sec    1.00      6.4±0.04ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.01  1239.7±16.28µs    13.4 MB/sec    1.00  1229.4±14.75µs    13.5 MB/sec
parser/numpy/globals.py                    1.01    126.1±2.14µs    23.4 MB/sec    1.00    125.3±1.74µs    23.5 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.2 MB/sec    1.00      2.8±0.04ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @tjkuson on 2023-05-28 09:51_

`cargo clippy -p ruff_wasm --target wasm32-unknown-unknown --all-features -- -D warnings` completed on my machine in 3 minutes, so I am not sure why the action timed out

---

_Label `documentation` added by @charliermarsh on 2023-05-28 22:22_

---

_Merged by @charliermarsh on 2023-05-28 22:29_

---

_Closed by @charliermarsh on 2023-05-28 22:29_

---

_Branch deleted on 2023-05-29 15:34_

---
