```yaml
number: 4366
title: "Implement pygrep-hook's Mock-mistake diagnostic"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/pygrep
created_at: 2023-05-11T03:18:29Z
updated_at: 2023-05-11T03:54:05Z
url: https://github.com/astral-sh/ruff/pull/4366
synced_at: 2026-01-12T15:55:15Z
```

# Implement pygrep-hook's Mock-mistake diagnostic

---

_@charliermarsh_

Adds a new rule, `PGH005`, to close out `pygrep-hooks`.

This rule checks for common mistakes when interacting with mocks, e.g., using `assert my_mock.called_once_with()` instead of `my_mock.assert_called_once_with()`.

Closes #980.

---

_Label `rule` added by @charliermarsh on 2023-05-11 03:18_

---

_Comment by @charliermarsh on 2023-05-11 03:18_

Random PR but I just want to close out the issue and remove the TODOs from the README etc.

---

_Merged by @charliermarsh on 2023-05-11 03:26_

---

_Closed by @charliermarsh on 2023-05-11 03:26_

---

_Branch deleted on 2023-05-11 03:26_

---

_Comment by @github-actions[bot] on 2023-05-11 03:33_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+26, -0, 0 error(s))

<details><summary>airflow (+24, -0)</summary>
<p>

```diff
+ tests/executors/test_celery_executor.py:248:16: PGH005 Non-existent mock method: `called_with`
+ tests/executors/test_celery_executor.py:249:16: PGH005 Non-existent mock method: `called_once`
+ tests/kubernetes/test_client.py:34:16: PGH005 Non-existent mock method: `not_called`
+ tests/kubernetes/test_client.py:39:16: PGH005 Non-existent mock method: `not_called`
+ tests/providers/amazon/aws/operators/test_eks.py:466:9: PGH005 Mock method should be called: `assert_called_once`
+ tests/providers/apache/druid/hooks/test_druid.py:113:16: PGH005 Non-existent mock method: `called_once`
+ tests/providers/apache/druid/hooks/test_druid.py:114:16: PGH005 Non-existent mock method: `called_once`
+ tests/providers/apache/druid/hooks/test_druid.py:137:16: PGH005 Non-existent mock method: `called_once`
+ tests/providers/apache/druid/hooks/test_druid.py:139:16: PGH005 Non-existent mock method: `called_once`
+ tests/providers/apache/druid/hooks/test_druid.py:57:16: PGH005 Non-existent mock method: `called_once`
+ tests/providers/apache/druid/hooks/test_druid.py:58:16: PGH005 Non-existent mock method: `called_once`
+ tests/providers/apache/druid/hooks/test_druid.py:73:16: PGH005 Non-existent mock method: `called_once`
+ tests/providers/apache/druid/hooks/test_druid.py:74:16: PGH005 Non-existent mock method: `called_once`
+ tests/providers/apache/druid/hooks/test_druid.py:93:16: PGH005 Non-existent mock method: `called_once`
+ tests/providers/apache/druid/hooks/test_druid.py:94:16: PGH005 Non-existent mock method: `called_once`
+ tests/providers/microsoft/azure/hooks/test_adx.py:107:16: PGH005 Non-existent mock method: `called_with`
+ tests/providers/microsoft/azure/hooks/test_adx.py:132:16: PGH005 Non-existent mock method: `called_with`
+ tests/providers/microsoft/azure/hooks/test_adx.py:158:16: PGH005 Non-existent mock method: `called_with`
+ tests/providers/microsoft/azure/hooks/test_adx.py:176:16: PGH005 Non-existent mock method: `called_with`
+ tests/providers/microsoft/azure/hooks/test_adx.py:195:16: PGH005 Non-existent mock method: `called_with`
+ tests/www/test_security.py:914:12: PGH005 Non-existent mock method: `called_once`
+ tests/www/test_security.py:924:12: PGH005 Non-existent mock method: `called_once`
+ tests/www/test_security.py:934:12: PGH005 Non-existent mock method: `called_once`
+ tests/www/test_security.py:944:12: PGH005 Non-existent mock method: `called_once`
```

</p>
</details>
<details><summary>bokeh (+2, -0)</summary>
<p>

```diff
+ tests/unit/bokeh/core/property/test_descriptors.py:86:16: PGH005 Non-existent mock method: `called_once_with`
+ tests/unit/bokeh/core/property/test_descriptors.py:87:16: PGH005 Non-existent mock method: `called_once_with`
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.6±0.02ms     3.0 MB/sec    1.02     13.8±0.10ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    416.1±3.65µs     7.1 MB/sec    1.00    415.8±0.69µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.03ms     4.5 MB/sec    1.01      5.8±0.05ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.0 MB/sec    1.01      6.8±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1453.4±2.73µs    11.5 MB/sec    1.01   1470.2±2.26µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    160.1±0.68µs    18.4 MB/sec    1.02    163.2±2.89µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.02      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.3±0.00ms     7.6 MB/sec    1.02      5.4±0.00ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1037.3±1.34µs    16.1 MB/sec    1.02   1055.6±1.26µs    15.8 MB/sec
parser/numpy/globals.py                    1.00    105.9±0.36µs    27.9 MB/sec    1.02    107.6±0.23µs    27.4 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.2 MB/sec    1.02      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.4±0.63ms     2.2 MB/sec    1.04     19.1±0.62ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.6±0.16ms     3.6 MB/sec    1.03      4.8±0.18ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   571.1±31.89µs     5.2 MB/sec    1.00   573.5±27.88µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.26ms     3.2 MB/sec    1.07      8.4±0.28ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.4±0.32ms     4.3 MB/sec    1.02      9.6±0.39ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.0±0.08ms     8.3 MB/sec    1.00  1983.4±93.51µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    226.0±9.80µs    13.1 MB/sec    1.01   229.2±14.43µs    12.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.21ms     6.0 MB/sec    1.02      4.3±0.21ms     5.9 MB/sec
parser/large/dataset.py                    1.05      8.1±0.19ms     5.0 MB/sec    1.00      7.8±0.21ms     5.2 MB/sec
parser/numpy/ctypeslib.py                  1.06  1537.4±65.61µs    10.8 MB/sec    1.00  1452.8±63.69µs    11.5 MB/sec
parser/numpy/globals.py                    1.00    144.0±9.02µs    20.5 MB/sec    1.04    150.1±8.25µs    19.7 MB/sec
parser/pydantic/types.py                   1.00      3.3±0.30ms     7.7 MB/sec    1.00      3.3±0.11ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
