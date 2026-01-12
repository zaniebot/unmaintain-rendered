```yaml
number: 5467
title: Detect consecutive, non-newline-delimited NumPy sections
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - docstring
assignees: []
merged: true
base: main
head: charlie/D410
created_at: 2023-07-03T00:16:08Z
updated_at: 2023-07-03T00:50:43Z
url: https://github.com/astral-sh/ruff/pull/5467
synced_at: 2026-01-12T03:36:55Z
```

# Detect consecutive, non-newline-delimited NumPy sections

---

_Pull request opened by @charliermarsh on 2023-07-03 00:16_

## Summary

Given a docstring like:

```py
def f(a: int, b: int) -> int:
    """Showcase function.
    Parameters
    ----------
    a : int
        _description_
    b : int
        _description_
    Returns
    -------
    int
        _description
    """
```

We were failing to identify `Returns` as a section, because the previous line was neither empty nor ended with punctuation. This was causing a false negative, where by we weren't flagging a missing line before `Returns`. So, the very reason for the rule (no blank line) was causing us to fail to catch it.

Note that, we did have a test case for this, which was working properly:

```py
def f() -> int:
    """Showcase function.
    Parameters
    ----------
    Returns
    -------
    """
```

...because the line before `Returns` "ends in a punctuation mark" (`-`).

Closes #5442.

---

_Comment by @charliermarsh on 2023-07-03 00:16_

Our behavior now deviates from pydocstyle, which also has this bug.

---

_Label `bug` added by @charliermarsh on 2023-07-03 00:16_

---

_Label `docstring` added by @charliermarsh on 2023-07-03 00:16_

---

_Comment by @github-actions[bot] on 2023-07-03 00:28_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+20, -20, 0 error(s))

<details><summary>airflow (+20, -20)</summary>
<p>

