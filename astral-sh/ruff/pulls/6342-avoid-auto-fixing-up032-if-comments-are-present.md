```yaml
number: 6342
title: Avoid auto-fixing UP032 if comments are present around format call arguments
type: pull_request
state: merged
author: harupy
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: UP032-comment-autofix
created_at: 2023-08-04T14:08:05Z
updated_at: 2023-08-04T15:59:59Z
url: https://github.com/astral-sh/ruff/pull/6342
synced_at: 2026-01-12T15:55:21Z
```

# Avoid auto-fixing UP032 if comments are present around format call arguments

---

_@harupy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

Fix #6336 

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


A new test case

---

_@zanieb reviewed on 2023-08-04 14:10_

---

_Review comment by @zanieb on `crates/ruff/src/rules/pyupgrade/rules/f_strings.rs`:437 on 2023-08-04 14:10_

Alternatively we could downgrade from a `suggested` to a `manual` fix if we detect comments? 

---

_Review comment by @zanieb on `crates/ruff/src/rules/pyupgrade/rules/f_strings.rs`:437 on 2023-08-04 14:11_

(Although this change is probably best for right now)

---

_@zanieb reviewed on 2023-08-04 14:11_

---

_Comment by @github-actions[bot] on 2023-08-04 14:24_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+1, -1, 0 error(s))

<details><summary>airflow (+1, -1)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/52ca7bfc988f4c9b608f544bc3e9524fd6564639/airflow/providers/qubole/hooks/qubole.py#L189'>airflow/providers/qubole/hooks/qubole.py:189:17:</a> UP032 Use f-string instead of `format` call
- <a href='https://github.com/apache/airflow/blob/52ca7bfc988f4c9b608f544bc3e9524fd6564639/airflow/providers/qubole/hooks/qubole.py#L189'>airflow/providers/qubole/hooks/qubole.py:189:17:</a> UP032 [*] Use f-string instead of `format` call
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| UP032 | 2 | 1 | 1 |

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.06      9.6±0.44ms     4.2 MB/sec     1.00      9.1±0.32ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.04  1862.9±101.75µs     8.9 MB/sec    1.00  1784.8±90.53µs     9.3 MB/sec
formatter/numpy/globals.py                 1.00    213.3±7.49µs    13.8 MB/sec     1.01    214.6±7.08µs    13.7 MB/sec
formatter/pydantic/types.py                1.04      3.9±0.12ms     6.5 MB/sec     1.00      3.8±0.30ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.06     13.7±0.82ms     3.0 MB/sec     1.00     12.9±0.57ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.12ms     5.0 MB/sec     1.00      3.3±0.13ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   457.6±17.84µs     6.4 MB/sec     1.03   472.3±36.53µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.08      6.4±0.45ms     4.0 MB/sec     1.00      5.9±0.25ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.15      7.1±0.18ms     5.8 MB/sec     1.00      6.1±0.29ms     6.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.08  1453.4±46.19µs    11.5 MB/sec     1.00  1347.5±54.03µs    12.4 MB/sec
linter/default-rules/numpy/globals.py      1.05    171.3±7.34µs    17.2 MB/sec     1.00    163.6±8.82µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.16      3.1±0.11ms     8.3 MB/sec     1.00      2.6±0.09ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.12     12.4±0.57ms     3.3 MB/sec    1.00     11.0±0.76ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.4±0.16ms     7.0 MB/sec    1.00      2.3±0.16ms     7.1 MB/sec
formatter/numpy/globals.py                 1.07   274.5±16.18µs    10.7 MB/sec    1.00   257.0±24.12µs    11.5 MB/sec
formatter/pydantic/types.py                1.12      5.3±0.23ms     4.8 MB/sec    1.00      4.7±0.23ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.22     19.2±0.91ms     2.1 MB/sec    1.00     15.7±0.75ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.10      4.5±0.19ms     3.7 MB/sec    1.00      4.1±0.21ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   566.5±29.90µs     5.2 MB/sec    1.03   584.3±36.02µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.11      8.0±0.27ms     3.2 MB/sec    1.00      7.3±0.40ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.07      9.0±0.42ms     4.5 MB/sec    1.00      8.4±0.42ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.11  1862.5±63.11µs     8.9 MB/sec    1.00  1681.4±86.67µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.10   222.3±13.14µs    13.3 MB/sec    1.00   202.8±12.33µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.13      4.2±0.16ms     6.1 MB/sec    1.00      3.7±0.18ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-08-04 15:22_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/f_strings.rs`:436 on 2023-08-04 15:22_

Instead, can you grab the `arguments` node off the call (line 321), and use `arugments.range()`? To me that better matches the semantics and is a bit more readable.

---

_@harupy reviewed on 2023-08-04 15:26_

---

_Review comment by @harupy on `crates/ruff/src/rules/pyupgrade/rules/f_strings.rs`:436 on 2023-08-04 15:26_

Great suggestion. Yeah, that's much better.

---

_Label `bug` added by @charliermarsh on 2023-08-04 15:29_

---

_Label `autofix` added by @charliermarsh on 2023-08-04 15:29_

---

_@charliermarsh approved on 2023-08-04 15:29_

---

_Merged by @charliermarsh on 2023-08-04 15:37_

---

_Closed by @charliermarsh on 2023-08-04 15:37_

---

_Branch deleted on 2023-08-04 15:55_

---
