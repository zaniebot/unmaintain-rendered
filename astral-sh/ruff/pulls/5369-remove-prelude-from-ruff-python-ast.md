```yaml
number: 5369
title: "Remove prelude from `ruff_python_ast`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/prelude
created_at: 2023-06-26T14:59:09Z
updated_at: 2023-06-26T15:58:11Z
url: https://github.com/astral-sh/ruff/pull/5369
synced_at: 2026-01-12T03:36:55Z
```

# Remove prelude from `ruff_python_ast`

---

_Pull request opened by @charliermarsh on 2023-06-26 14:59_

## Summary

Per @MichaReiser, this is causing more confusion than it is helpful.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-06-26 14:59_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/visitor.rs`:1 on 2023-06-26 15:12_

Are you using `rustfmt` nightly? or do you group the use statements manually?

---

_@MichaReiser approved on 2023-06-26 15:12_

Thank you

---

_@charliermarsh reviewed on 2023-06-26 15:19_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/visitor.rs`:1 on 2023-06-26 15:19_

I use "Optimize Imports" in IntelliJ. It's arguably a bad habit but it helps a lot with refactors since it automatically cleans up unused imports and others.

---

_@MichaReiser reviewed on 2023-06-26 15:34_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/visitor.rs`:1 on 2023-06-26 15:34_

I think it's fine. It's just that it groups the imports and I think my IntelliJ doesn't use the same grouping, resulting in arbitrarily grouped imports that are no longer sorted. 

---

_Comment by @github-actions[bot] on 2023-06-26 15:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      8.1±0.03ms     5.0 MB/sec    1.00      8.0±0.03ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1840.4±17.51µs     9.0 MB/sec    1.03  1894.1±82.13µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    218.6±0.34µs    13.5 MB/sec    1.00    218.6±0.53µs    13.5 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.01ms     6.2 MB/sec    1.00      4.1±0.06ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.01     15.9±0.11ms     2.6 MB/sec    1.00     15.8±0.09ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.02ms     4.2 MB/sec    1.00      3.9±0.02ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    499.6±0.69µs     5.9 MB/sec    1.01    506.3±5.31µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.04ms     3.7 MB/sec    1.00      6.9±0.05ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.05ms     5.2 MB/sec    1.00      7.8±0.03ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1714.5±5.74µs     9.7 MB/sec    1.00   1698.2±3.19µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.5±0.37µs    15.6 MB/sec    1.02    193.8±8.45µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.01ms     7.1 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.49ms     4.4 MB/sec    1.08     10.1±0.67ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.14ms     7.9 MB/sec    1.09      2.3±0.13ms     7.2 MB/sec
formatter/numpy/globals.py                 1.00   258.1±19.33µs    11.4 MB/sec    1.11   285.8±21.50µs    10.3 MB/sec
formatter/pydantic/types.py                1.00      4.6±0.26ms     5.5 MB/sec    1.17      5.4±0.26ms     4.7 MB/sec
linter/all-rules/large/dataset.py          1.04     18.4±0.97ms     2.2 MB/sec    1.00     17.7±0.88ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.22ms     3.5 MB/sec    1.05      5.0±0.21ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   572.9±42.21µs     5.2 MB/sec    1.01   578.5±28.91µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.5±0.46ms     3.0 MB/sec    1.01      8.5±0.42ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.50ms     4.1 MB/sec    1.01     10.1±0.65ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.09      2.2±0.09ms     7.7 MB/sec    1.00  1988.5±142.56µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.02   251.7±16.38µs    11.7 MB/sec    1.00   247.5±13.33µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.5±0.31ms     5.7 MB/sec    1.00      4.4±0.22ms     5.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-26 15:43_

---

_Closed by @charliermarsh on 2023-06-26 15:43_

---

_Branch deleted on 2023-06-26 15:43_

---