```diff
+ tests/jobs/test_scheduler_job.py:1698:14: B017 `pytest.raises(Exception)` should be considered evil
- tests/jobs/test_scheduler_job.py:1698:9: B017 `pytest.raises(Exception)` should be considered evil
+ tests/providers/amazon/aws/hooks/test_base_aws.py:1016:14: B017 `pytest.raises(Exception)` should be considered evil
- tests/providers/amazon/aws/hooks/test_base_aws.py:1016:9: B017 `pytest.raises(Exception)` should be considered evil
+ tests/providers/amazon/aws/hooks/test_base_aws.py:1021:14: B017 `pytest.raises(Exception)` should be considered evil
- tests/providers/amazon/aws/hooks/test_base_aws.py:1021:9: B017 `pytest.raises(Exception)` should be considered evil
+ tests/providers/amazon/aws/hooks/test_glue_catalog.py:118:14: B017 `pytest.raises(Exception)` should be considered evil
- tests/providers/amazon/aws/hooks/test_glue_catalog.py:118:9: B017 `pytest.raises(Exception)` should be considered evil
- tests/providers/amazon/aws/log/test_s3_task_handler.py:107:13: B017 `pytest.raises(Exception)` should be considered evil
+ tests/providers/amazon/aws/log/test_s3_task_handler.py:107:18: B017 `pytest.raises(Exception)` should be considered evil
+ tests/providers/amazon/aws/operators/test_athena.py:114:14: B017 `pytest.raises(Exception)` should be considered evil
- tests/providers/amazon/aws/operators/test_athena.py:114:9: B017 `pytest.raises(Exception)` should be considered evil
+ tests/providers/amazon/aws/operators/test_athena.py:129:14: B017 `pytest.raises(Exception)` should be considered evil
- tests/providers/amazon/aws/operators/test_athena.py:129:9: B017 `pytest.raises(Exception)` should be considered evil
+ tests/providers/amazon/aws/operators/test_athena.py:143:14: B017 `pytest.raises(Exception)` should be considered evil
- tests/providers/amazon/aws/operators/test_athena.py:143:9: B017 `pytest.raises(Exception)` should be considered evil
+ tests/providers/amazon/aws/utils/test_task_log_fetcher.py:92:14: B017 `pytest.raises(Exception)` should be considered evil
- tests/providers/amazon/aws/utils/test_task_log_fetcher.py:92:9: B017 `pytest.raises(Exception)` should be considered evil
+ tests/providers/apache/beam/hooks/test_beam.py:372:14: B017 `pytest.raises(Exception)` should be considered evil
- tests/providers/apache/beam/hooks/test_beam.py:372:9: B017 `pytest.raises(Exception)` should be considered evil
+ tests/providers/google/cloud/hooks/test_dataflow.py:1939:14: B017 `pytest.raises(Exception)` should be considered evil
- tests/providers/google/cloud/hooks/test_dataflow.py:1939:9: B017 `pytest.raises(Exception)` should be considered evil
+ tests/providers/google/cloud/hooks/test_dataproc.py:1030:14: B017 `pytest.raises(Exception)` should be considered evil
- tests/providers/google/cloud/hooks/test_dataproc.py:1030:9: B017 `pytest.raises(Exception)` should be considered evil
+ tests/providers/google/common/hooks/test_base_google.py:251:14: B017 `pytest.raises(Exception)` should be considered evil
- tests/providers/google/common/hooks/test_base_google.py:251:9: B017 `pytest.raises(Exception)` should be considered evil
+ tests/providers/google/common/hooks/test_base_google.py:277:14: B017 `pytest.raises(Exception)` should be considered evil
- tests/providers/google/common/hooks/test_base_google.py:277:9: B017 `pytest.raises(Exception)` should be considered evil
+ tests/providers/google/common/hooks/test_base_google.py:327:14: B017 `pytest.raises(Exception)` should be considered evil
- tests/providers/google/common/hooks/test_base_google.py:327:9: B017 `pytest.raises(Exception)` should be considered evil
+ tests/providers/google/common/hooks/test_base_google.py:348:14: B017 `pytest.raises(Exception)` should be considered evil
- tests/providers/google/common/hooks/test_base_google.py:348:9: B017 `pytest.raises(Exception)` should be considered evil
- tests/providers/microsoft/azure/hooks/test_azure_synapse.py:174:13: B017 `pytest.raises(Exception)` should be considered evil
+ tests/providers/microsoft/azure/hooks/test_azure_synapse.py:174:18: B017 `pytest.raises(Exception)` should be considered evil
+ tests/providers/mysql/transfers/test_s3_to_mysql.py:89:14: B017 `pytest.raises(Exception)` should be considered evil
- tests/providers/mysql/transfers/test_s3_to_mysql.py:89:9: B017 `pytest.raises(Exception)` should be considered evil
+ tests/providers/openlineage/plugins/test_openlineage_adapter.py:71:10: B017 `pytest.raises(Exception)` should be considered evil
- tests/providers/openlineage/plugins/test_openlineage_adapter.py:71:5: B017 `pytest.raises(Exception)` should be considered evil
+ tests/providers/ssh/hooks/test_ssh.py:943:14: B017 `pytest.raises(Exception)` should be considered evil
- tests/providers/ssh/hooks/test_ssh.py:943:9: B017 `pytest.raises(Exception)` should be considered evil
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| B017 | 40 | 20 | 20 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.31ms     4.0 MB/sec    1.02     10.3±0.27ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.05ms     7.5 MB/sec    1.01      2.2±0.04ms     7.4 MB/sec
formatter/numpy/globals.py                 1.01   254.8±26.85µs    11.6 MB/sec    1.00    253.4±6.89µs    11.6 MB/sec
formatter/pydantic/types.py                1.00      4.9±0.14ms     5.2 MB/sec    1.00      4.9±0.12ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.00     17.8±0.34ms     2.3 MB/sec    1.06     18.8±0.42ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.08ms     3.9 MB/sec    1.06      4.6±0.17ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   566.6±13.87µs     5.2 MB/sec    1.02   578.2±15.43µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.28ms     3.2 MB/sec    1.03      8.1±0.19ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      8.9±0.21ms     4.6 MB/sec    1.13     10.0±0.25ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1910.0±29.59µs     8.7 MB/sec    1.10      2.1±0.05ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    226.0±7.05µs    13.1 MB/sec    1.06    240.3±9.03µs    12.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.07ms     6.3 MB/sec    1.11      4.4±0.18ms     5.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.06ms     4.4 MB/sec    1.02      9.5±0.08ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1989.4±32.73µs     8.4 MB/sec    1.02      2.0±0.04ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00    216.3±2.65µs    13.6 MB/sec    1.01    218.7±7.29µs    13.5 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.05ms     5.7 MB/sec    1.01      4.5±0.05ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.00     15.6±0.23ms     2.6 MB/sec    1.00     15.5±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.1 MB/sec    1.02      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.6±6.35µs     6.9 MB/sec    1.04    443.4±8.52µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.05ms     3.7 MB/sec    1.03      7.0±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.11ms     5.0 MB/sec    1.00      8.2±0.15ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1658.7±12.70µs    10.0 MB/sec    1.02  1687.7±10.93µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    182.2±2.22µs    16.2 MB/sec    1.01    184.3±1.75µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     7.0 MB/sec    1.00      3.7±0.03ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-03 00:29_

---

_Closed by @charliermarsh on 2023-07-03 00:29_

---

_Branch deleted on 2023-07-03 00:29_

---
