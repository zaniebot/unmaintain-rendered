```yaml
number: 5399
title: "Implement Pylint `single-string-used-for-slots` (`C0205`) as `single-string-slots` (`PLC0205`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
assignees: []
merged: true
base: main
head: single-string-slots
created_at: 2023-06-27T17:26:01Z
updated_at: 2023-07-10T09:54:59Z
url: https://github.com/astral-sh/ruff/pull/5399
synced_at: 2026-01-12T03:36:55Z
```

# Implement Pylint `single-string-used-for-slots` (`C0205`) as `single-string-slots` (`PLC0205`)

---

_Pull request opened by @tjkuson on 2023-06-27 17:26_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement Pylint rule `single-string-used-for-slots` (`C0205`) as `single-string-slots` (`PLC0205`). This rule checks for single strings being assigned to `__slots__`. For example

```python
class Foo:
    __slots__: str = "bar"

    def __init__(self, bar: str) -> None:
        self.bar = bar
```

should be

```python
class Foo:
    __slots__: tuple[str, ...] = ("bar",)

    def __init__(self, bar: str) -> None:
        self.bar = bar
```

Related to #970. Includes documentation.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-06-27 17:34_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+1, -0, 0 error(s))

<details><summary>disnake (+1, -0)</summary>
<p>

```diff
+ tests/test_utils.py:380:9: PLC0205 Class `__slots__` should be a non-string iterable
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLC0205 | 1 | 1 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.04ms     4.3 MB/sec    1.00      9.3±0.03ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.00ms     8.2 MB/sec    1.00      2.0±0.01ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00    228.1±0.69µs    12.9 MB/sec    1.00    228.3±1.49µs    12.9 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.01ms     5.7 MB/sec    1.00      4.4±0.01ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.08ms     2.6 MB/sec    1.01     16.0±0.12ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.02ms     4.2 MB/sec    1.00      4.0±0.01ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    512.3±1.11µs     5.8 MB/sec    1.00    513.6±3.70µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.06ms     3.7 MB/sec    1.00      7.0±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.04ms     5.1 MB/sec    1.01      8.0±0.04ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1758.5±3.43µs     9.5 MB/sec    1.00   1760.8±5.99µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    198.8±0.72µs    14.8 MB/sec    1.00    198.8±3.21µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     7.0 MB/sec    1.00      3.7±0.02ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03     10.7±0.15ms     3.8 MB/sec    1.00     10.4±0.20ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.2±0.03ms     7.7 MB/sec    1.00      2.1±0.03ms     7.9 MB/sec
formatter/numpy/globals.py                 1.00    237.9±4.67µs    12.4 MB/sec    1.00    237.2±9.08µs    12.4 MB/sec
formatter/pydantic/types.py                1.01      4.9±0.08ms     5.3 MB/sec    1.00      4.8±0.17ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.02     16.5±0.27ms     2.5 MB/sec    1.00     16.3±0.26ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.09ms     3.9 MB/sec    1.00      4.3±0.10ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   516.6±23.69µs     5.7 MB/sec    1.00    515.0±9.21µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.3±0.16ms     3.5 MB/sec    1.00      7.3±0.12ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.13ms     4.9 MB/sec    1.00      8.3±0.11ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1756.0±24.03µs     9.5 MB/sec    1.02  1792.9±25.84µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    205.2±3.89µs    14.4 MB/sec    1.00    204.6±6.59µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.06ms     6.8 MB/sec    1.01      3.8±0.06ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `rule` added by @charliermarsh on 2023-06-27 18:26_

---

_Merged by @charliermarsh on 2023-06-27 18:33_

---

_Closed by @charliermarsh on 2023-06-27 18:33_

---

_Branch deleted on 2023-07-10 09:54_

---
