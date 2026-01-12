```yaml
number: 5360
title: Improve backslash-detection rule for docstrings
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/backslash
created_at: 2023-06-26T01:35:52Z
updated_at: 2023-06-26T02:15:52Z
url: https://github.com/astral-sh/ruff/pull/5360
synced_at: 2026-01-12T03:36:55Z
```

# Improve backslash-detection rule for docstrings

---

_Pull request opened by @charliermarsh on 2023-06-26 01:35_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-06-26 01:48_

---

_Merged by @charliermarsh on 2023-06-26 01:58_

---

_Closed by @charliermarsh on 2023-06-26 01:58_

---

_Branch deleted on 2023-06-26 01:58_

---

_Comment by @github-actions[bot] on 2023-06-26 02:01_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+14, -0, 0 error(s))

<details><summary>airflow (+13, -0)</summary>
<p>

```diff
+ airflow/kubernetes/pod_generator.py:550:9: D301 Use `r"""` if any backslashes in a docstring
+ airflow/kubernetes/pod_generator_deprecated.py:293:9: D301 Use `r"""` if any backslashes in a docstring
+ airflow/models/baseoperator.py:1613:5: D301 Use `r"""` if any backslashes in a docstring
+ airflow/models/baseoperator.py:1745:5: D301 Use `r"""` if any backslashes in a docstring
+ airflow/operators/bash.py:34:5: D301 Use `r"""` if any backslashes in a docstring
+ airflow/providers/cncf/kubernetes/utils/pod_manager.py:184:9: D301 Use `r"""` if any backslashes in a docstring
+ airflow/providers/docker/operators/docker.py:503:9: D301 Use `r"""` if any backslashes in a docstring
+ airflow/providers/google/cloud/hooks/datacatalog.py:301:9: D301 Use `r"""` if any backslashes in a docstring
+ airflow/providers/google/cloud/hooks/datacatalog.py:723:9: D301 Use `r"""` if any backslashes in a docstring
+ airflow/providers/google/cloud/hooks/vertex_ai/model_service.py:159:9: D301 Use `r"""` if any backslashes in a docstring
+ airflow/providers/google/cloud/operators/datacatalog.py:1451:5: D301 Use `r"""` if any backslashes in a docstring
+ airflow/providers/google/cloud/operators/datacatalog.py:526:5: D301 Use `r"""` if any backslashes in a docstring
+ airflow/providers/google/cloud/operators/vertex_ai/model_service.py:189:5: D301 Use `r"""` if any backslashes in a docstring
```

</p>
</details>
<details><summary>zulip (+1, -0)</summary>
<p>

```diff
+ zerver/lib/tex.py:12:5: D301 Use `r"""` if any backslashes in a docstring
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| D301 | 14 | 14 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05      7.6±0.23ms     5.3 MB/sec    1.00      7.2±0.30ms     5.6 MB/sec
formatter/numpy/ctypeslib.py               1.04  1805.5±85.98µs     9.2 MB/sec    1.00  1730.3±90.60µs     9.6 MB/sec
formatter/numpy/globals.py                 1.07   220.0±12.58µs    13.4 MB/sec    1.00   206.4±12.31µs    14.3 MB/sec
formatter/pydantic/types.py                1.04      4.0±0.16ms     6.4 MB/sec    1.00      3.8±0.21ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.03     15.7±0.44ms     2.6 MB/sec    1.00     15.1±0.77ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.0±0.19ms     4.2 MB/sec    1.00      3.9±0.20ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.05   510.5±25.00µs     5.8 MB/sec    1.00   485.4±24.22µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.24ms     3.7 MB/sec    1.00      6.8±0.34ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.22ms     5.2 MB/sec    1.02      8.0±0.64ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1681.0±50.15µs     9.9 MB/sec    1.02  1718.9±112.90µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    204.1±9.41µs    14.5 MB/sec    1.01    206.2±9.80µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.05      3.6±0.17ms     7.1 MB/sec    1.00      3.4±0.16ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.9±0.06ms     5.2 MB/sec    1.00      7.8±0.06ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1794.6±22.30µs     9.3 MB/sec    1.00  1787.3±25.52µs     9.3 MB/sec
formatter/numpy/globals.py                 1.00    215.5±3.71µs    13.7 MB/sec    1.01   218.7±10.07µs    13.5 MB/sec
formatter/pydantic/types.py                1.02      4.1±0.05ms     6.3 MB/sec    1.00      4.0±0.05ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.13ms     2.7 MB/sec    1.01     15.3±0.18ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.08ms     4.1 MB/sec    1.00      4.0±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    498.0±6.45µs     5.9 MB/sec    1.00    498.5±5.82µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.07ms     3.8 MB/sec    1.01      6.8±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      7.9±0.09ms     5.1 MB/sec    1.00      7.9±0.07ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1698.7±16.72µs     9.8 MB/sec    1.00  1699.7±27.85µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.9±2.39µs    14.8 MB/sec    1.00    199.8±2.92µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.1 MB/sec    1.00      3.6±0.03ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
