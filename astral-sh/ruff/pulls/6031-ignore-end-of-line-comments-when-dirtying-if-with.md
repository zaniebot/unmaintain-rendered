```yaml
number: 6031
title: Ignore end-of-line comments when dirtying if-with-same-arms branches
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/sim
created_at: 2023-07-24T14:44:41Z
updated_at: 2024-03-07T11:32:26Z
url: https://github.com/astral-sh/ruff/pull/6031
synced_at: 2026-01-12T15:55:20Z
```

# Ignore end-of-line comments when dirtying if-with-same-arms branches

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ruff/issues/6025 (which contains a more thorough description of the issue). Previously, the `# noqa` here was being marked as unused, but removing it raised `SIM114`:

```python
def foo():
    a = True
    b = False
    if a > b:  # noqa: SIM114
        return 3
    elif a == b:
        return 3
```

---

_Label `bug` added by @charliermarsh on 2023-07-24 14:44_

---

_Review requested from @konstin by @MichaReiser on 2023-07-24 14:46_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:685 on 2023-07-24 14:56_

that's a smart way to handle this

---

_@konstin approved on 2023-07-24 14:56_

---

_Comment by @github-actions[bot] on 2023-07-24 14:59_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+1, -0, 0 error(s))

<details><summary>airflow (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/d05e42e5d2081909c9c33de4bd4dfb759ac860c1/airflow/providers/oracle/hooks/oracle.py#L299'>airflow/providers/oracle/hooks/oracle.py:299:17:</a> SIM114 Combine `if` branches using logical `or` operator
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| SIM114 | 1 | 1 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     10.6±0.58ms     3.8 MB/sec    1.00     10.5±0.71ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.08      2.1±0.17ms     7.8 MB/sec    1.00  1975.2±146.37µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00   232.2±16.13µs    12.7 MB/sec    1.07   248.7±18.82µs    11.9 MB/sec
formatter/pydantic/types.py                1.06      4.3±0.33ms     5.9 MB/sec    1.00      4.1±0.16ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.98ms     2.7 MB/sec    1.01     15.4±1.10ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      3.9±0.27ms     4.3 MB/sec    1.00      3.7±0.25ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.04   540.6±40.39µs     5.5 MB/sec    1.00   517.4±41.88µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.07      7.1±0.41ms     3.6 MB/sec    1.00      6.6±0.53ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.08      8.0±0.39ms     5.1 MB/sec    1.00      7.4±0.38ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1592.6±88.46µs    10.5 MB/sec    1.00  1554.6±99.06µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.04    186.1±9.48µs    15.9 MB/sec    1.00    178.8±9.63µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.11      3.5±0.18ms     7.2 MB/sec    1.00      3.2±0.15ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.10     12.1±0.23ms     3.4 MB/sec    1.00     11.0±0.17ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.07      2.4±0.07ms     7.1 MB/sec    1.00      2.2±0.07ms     7.5 MB/sec
formatter/numpy/globals.py                 1.02   262.0±10.38µs    11.3 MB/sec    1.00   256.4±16.34µs    11.5 MB/sec
formatter/pydantic/types.py                1.05      5.1±0.07ms     5.0 MB/sec    1.00      4.9±0.13ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.22ms     2.6 MB/sec    1.00     15.7±0.28ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.06ms     4.1 MB/sec    1.00      4.1±0.07ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    485.2±7.77µs     6.1 MB/sec    1.01   491.3±11.57µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.09ms     3.6 MB/sec    1.00      7.1±0.10ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.15ms     5.0 MB/sec    1.00      8.1±0.14ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1690.9±36.47µs     9.8 MB/sec    1.03  1739.7±44.68µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.5±5.22µs    15.3 MB/sec    1.01    194.5±6.65µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.05ms     7.1 MB/sec    1.01      3.6±0.07ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-24 14:59_

---

_Closed by @charliermarsh on 2023-07-24 14:59_

---

_Branch deleted on 2023-07-24 14:59_

---

_Comment by @floschne on 2024-03-07 11:32_

I'm experiencing the same with `S404`. Is this known? 

---
