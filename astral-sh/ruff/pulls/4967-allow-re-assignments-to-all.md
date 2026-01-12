```yaml
number: 4967
title: "Allow re-assignments to `__all__`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/all
created_at: 2023-06-08T17:10:12Z
updated_at: 2023-06-08T17:43:42Z
url: https://github.com/astral-sh/ruff/pull/4967
synced_at: 2026-01-12T15:55:17Z
```

# Allow re-assignments to `__all__`

---

_@charliermarsh_

## Summary

We should arguably allow any assignments like `__all__ += foo`, since we can't resolve the type of `foo`, but the most common is reassigning like `__all__ += foo.__all__`, so I'm just allowing that for now.

Closes #4345.


---

_Label `bug` added by @charliermarsh on 2023-06-08 17:10_

---

_Merged by @charliermarsh on 2023-06-08 17:19_

---

_Closed by @charliermarsh on 2023-06-08 17:19_

---

_Branch deleted on 2023-06-08 17:19_

---

_Comment by @github-actions[bot] on 2023-06-08 17:23_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -1, 0 error(s))

<details><summary>bokeh (+0, -1)</summary>
<p>

```diff
- src/bokeh/colors/named.py:179:1: PLE0605 Invalid format for `__all__`, must be `tuple` or `list`
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLE0605 | 1 | 0 | 1 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.20      6.5±0.03ms     6.2 MB/sec    1.00      5.4±0.03ms     7.5 MB/sec
formatter/numpy/ctypeslib.py               1.06   1152.7±3.23µs    14.4 MB/sec    1.00  1089.7±10.03µs    15.3 MB/sec
formatter/numpy/globals.py                 1.01    129.1±0.32µs    22.9 MB/sec    1.00    127.7±0.25µs    23.1 MB/sec
formatter/pydantic/types.py                1.08      2.6±0.01ms     9.6 MB/sec    1.00      2.4±0.02ms    10.4 MB/sec
linter/all-rules/large/dataset.py          1.01     14.5±0.09ms     2.8 MB/sec    1.00     14.4±0.09ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.02ms     4.8 MB/sec    1.01      3.5±0.02ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    427.6±0.85µs     6.9 MB/sec    1.00    427.7±1.14µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.04ms     4.3 MB/sec    1.00      6.0±0.05ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.03ms     5.9 MB/sec    1.00      6.9±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1485.9±5.47µs    11.2 MB/sec    1.01   1497.1±4.13µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.9±0.35µs    17.9 MB/sec    1.00    165.7±0.60µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.01      3.1±0.01ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.15     10.2±0.32ms     4.0 MB/sec    1.00      8.8±0.24ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.03  1762.8±57.25µs     9.4 MB/sec    1.00  1719.4±78.82µs     9.7 MB/sec
formatter/numpy/globals.py                 1.00    190.2±9.09µs    15.5 MB/sec    1.04   197.2±18.80µs    15.0 MB/sec
formatter/pydantic/types.py                1.04      4.1±0.23ms     6.2 MB/sec    1.00      3.9±0.19ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.01     21.6±0.48ms  1931.6 KB/sec    1.00     21.4±0.44ms  1943.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.5±0.22ms     3.0 MB/sec    1.00      5.4±0.17ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   636.6±20.22µs     4.6 MB/sec    1.00   638.3±34.53µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.3±0.26ms     2.8 MB/sec    1.00      9.3±0.28ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.7±0.25ms     3.8 MB/sec    1.01     10.8±0.41ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.3±0.09ms     7.2 MB/sec    1.00      2.3±0.10ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.02   263.8±11.60µs    11.2 MB/sec    1.00   257.4±11.18µs    11.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.15ms     5.2 MB/sec    1.00      4.9±0.16ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
