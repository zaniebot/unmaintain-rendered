```yaml
number: 5219
title: Support parenthesized expressions when splitting compound assertions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/parens
created_at: 2023-06-20T17:12:08Z
updated_at: 2023-06-20T18:17:43Z
url: https://github.com/astral-sh/ruff/pull/5219
synced_at: 2026-01-12T03:43:30Z
```

# Support parenthesized expressions when splitting compound assertions

---

_Pull request opened by @charliermarsh on 2023-06-20 17:12_

## Summary

I'm looking into the Black stability tests, and here's one failing case.

We split `assert a and (b and c)` into:

```python
assert a
assert (b and c)
```

We fail to split `assert (b and c)` due to the parentheses. But Black then removes then, and when running Ruff again, we get:

```python
assert a
assert b
assert c
```

This PR just enables us to fix to this in one pass.


---

_Label `bug` added by @charliermarsh on 2023-06-20 17:12_

---

_Comment by @github-actions[bot] on 2023-06-20 17:39_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+2, -2, 0 error(s))

<details><summary>airflow (+2, -2)</summary>
<p>

```diff
- dev/breeze/tests/test_run_utils.py:41:5: PT018 Assertion should be broken down into multiple parts
+ dev/breeze/tests/test_run_utils.py:41:5: PT018 [*] Assertion should be broken down into multiple parts
- tests/models/test_dagrun.py:920:17: PT018 Assertion should be broken down into multiple parts
+ tests/models/test_dagrun.py:920:17: PT018 [*] Assertion should be broken down into multiple parts
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PT018 | 4 | 2 | 2 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.8±0.03ms     6.0 MB/sec    1.00      6.7±0.21ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.02  1377.1±29.36µs    12.1 MB/sec    1.00   1355.9±2.80µs    12.3 MB/sec
formatter/numpy/globals.py                 1.00    132.1±0.29µs    22.3 MB/sec    1.00    131.6±0.95µs    22.4 MB/sec
formatter/pydantic/types.py                1.01      2.8±0.01ms     9.2 MB/sec    1.00      2.7±0.01ms     9.3 MB/sec
linter/all-rules/large/dataset.py          1.01     13.8±0.07ms     3.0 MB/sec    1.00     13.6±0.04ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.01ms     4.8 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.03   374.1±11.65µs     7.9 MB/sec    1.00    362.6±1.22µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.04ms     4.2 MB/sec    1.00      6.0±0.03ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.1±0.02ms     5.8 MB/sec    1.00      7.0±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1482.4±5.54µs    11.2 MB/sec    1.00   1477.5±2.27µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.7±0.18µs    18.6 MB/sec    1.00    158.1±1.10µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     7.9 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.6±0.07ms     5.4 MB/sec    1.00      7.6±0.14ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1568.2±20.60µs    10.6 MB/sec    1.00  1565.4±19.04µs    10.6 MB/sec
formatter/numpy/globals.py                 1.00    156.4±3.57µs    18.9 MB/sec    1.01    158.2±8.19µs    18.7 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.04ms     8.1 MB/sec    1.01      3.2±0.06ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.17ms     2.7 MB/sec    1.00     15.1±0.15ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.2 MB/sec    1.00      3.9±0.03ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.02    491.8±8.38µs     6.0 MB/sec    1.00    481.7±5.16µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.08ms     3.8 MB/sec    1.00      6.7±0.12ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.11ms     5.2 MB/sec    1.01      8.0±0.08ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1684.4±17.30µs     9.9 MB/sec    1.00  1683.8±19.90µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    192.7±3.26µs    15.3 MB/sec    1.00    190.7±3.06µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.1 MB/sec    1.01      3.6±0.04ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-20 17:47_

---

_Closed by @charliermarsh on 2023-06-20 17:47_

---

_Branch deleted on 2023-06-20 17:47_

---
