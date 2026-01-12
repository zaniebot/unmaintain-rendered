```yaml
number: 6406
title: "Change `model: &SemanticModel` to `semantic: &SemanticModel`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/semantic
created_at: 2023-08-07T19:59:44Z
updated_at: 2023-08-07T20:37:08Z
url: https://github.com/astral-sh/ruff/pull/6406
synced_at: 2026-01-12T15:55:21Z
```

# Change `model: &SemanticModel` to `semantic: &SemanticModel`

---

_@charliermarsh_

Use the same naming conventions everywhere. See: https://github.com/astral-sh/ruff/pull/6314/files#r1284457874.


---

_Review requested from @zanieb by @charliermarsh on 2023-08-07 20:01_

---

_Comment by @github-actions[bot] on 2023-08-07 20:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.03      8.5±0.31ms     4.8 MB/sec     1.00      8.3±0.06ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.04  1698.2±135.07µs     9.8 MB/sec    1.00  1628.2±27.29µs    10.2 MB/sec
formatter/numpy/globals.py                 1.15   210.4±17.47µs    14.0 MB/sec     1.00    182.3±2.76µs    16.2 MB/sec
formatter/pydantic/types.py                1.17      4.1±0.27ms     6.3 MB/sec     1.00      3.5±0.04ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.04     10.7±0.52ms     3.8 MB/sec     1.00     10.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.7±0.01ms     6.1 MB/sec     1.00      2.7±0.01ms     6.2 MB/sec
linter/all-rules/numpy/globals.py          1.14   432.1±34.08µs     6.8 MB/sec     1.00    380.1±0.84µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.17      5.5±0.50ms     4.7 MB/sec     1.00      4.7±0.03ms     5.5 MB/sec
linter/default-rules/large/dataset.py      1.00      5.3±0.02ms     7.7 MB/sec     1.00      5.3±0.02ms     7.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1176.3±78.15µs    14.2 MB/sec     1.00   1132.8±1.90µs    14.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    128.1±2.02µs    23.0 MB/sec     1.00    127.9±0.26µs    23.1 MB/sec
linter/default-rules/pydantic/types.py     1.06      2.5±0.18ms    10.2 MB/sec     1.00      2.4±0.02ms    10.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     12.3±0.33ms     3.3 MB/sec    1.00     12.1±0.35ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.4±0.14ms     7.0 MB/sec    1.00      2.3±0.08ms     7.2 MB/sec
formatter/numpy/globals.py                 1.00   259.3±20.35µs    11.4 MB/sec    1.00   259.5±22.14µs    11.4 MB/sec
formatter/pydantic/types.py                1.03      5.3±0.21ms     4.8 MB/sec    1.00      5.1±0.19ms     5.0 MB/sec
linter/all-rules/large/dataset.py          1.00     15.3±0.44ms     2.7 MB/sec    1.01     15.4±0.34ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.28ms     3.9 MB/sec    1.00      4.3±0.16ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   527.2±20.07µs     5.6 MB/sec    1.01   532.6±26.72µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.2±0.21ms     3.5 MB/sec    1.00      7.1±0.18ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.19ms     4.9 MB/sec    1.00      8.3±0.19ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1712.6±56.85µs     9.7 MB/sec    1.00  1712.8±59.18µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   197.4±11.81µs    14.9 MB/sec    1.00    196.9±7.51µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.12ms     7.0 MB/sec    1.01      3.7±0.09ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-08-07 20:27_

Thanks!

---

_Merged by @charliermarsh on 2023-08-07 20:32_

---

_Closed by @charliermarsh on 2023-08-07 20:32_

---

_Branch deleted on 2023-08-07 20:32_

---
