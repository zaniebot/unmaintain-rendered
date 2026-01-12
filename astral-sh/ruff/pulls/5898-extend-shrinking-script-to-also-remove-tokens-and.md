```yaml
number: 5898
title: Extend shrinking script to also remove tokens and characters
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: extend_shrinking_script
created_at: 2023-07-19T19:50:11Z
updated_at: 2023-07-20T10:02:02Z
url: https://github.com/astral-sh/ruff/pull/5898
synced_at: 2026-01-12T03:30:22Z
```

# Extend shrinking script to also remove tokens and characters

---

_Pull request opened by @konstin on 2023-07-19 19:50_

This shrinks a good bit more than previously, which was helpful for all the formatter bugs. fwiw i treat this as a very ad-hoc script since it's mainly my ecosystem bug processing companion.

---

_Label `internal` added by @konstin on 2023-07-19 19:50_

---

_Comment by @github-actions[bot] on 2023-07-19 20:21_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.01ms     4.2 MB/sec    1.01      9.8±0.02ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1902.1±8.80µs     8.8 MB/sec    1.00   1904.1±4.32µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    206.7±2.43µs    14.3 MB/sec    1.01    207.9±0.37µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.1 MB/sec    1.01      4.2±0.02ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.03ms     3.0 MB/sec    1.00     13.6±0.03ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.8 MB/sec    1.00      3.4±0.00ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    369.5±0.88µs     8.0 MB/sec    1.02    375.7±0.84µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.01      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.02ms     5.8 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1435.4±3.99µs    11.6 MB/sec    1.01   1443.2±3.41µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    150.7±0.23µs    19.6 MB/sec    1.00    150.0±0.78µs    19.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.00ms     8.0 MB/sec    1.00      3.2±0.00ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.1±0.16ms     3.7 MB/sec    1.00     11.0±0.09ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.05ms     7.7 MB/sec    1.01      2.2±0.05ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    243.7±5.83µs    12.1 MB/sec    1.01    247.0±6.08µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.07ms     5.4 MB/sec    1.01      4.8±0.08ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.09     16.6±0.47ms     2.4 MB/sec    1.00     15.3±0.19ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.12      4.5±0.13ms     3.7 MB/sec    1.00      4.0±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.05    514.0±8.32µs     5.7 MB/sec    1.00    488.8±9.68µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.07      7.4±0.16ms     3.4 MB/sec    1.00      7.0±0.10ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.04      8.3±0.09ms     4.9 MB/sec    1.00      8.0±0.05ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1699.6±19.73µs     9.8 MB/sec    1.00  1661.8±16.10µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    191.2±3.28µs    15.4 MB/sec    1.00    190.0±2.43µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.03ms     7.1 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_shrinking/src/main.rs`:296 on 2023-07-20 07:23_

Use `flatten` to not throw on invalid syntax OR should this bail if the input syntax is invalid?

---

_@MichaReiser approved on 2023-07-20 07:24_

---

_@konstin reviewed on 2023-07-20 09:06_

---

_Review comment by @konstin on `crates/ruff_shrinking/src/main.rs`:296 on 2023-07-20 09:06_

added a comment because we know we have valid python here (we get the AST)

---

_Merged by @konstin on 2023-07-20 10:02_

---

_Closed by @konstin on 2023-07-20 10:02_

---

_Branch deleted on 2023-07-20 10:02_

---
