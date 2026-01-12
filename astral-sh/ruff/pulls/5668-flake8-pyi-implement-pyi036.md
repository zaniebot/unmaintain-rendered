```yaml
number: 5668
title: "[`flake8-pyi`] Implement PYI036"
type: pull_request
state: merged
author: density
labels:
  - rule
assignees: []
merged: true
base: main
head: PYI036
created_at: 2023-07-11T01:48:38Z
updated_at: 2023-07-13T03:18:33Z
url: https://github.com/astral-sh/ruff/pull/5668
synced_at: 2026-01-12T03:36:55Z
```

# [`flake8-pyi`] Implement PYI036

---

_Pull request opened by @density on 2023-07-11 01:48_

## Summary

Implements PYI036 from `flake8-pyi`. See [original code](https://github.com/PyCQA/flake8-pyi/blob/main/pyi.py#L1585)

## Test Plan

- Updated snapshots
- Checked against manual runs of flake8

ref: #848 


---

_Marked ready for review by @density on 2023-07-11 01:57_

---

_Comment by @github-actions[bot] on 2023-07-11 02:30_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+2, -0, 0 error(s))

<details><summary>typeshed (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/f2ee9e9368a18b19bbf2ac05b6eb6bfea96d9a0c/stdlib/builtins.pyi#L867'>stdlib/builtins.pyi:867:27:</a> PYI036 The first argument in `__exit__` should be annotated with `object` or `type[BaseException] | None`
+ <a href='https://github.com/python/typeshed/blob/f2ee9e9368a18b19bbf2ac05b6eb6bfea96d9a0c/stdlib/typing.pyi#L758'>stdlib/typing.pyi:758:23:</a> PYI036 The first argument in `__exit__` should be annotated with `object` or `type[BaseException] | None`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI036 | 2 | 2 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.6±0.04ms     4.2 MB/sec    1.00      9.5±0.03ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.04      2.3±0.01ms     7.4 MB/sec    1.00      2.2±0.00ms     7.7 MB/sec
formatter/numpy/globals.py                 1.03    251.9±1.10µs    11.7 MB/sec    1.00    244.4±0.49µs    12.1 MB/sec
formatter/pydantic/types.py                1.04      4.9±0.01ms     5.2 MB/sec    1.00      4.7±0.01ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.01     16.5±0.07ms     2.5 MB/sec    1.00     16.3±0.08ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.02ms     4.1 MB/sec    1.00      4.1±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    516.9±2.75µs     5.7 MB/sec    1.00    516.0±1.23µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.3±0.04ms     3.5 MB/sec    1.00      7.3±0.05ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.03ms     5.0 MB/sec    1.00      8.1±0.03ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1762.3±5.44µs     9.4 MB/sec    1.00   1752.6±7.25µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.1±0.70µs    14.7 MB/sec    1.00    200.4±2.85µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.01ms     7.0 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06     15.3±0.45ms     2.7 MB/sec    1.00     14.4±0.32ms     2.8 MB/sec
formatter/numpy/ctypeslib.py               1.09      3.3±0.14ms     5.1 MB/sec    1.00      3.0±0.13ms     5.5 MB/sec
formatter/numpy/globals.py                 1.08   355.0±20.57µs     8.3 MB/sec    1.00   328.1±21.98µs     9.0 MB/sec
formatter/pydantic/types.py                1.08      7.2±0.23ms     3.5 MB/sec    1.00      6.7±0.29ms     3.8 MB/sec
linter/all-rules/large/dataset.py          1.00     22.4±0.47ms  1860.1 KB/sec    1.00     22.4±0.52ms  1860.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.7±0.15ms     2.9 MB/sec    1.01      5.8±0.17ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   674.9±31.84µs     4.4 MB/sec    1.01   683.9±24.61µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.00     10.0±0.23ms     2.6 MB/sec    1.00     10.0±0.29ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.01     11.5±0.22ms     3.5 MB/sec    1.00     11.4±0.23ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.4±0.08ms     7.1 MB/sec    1.00      2.4±0.08ms     7.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   277.6±11.60µs    10.6 MB/sec    1.04   287.7±18.11µs    10.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.0±0.12ms     5.1 MB/sec    1.01      5.0±0.22ms     5.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `rule` added by @charliermarsh on 2023-07-13 02:43_

---

_Merged by @charliermarsh on 2023-07-13 02:50_

---

_Closed by @charliermarsh on 2023-07-13 02:50_

---
