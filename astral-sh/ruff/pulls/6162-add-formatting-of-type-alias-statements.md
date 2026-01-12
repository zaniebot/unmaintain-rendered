```yaml
number: 6162
title: Add formatting of type alias statements
type: pull_request
state: merged
author: zanieb
labels:
  - formatter
assignees: []
merged: true
base: main
head: zanie/695-format-type-alias
created_at: 2023-07-28T21:04:00Z
updated_at: 2023-08-02T20:54:53Z
url: https://github.com/astral-sh/ruff/pull/6162
synced_at: 2026-01-12T15:55:20Z
```

# Add formatting of type alias statements

---

_@zanieb_

Part of #5062 
Extends https://github.com/astral-sh/ruff/pull/6161
Closes #5929 

---

_Comment by @github-actions[bot] on 2023-07-28 21:34_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      8.5±0.06ms     4.8 MB/sec    1.00      8.3±0.10ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1644.0±24.84µs    10.1 MB/sec    1.00  1642.3±24.25µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    183.0±4.08µs    16.1 MB/sec    1.00    182.3±3.57µs    16.2 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.09ms     7.1 MB/sec    1.00      3.6±0.10ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.00     11.0±0.08ms     3.7 MB/sec    1.03     11.3±0.12ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.02ms     5.9 MB/sec    1.00      2.8±0.02ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    379.8±1.21µs     7.8 MB/sec    1.00    378.4±0.96µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.0±0.06ms     5.1 MB/sec    1.00      4.9±0.07ms     5.2 MB/sec
linter/default-rules/large/dataset.py      1.02      5.9±0.03ms     6.9 MB/sec    1.00      5.7±0.04ms     7.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1198.0±3.18µs    13.9 MB/sec    1.00   1192.0±2.07µs    14.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    132.5±0.46µs    22.3 MB/sec    1.00    132.2±0.28µs    22.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.03ms    10.1 MB/sec    1.00      2.5±0.04ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.11ms     4.0 MB/sec    1.00     10.0±0.11ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1955.7±24.20µs     8.5 MB/sec    1.00  1953.0±41.65µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    214.6±3.88µs    13.7 MB/sec    1.01    216.4±6.94µs    13.6 MB/sec
formatter/pydantic/types.py                1.01      4.3±0.05ms     6.0 MB/sec    1.00      4.2±0.07ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.01     13.6±0.13ms     3.0 MB/sec    1.00     13.5±0.12ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.04ms     4.7 MB/sec    1.00      3.5±0.03ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.8±5.77µs     6.8 MB/sec    1.01    433.5±6.35µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.09ms     4.2 MB/sec    1.01      6.1±0.09ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.06ms     5.6 MB/sec    1.00      7.3±0.04ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1473.5±18.51µs    11.3 MB/sec    1.00  1477.1±13.82µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.2±3.07µs    17.9 MB/sec    1.00    165.4±4.07µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.04ms     8.0 MB/sec    1.01      3.2±0.04ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @zanieb on 2023-07-28 22:25_

---

_Review requested from @konstin by @zanieb on 2023-07-28 22:25_

---

_Label `formatter` added by @konstin on 2023-07-31 12:42_

---

_@MichaReiser approved on 2023-07-31 15:42_

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/type_alias.py`:52 on 2023-08-01 10:02_

```suggestion
type X = (  # trailing parentheses comment
```
that's one of those that often creates problems

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/type_alias.py`:71 on 2023-08-01 10:03_

```suggestion
type type_params_comments[  # trailing bracket comment
```

---

_@konstin approved on 2023-08-01 10:04_

---

_Marked ready for review by @zanieb on 2023-08-02 20:22_

---

_Comment by @zanieb on 2023-08-02 20:22_

Will rebase on merge of #6161 

---

_Merged by @zanieb on 2023-08-02 20:40_

---

_Closed by @zanieb on 2023-08-02 20:40_

---

_Branch deleted on 2023-08-02 20:40_

---
