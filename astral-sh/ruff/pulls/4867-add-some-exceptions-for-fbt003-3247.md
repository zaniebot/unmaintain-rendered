```yaml
number: 4867
title: "Add some exceptions for FBT003 (#3247)"
type: pull_request
state: merged
author: allisonkarlitskaya
labels: []
assignees: []
merged: true
base: main
head: fbt003-allowlist
created_at: 2023-06-05T16:38:36Z
updated_at: 2023-06-05T17:11:57Z
url: https://github.com/astral-sh/ruff/pull/4867
synced_at: 2026-01-12T15:55:16Z
```

# Add some exceptions for FBT003 (#3247)

---

_@allisonkarlitskaya_

Hardcode a few exceptions for FBT003 (passing a boolean constant to a function):

  - bool (builtin)
  - set_blocking (from 'os' in the stdlib)
  - set_enabled (a reasonably common name for a function which takes a non-confusing boolean)

Add a test case, including testing another set_* function which is not currently allowed but which might be permitted in the future.

---

_@charliermarsh approved on 2023-06-05 16:39_

Looks great, thanks for contributing!

---

_Merged by @charliermarsh on 2023-06-05 16:44_

---

_Closed by @charliermarsh on 2023-06-05 16:44_

---

_Comment by @github-actions[bot] on 2023-06-05 16:47_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -1, 0 error(s))

<details><summary>airflow (+0, -1)</summary>
<p>

```diff
- airflow/dag_processing/manager.py:403:67: FBT003 Boolean positional value in function call
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| FBT003 | 1 | 0 | 1 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.6±0.05ms     2.8 MB/sec    1.00     14.6±0.04ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.05ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    361.0±1.24µs     8.2 MB/sec    1.00    362.1±1.23µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.02ms     5.6 MB/sec    1.00      7.3±0.02ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1527.5±2.99µs    10.9 MB/sec    1.00   1527.2±2.18µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.7±0.28µs    18.2 MB/sec    1.00    161.5±1.45µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.02ms     7.8 MB/sec    1.01      3.3±0.01ms     7.8 MB/sec
parser/large/dataset.py                    1.00      5.7±0.01ms     7.2 MB/sec    1.02      5.8±0.01ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1110.0±0.76µs    15.0 MB/sec    1.02   1127.8±3.74µs    14.8 MB/sec
parser/numpy/globals.py                    1.00    114.3±0.64µs    25.8 MB/sec    1.02    116.4±1.00µs    25.4 MB/sec
parser/pydantic/types.py                   1.00      2.4±0.00ms    10.4 MB/sec    1.01      2.5±0.00ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.9±0.22ms     2.4 MB/sec    1.01     17.1±0.12ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.03ms     3.9 MB/sec    1.01      4.3±0.03ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.4±5.90µs     6.9 MB/sec    1.02    435.0±7.29µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.08ms     3.6 MB/sec    1.02      7.3±0.29ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.04ms     4.9 MB/sec    1.02      8.5±0.04ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1728.6±9.27µs     9.6 MB/sec    1.03  1788.8±13.80µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.8±2.82µs    15.9 MB/sec    1.02    190.4±5.87µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.10ms     6.7 MB/sec    1.02      3.9±0.02ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.7±0.08ms     6.0 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00   1268.7±8.17µs    13.1 MB/sec    1.01  1283.8±11.03µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    131.5±0.87µs    22.4 MB/sec    1.01    132.8±1.03µs    22.2 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.0 MB/sec    1.01      2.9±0.02ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
