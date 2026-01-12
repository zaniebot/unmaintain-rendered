```yaml
number: 5869
title: "Implement `unparse` for type aliases and parameters"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/695-unparse
created_at: 2023-07-18T17:40:40Z
updated_at: 2023-07-18T21:25:50Z
url: https://github.com/astral-sh/ruff/pull/5869
synced_at: 2026-01-12T03:30:22Z
```

# Implement `unparse` for type aliases and parameters

---

_Pull request opened by @zanieb on 2023-07-18 17:40_

Part of https://github.com/astral-sh/ruff/issues/5062

---

_Review comment by @zanieb on `crates/ruff_python_ast/src/source_code/generator.rs`:1686 on 2023-07-18 17:42_

I don't quite understand why parens are added around the value here

---

_@zanieb reviewed on 2023-07-18 17:42_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/generator.rs`:541 on 2023-07-18 17:52_

I think you may want `precedence::ASSIGN` here.

---

_@charliermarsh reviewed on 2023-07-18 17:52_

---

_Comment by @github-actions[bot] on 2023-07-18 18:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.7±0.42ms     3.8 MB/sec    1.00     10.6±0.61ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.06      2.1±0.08ms     7.9 MB/sec    1.00  1997.1±67.65µs     8.3 MB/sec
formatter/numpy/globals.py                 1.05    251.2±9.36µs    11.7 MB/sec    1.00   238.5±16.76µs    12.4 MB/sec
formatter/pydantic/types.py                1.04      4.5±0.16ms     5.6 MB/sec    1.00      4.3±0.15ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.04     15.0±0.45ms     2.7 MB/sec    1.00     14.4±0.32ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.8±0.08ms     4.4 MB/sec    1.00      3.7±0.11ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.04   523.3±25.66µs     5.6 MB/sec    1.00   500.8±19.15µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.04      6.9±0.23ms     3.7 MB/sec    1.00      6.6±0.21ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.03      7.6±0.18ms     5.3 MB/sec    1.00      7.4±0.22ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1658.5±52.75µs    10.0 MB/sec    1.00  1652.9±59.66µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.2±6.20µs    15.2 MB/sec    1.03    200.7±6.42µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.09ms     7.4 MB/sec    1.00      3.4±0.11ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     13.3±0.32ms     3.1 MB/sec    1.01     13.5±0.30ms     3.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.6±0.08ms     6.3 MB/sec    1.00      2.6±0.08ms     6.3 MB/sec
formatter/numpy/globals.py                 1.00    295.6±8.78µs    10.0 MB/sec    1.03   303.6±16.12µs     9.7 MB/sec
formatter/pydantic/types.py                1.00      5.7±0.21ms     4.4 MB/sec    1.01      5.8±0.15ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.00     18.6±0.32ms     2.2 MB/sec    1.01     18.8±0.39ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.07ms     3.4 MB/sec    1.01      4.9±0.09ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   590.2±11.85µs     5.0 MB/sec    1.06   624.0±27.02µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.6±0.23ms     3.0 MB/sec    1.00      8.5±0.16ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.17ms     4.2 MB/sec    1.03      9.9±0.20ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.04ms     8.3 MB/sec    1.02      2.1±0.05ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    229.8±7.29µs    12.8 MB/sec    1.02   235.4±10.81µs    12.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.06ms     5.9 MB/sec    1.03      4.4±0.11ms     5.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-18 20:31_

---

_Merged by @zanieb on 2023-07-18 21:25_

---

_Closed by @zanieb on 2023-07-18 21:25_

---

_Branch deleted on 2023-07-18 21:25_

---
