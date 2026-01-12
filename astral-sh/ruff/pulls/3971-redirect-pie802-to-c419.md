```yaml
number: 3971
title: "Redirect `PIE802` to `C419`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/map
created_at: 2023-04-14T01:47:14Z
updated_at: 2023-04-14T02:13:32Z
url: https://github.com/astral-sh/ruff/pull/3971
synced_at: 2026-01-12T15:55:14Z
```

# Redirect `PIE802` to `C419`

---

_@charliermarsh_

This rule was implemented in the latest `flake8-comprehensions` release. `flake8-comprehensions` is a more popular plugin (and the rules are more thematically related), so I'd rather have this categorized under `flake8-comprehensions` than `flake8-pie`.

Closes #3963.


---

_Comment by @github-actions[bot] on 2023-04-14 01:59_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+5, -5, 0 error(s))

<details><summary>airflow (+5, -5)</summary>
<p>

```diff
+ airflow/providers/amazon/aws/hooks/batch_client.py:456:16: C419 [*] Unnecessary list comprehension.
- airflow/providers/amazon/aws/hooks/batch_client.py:456:16: PIE802 [*] Unnecessary list comprehension.
+ dev/breeze/src/airflow_breeze/utils/selective_checks.py:300:13: C419 [*] Unnecessary list comprehension.
- dev/breeze/src/airflow_breeze/utils/selective_checks.py:300:13: PIE802 [*] Unnecessary list comprehension.
+ tests/models/test_taskinstance.py:1860:20: C419 [*] Unnecessary list comprehension.
- tests/models/test_taskinstance.py:1860:20: PIE802 [*] Unnecessary list comprehension.
+ tests/providers/amazon/aws/hooks/test_batch_client.py:364:20: C419 [*] Unnecessary list comprehension.
- tests/providers/amazon/aws/hooks/test_batch_client.py:364:20: PIE802 [*] Unnecessary list comprehension.
+ tests/system/providers/amazon/aws/utils/__init__.py:256:16: C419 [*] Unnecessary list comprehension.
- tests/system/providers/amazon/aws/utils/__init__.py:256:16: PIE802 [*] Unnecessary list comprehension.
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.1±0.08ms     2.7 MB/sec    1.00     15.2±0.07ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.14ms     4.3 MB/sec    1.00      3.8±0.01ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    414.8±7.84µs     7.1 MB/sec    1.01    419.0±1.89µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.08ms     3.9 MB/sec    1.00      6.5±0.02ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.05ms     5.2 MB/sec    1.00      7.9±0.03ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1742.5±11.41µs     9.6 MB/sec    1.01   1754.7±6.21µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.9±3.18µs    16.4 MB/sec    1.02    183.5±0.38µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.0 MB/sec    1.00      3.6±0.02ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     20.7±0.75ms  2017.1 KB/sec    1.00     20.5±0.43ms  2036.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.19ms     3.2 MB/sec    1.00      5.2±0.20ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   622.8±31.13µs     4.7 MB/sec    1.00   619.0±29.87µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.7±0.33ms     2.9 MB/sec    1.00      8.6±0.25ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.2±0.28ms     4.0 MB/sec    1.01     10.2±0.27ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.06ms     7.5 MB/sec    1.01      2.2±0.06ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   237.5±12.64µs    12.4 MB/sec    1.02   243.0±13.29µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.16ms     5.5 MB/sec    1.02      4.7±0.15ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-14 02:12_

---

_Closed by @charliermarsh on 2023-04-14 02:12_

---

_Branch deleted on 2023-04-14 02:12_

---
