```yaml
number: 4647
title: "Use class name as range for `B024`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/range
created_at: 2023-05-24T22:09:40Z
updated_at: 2023-05-24T22:45:22Z
url: https://github.com/astral-sh/ruff/pull/4647
synced_at: 2026-01-12T15:55:16Z
```

# Use class name as range for `B024`

---

_@charliermarsh_

## Summary

Previously, this used the whole class as the diagnostic range -- which, in addition to being inconsistent with other diagnostics, means that if a class includes a decorator, the `noqa` now needs to appear on the line of the first decorator, rather than the line on which the class is defined.

This PR tweaks the range to use the identifier range.

Closes #4646.


---

_Merged by @charliermarsh on 2023-05-24 22:16_

---

_Closed by @charliermarsh on 2023-05-24 22:16_

---

_Branch deleted on 2023-05-24 22:16_

---

_@zanieb reviewed on 2023-05-24 22:16_

If only I could approve pull requests :) this lgtm!

---

_Comment by @github-actions[bot] on 2023-05-24 22:19_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+1, -1, 0 error(s))

<details><summary>airflow (+1, -1)</summary>
<p>

```diff
- airflow/secrets/base_secrets.py:29:1: B024 `BaseSecretsBackend` is an abstract base class, but it has no abstract methods
+ airflow/secrets/base_secrets.py:29:7: B024 `BaseSecretsBackend` is an abstract base class, but it has no abstract methods
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| B024 | 2 | 1 | 1 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.3±0.05ms     2.8 MB/sec    1.00     14.3±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.01      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.3±0.70µs     7.0 MB/sec    1.01    428.8±0.87µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.01      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.02      6.9±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1508.3±4.57µs    11.0 MB/sec    1.01   1518.3±2.38µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.7±1.07µs    17.3 MB/sec    1.01    173.2±1.23µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.01      3.1±0.01ms     8.1 MB/sec
parser/large/dataset.py                    1.00      5.2±0.01ms     7.9 MB/sec    1.02      5.3±0.01ms     7.7 MB/sec
parser/numpy/ctypeslib.py                  1.00   1021.2±0.66µs    16.3 MB/sec    1.01   1031.6±1.07µs    16.1 MB/sec
parser/numpy/globals.py                    1.00    104.8±0.13µs    28.1 MB/sec    1.01    105.8±0.57µs    27.9 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.01      2.3±0.00ms    11.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     21.4±0.97ms  1948.6 KB/sec    1.03     22.0±0.76ms  1895.4 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.5±0.32ms     3.1 MB/sec    1.00      5.5±0.28ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   645.1±41.04µs     4.6 MB/sec    1.00   644.2±33.33µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.2±0.69ms     2.8 MB/sec    1.01      9.3±0.39ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.9±0.57ms     3.7 MB/sec    1.00     10.9±0.39ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.11ms     7.4 MB/sec    1.03      2.3±0.13ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   274.8±17.44µs    10.7 MB/sec    1.06   291.4±26.81µs    10.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.25ms     5.3 MB/sec    1.05      5.0±0.24ms     5.1 MB/sec
parser/large/dataset.py                    1.00      8.5±0.30ms     4.8 MB/sec    1.04      8.8±0.24ms     4.6 MB/sec
parser/numpy/ctypeslib.py                  1.00  1603.7±66.46µs    10.4 MB/sec    1.07  1713.0±65.31µs     9.7 MB/sec
parser/numpy/globals.py                    1.00   163.8±12.40µs    18.0 MB/sec    1.07   175.7±13.48µs    16.8 MB/sec
parser/pydantic/types.py                   1.00      3.6±0.12ms     7.2 MB/sec    1.06      3.8±0.19ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
