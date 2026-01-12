```yaml
number: 5362
title: Add tests for escape-sequence-in-docstring
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/backslash
created_at: 2023-06-26T02:23:23Z
updated_at: 2023-06-26T02:51:32Z
url: https://github.com/astral-sh/ruff/pull/5362
synced_at: 2026-01-12T03:36:55Z
```

# Add tests for escape-sequence-in-docstring

---

_Pull request opened by @charliermarsh on 2023-06-26 02:23_

## Summary

Looks like I added a regression in #5360. This PR fixes it and adds dedicated tests to avoid it in the future.

---

_Comment by @github-actions[bot] on 2023-06-26 02:33_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -14, 0 error(s))

<details><summary>airflow (+0, -13)</summary>
<p>

```diff
- airflow/kubernetes/pod_generator.py:550:9: D301 Use `r"""` if any backslashes in a docstring
- airflow/kubernetes/pod_generator_deprecated.py:293:9: D301 Use `r"""` if any backslashes in a docstring
- airflow/models/baseoperator.py:1613:5: D301 Use `r"""` if any backslashes in a docstring
- airflow/models/baseoperator.py:1745:5: D301 Use `r"""` if any backslashes in a docstring
- airflow/operators/bash.py:34:5: D301 Use `r"""` if any backslashes in a docstring
- airflow/providers/cncf/kubernetes/utils/pod_manager.py:184:9: D301 Use `r"""` if any backslashes in a docstring
- airflow/providers/docker/operators/docker.py:503:9: D301 Use `r"""` if any backslashes in a docstring
- airflow/providers/google/cloud/hooks/datacatalog.py:301:9: D301 Use `r"""` if any backslashes in a docstring
- airflow/providers/google/cloud/hooks/datacatalog.py:723:9: D301 Use `r"""` if any backslashes in a docstring
- airflow/providers/google/cloud/hooks/vertex_ai/model_service.py:159:9: D301 Use `r"""` if any backslashes in a docstring
- airflow/providers/google/cloud/operators/datacatalog.py:1451:5: D301 Use `r"""` if any backslashes in a docstring
- airflow/providers/google/cloud/operators/datacatalog.py:526:5: D301 Use `r"""` if any backslashes in a docstring
- airflow/providers/google/cloud/operators/vertex_ai/model_service.py:189:5: D301 Use `r"""` if any backslashes in a docstring
```

</p>
</details>
<details><summary>zulip (+0, -1)</summary>
<p>

```diff
- zerver/lib/tex.py:12:5: D301 Use `r"""` if any backslashes in a docstring
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| D301 | 14 | 0 | 14 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.7±0.05ms     6.1 MB/sec    1.00      6.7±0.05ms     6.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1554.3±5.87µs    10.7 MB/sec    1.01   1565.6±2.48µs    10.6 MB/sec
formatter/numpy/globals.py                 1.00    188.3±0.93µs    15.7 MB/sec    1.00    189.1±1.85µs    15.6 MB/sec
formatter/pydantic/types.py                1.01      3.5±0.01ms     7.3 MB/sec    1.00      3.5±0.01ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.01     13.5±0.09ms     3.0 MB/sec    1.00     13.4±0.07ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.02ms     5.0 MB/sec    1.00      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    429.0±1.03µs     6.9 MB/sec    1.00    430.9±1.37µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.05ms     4.3 MB/sec    1.02      6.0±0.05ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      6.8±0.03ms     6.0 MB/sec    1.00      6.7±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1500.3±1.86µs    11.1 MB/sec    1.00   1491.1±1.70µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.2±0.21µs    17.3 MB/sec    1.01    171.5±0.82µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.0±0.08ms     6.8 MB/sec    1.04      6.2±0.09ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1306.1±10.62µs    12.7 MB/sec    1.03  1342.4±13.51µs    12.4 MB/sec
formatter/numpy/globals.py                 1.00    149.7±2.99µs    19.7 MB/sec    1.01    151.5±2.69µs    19.5 MB/sec
formatter/pydantic/types.py                1.00      3.0±0.03ms     8.4 MB/sec    1.03      3.1±0.08ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.00     11.0±0.06ms     3.7 MB/sec    1.00     11.0±0.10ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.0±0.02ms     5.6 MB/sec    1.00      3.0±0.03ms     5.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    309.8±3.80µs     9.5 MB/sec    1.00    310.6±3.85µs     9.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.0±0.03ms     5.1 MB/sec    1.00      5.0±0.05ms     5.1 MB/sec
linter/default-rules/large/dataset.py      1.01      5.8±0.06ms     7.0 MB/sec    1.00      5.8±0.07ms     7.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1210.5±8.73µs    13.8 MB/sec    1.00  1206.8±15.99µs    13.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    129.9±1.26µs    22.7 MB/sec    1.00    128.3±1.52µs    23.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.7±0.02ms     9.6 MB/sec    1.00      2.6±0.03ms     9.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-26 02:42_

---

_Closed by @charliermarsh on 2023-06-26 02:42_

---

_Branch deleted on 2023-06-26 02:42_

---

_Label `internal` added by @charliermarsh on 2023-06-26 02:42_

---
