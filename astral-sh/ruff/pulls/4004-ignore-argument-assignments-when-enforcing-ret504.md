```yaml
number: 4004
title: "Ignore argument assignments when enforcing `RET504`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/args
created_at: 2023-04-18T03:16:39Z
updated_at: 2023-04-18T03:51:03Z
url: https://github.com/astral-sh/ruff/pull/4004
synced_at: 2026-01-12T04:28:19Z
```

# Ignore argument assignments when enforcing `RET504`

---

_Pull request opened by @charliermarsh on 2023-04-18 03:16_

## Summary

Right now, we trigger a `RET504` (`unnecessary-assign`) error here:

```py
def str_to_bool(val):
    if isinstance(val, bool):
        return val
    val = val.strip().lower()
    if val in ("1", "true", "yes"):
        return True

    return False
```

The issue is that we can't find an assignment to `val` prior to the first `return val`, so the code as it exists today falls back to assuming that `val` is defined at the start of the function. Instead, we need to avoid flagging cases in which the return comes before an explicit in-function assignment (like arguments, or returning values defined outside of the function).

Closes #3992.


---

_Label `bug` added by @charliermarsh on 2023-04-18 03:16_

---

_Merged by @charliermarsh on 2023-04-18 03:22_

---

_Closed by @charliermarsh on 2023-04-18 03:22_

---

_Branch deleted on 2023-04-18 03:22_

---

_Comment by @github-actions[bot] on 2023-04-18 03:26_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -1, 0 error(s))

<details><summary>airflow (+0, -1)</summary>
<p>

```diff
- airflow/providers/oracle/hooks/oracle.py:47:16: RET504 Unnecessary variable assignment before `return` statement
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     14.9±0.16ms     2.7 MB/sec    1.00     14.7±0.19ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.7±0.04ms     4.5 MB/sec    1.00      3.7±0.05ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    464.6±1.49µs     6.4 MB/sec    1.00    463.4±1.69µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.09ms     4.1 MB/sec    1.00      6.2±0.09ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.04ms     5.6 MB/sec    1.02      7.4±0.06ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1627.1±3.67µs    10.2 MB/sec    1.02  1654.8±24.34µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.4±2.08µs    16.4 MB/sec    1.01    182.2±0.50µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.6 MB/sec    1.01      3.4±0.02ms     7.5 MB/sec
parser/large/dataset.py                    1.00      5.8±0.01ms     7.0 MB/sec    1.00      5.8±0.02ms     7.0 MB/sec
parser/numpy/ctypeslib.py                  1.00   1137.9±0.70µs    14.6 MB/sec    1.00   1141.5±5.78µs    14.6 MB/sec
parser/numpy/globals.py                    1.00    112.5±0.24µs    26.2 MB/sec    1.00    112.1±0.19µs    26.3 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.2 MB/sec    1.00      2.5±0.01ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.1±0.21ms     2.4 MB/sec    1.00     17.0±0.23ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.09ms     3.9 MB/sec    1.00      4.3±0.12ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.04    516.0±8.51µs     5.7 MB/sec    1.00    494.4±7.59µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.2±0.12ms     3.5 MB/sec    1.00      7.2±0.13ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.5±0.10ms     4.8 MB/sec    1.00      8.4±0.10ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1853.5±26.39µs     9.0 MB/sec    1.00  1832.7±21.69µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.03    199.0±5.93µs    14.8 MB/sec    1.00    194.1±6.57µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.05ms     6.6 MB/sec    1.00      3.8±0.06ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.6±0.05ms     6.2 MB/sec    1.01      6.7±0.04ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1270.6±14.36µs    13.1 MB/sec    1.00  1266.0±24.79µs    13.2 MB/sec
parser/numpy/globals.py                    1.00    125.8±2.36µs    23.5 MB/sec    1.01    126.7±2.96µs    23.3 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.04ms     8.9 MB/sec    1.00      2.9±0.04ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
