```yaml
number: 5314
title: "Allow `__slots__` assignments in `mutable-class-default`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/slots
created_at: 2023-06-22T19:32:56Z
updated_at: 2023-06-22T20:05:04Z
url: https://github.com/astral-sh/ruff/pull/5314
synced_at: 2026-01-12T15:55:18Z
```

# Allow `__slots__` assignments in `mutable-class-default`

---

_@charliermarsh_

Closes #5309.

---

_Label `bug` added by @charliermarsh on 2023-06-22 19:32_

---

_Merged by @charliermarsh on 2023-06-22 19:40_

---

_Closed by @charliermarsh on 2023-06-22 19:40_

---

_Branch deleted on 2023-06-22 19:40_

---

_Comment by @github-actions[bot] on 2023-06-22 19:45_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -3, 0 error(s))

<details><summary>airflow (+0, -2)</summary>
<p>

```diff
- airflow/models/param.py:166:17: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- airflow/providers_manager.py:96:17: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
```

</p>
</details>
<details><summary>disnake (+0, -1)</summary>
<p>

```diff
- tests/test_utils.py:377:21: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| RUF012 | 3 | 0 | 3 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.0±0.02ms     5.8 MB/sec    1.00      6.9±0.01ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1465.7±1.27µs    11.4 MB/sec    1.00   1469.3±2.47µs    11.3 MB/sec
formatter/numpy/globals.py                 1.00    161.1±0.32µs    18.3 MB/sec    1.00    160.8±1.21µs    18.3 MB/sec
formatter/pydantic/types.py                1.01      3.5±0.01ms     7.3 MB/sec    1.00      3.5±0.01ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.02ms     3.0 MB/sec    1.00     13.7±0.01ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.8 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.2±1.18µs     8.1 MB/sec    1.00    364.5±2.12µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.7 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1486.0±2.85µs    11.2 MB/sec    1.01   1493.9±2.91µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.8±0.19µs    18.6 MB/sec    1.00    158.5±0.24µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.00ms     7.9 MB/sec    1.00      3.2±0.00ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.1±0.34ms     5.0 MB/sec    1.00      8.0±0.25ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.01  1758.5±99.39µs     9.5 MB/sec    1.00  1743.7±78.23µs     9.5 MB/sec
formatter/numpy/globals.py                 1.00    188.5±6.08µs    15.7 MB/sec    1.02    193.2±9.15µs    15.3 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.17ms     6.3 MB/sec    1.01      4.1±0.17ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.46ms     2.6 MB/sec    1.02     16.1±0.62ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.15ms     3.9 MB/sec    1.02      4.3±0.19ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   527.1±30.06µs     5.6 MB/sec    1.00   528.1±21.62µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.24ms     3.6 MB/sec    1.00      7.1±0.20ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.27ms     4.9 MB/sec    1.01      8.4±0.22ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1773.6±63.23µs     9.4 MB/sec    1.01  1784.9±43.57µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    205.6±9.37µs    14.3 MB/sec    1.03    211.7±9.67µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.14ms     6.8 MB/sec    1.02      3.8±0.12ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
