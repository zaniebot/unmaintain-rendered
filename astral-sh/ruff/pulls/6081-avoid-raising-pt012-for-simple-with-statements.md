```yaml
number: 6081
title: "Avoid raising PT012 for simple `with` statements"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: PT012-allow-simple-statement
created_at: 2023-07-26T01:10:41Z
updated_at: 2023-07-26T02:32:12Z
url: https://github.com/astral-sh/ruff/pull/6081
synced_at: 2026-01-12T15:55:20Z
```

# Avoid raising PT012 for simple `with` statements

---

_@harupy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Close #6054

## Test Plan

<!-- How was it tested? -->

A new test case.


---

_Review comment by @harupy on `crates/ruff/src/rules/flake8_pytest_style/rules/raises.rs`:103 on 2023-07-26 01:11_

There is the exact same function here:

https://github.com/astral-sh/ruff/blob/da33c26238d359e9e7a4d0549fc25b1e5c1ce7fc/crates/ruff_python_formatter/src/statement/suite.rs#L145

Should we reuse this?

---

_@harupy reviewed on 2023-07-26 01:11_

---

_@charliermarsh reviewed on 2023-07-26 01:30_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pytest_style/rules/raises.rs`:103 on 2023-07-26 01:30_

I'll move it to `ruff_python_ast`...

---

_Merged by @charliermarsh on 2023-07-26 01:43_

---

_Closed by @charliermarsh on 2023-07-26 01:43_

---

_Branch deleted on 2023-07-26 01:43_

---

_Comment by @github-actions[bot] on 2023-07-26 01:46_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+9, -14, 0 error(s))

<details><summary>airflow (+4, -14)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/f2108892e89085f695f8a3f52e076b39288497c6/tests/core/test_sqlalchemy_config.py#L104'>tests/core/test_sqlalchemy_config.py:104:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
+ <a href='https://github.com/apache/airflow/blob/f2108892e89085f695f8a3f52e076b39288497c6/tests/decorators/test_python.py#L224'>tests/decorators/test_python.py:224:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
- <a href='https://github.com/apache/airflow/blob/f2108892e89085f695f8a3f52e076b39288497c6/tests/decorators/test_python.py#L426'>tests/decorators/test_python.py:426:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
- <a href='https://github.com/apache/airflow/blob/f2108892e89085f695f8a3f52e076b39288497c6/tests/models/test_baseoperator.py#L817'>tests/models/test_baseoperator.py:817:5:</a> PT012 `pytest.raises()` block should contain a single simple statement
- <a href='https://github.com/apache/airflow/blob/f2108892e89085f695f8a3f52e076b39288497c6/tests/providers/amazon/aws/hooks/test_batch_client.py#L167'>tests/providers/amazon/aws/hooks/test_batch_client.py:167:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
- <a href='https://github.com/apache/airflow/blob/f2108892e89085f695f8a3f52e076b39288497c6/tests/providers/amazon/aws/hooks/test_rds.py#L336'>tests/providers/amazon/aws/hooks/test_rds.py:336:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
- <a href='https://github.com/apache/airflow/blob/f2108892e89085f695f8a3f52e076b39288497c6/tests/providers/cncf/kubernetes/operators/test_pod.py#L1246'>tests/providers/cncf/kubernetes/operators/test_pod.py:1246:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
- <a href='https://github.com/apache/airflow/blob/f2108892e89085f695f8a3f52e076b39288497c6/tests/providers/cncf/kubernetes/operators/test_pod.py#L1256'>tests/providers/cncf/kubernetes/operators/test_pod.py:1256:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
- <a href='https://github.com/apache/airflow/blob/f2108892e89085f695f8a3f52e076b39288497c6/tests/providers/google/cloud/hooks/test_dataflow.py#L522'>tests/providers/google/cloud/hooks/test_dataflow.py:522:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
- <a href='https://github.com/apache/airflow/blob/f2108892e89085f695f8a3f52e076b39288497c6/tests/providers/google/common/hooks/test_base_google.py#L327'>tests/providers/google/common/hooks/test_base_google.py:327:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
- <a href='https://github.com/apache/airflow/blob/f2108892e89085f695f8a3f52e076b39288497c6/tests/providers/google/common/hooks/test_base_google.py#L348'>tests/providers/google/common/hooks/test_base_google.py:348:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
- <a href='https://github.com/apache/airflow/blob/f2108892e89085f695f8a3f52e076b39288497c6/tests/providers/google/common/hooks/test_base_google.py#L723'>tests/providers/google/common/hooks/test_base_google.py:723:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
- <a href='https://github.com/apache/airflow/blob/f2108892e89085f695f8a3f52e076b39288497c6/tests/providers/slack/operators/test_slack_webhook.py#L77'>tests/providers/slack/operators/test_slack_webhook.py:77:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
- <a href='https://github.com/apache/airflow/blob/f2108892e89085f695f8a3f52e076b39288497c6/tests/providers/smtp/hooks/test_smtp.py#L130'>tests/providers/smtp/hooks/test_smtp.py:130:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
- <a href='https://github.com/apache/airflow/blob/f2108892e89085f695f8a3f52e076b39288497c6/tests/providers/smtp/hooks/test_smtp.py#L136'>tests/providers/smtp/hooks/test_smtp.py:136:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
+ <a href='https://github.com/apache/airflow/blob/f2108892e89085f695f8a3f52e076b39288497c6/tests/utils/test_retries.py#L78'>tests/utils/test_retries.py:78:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
+ <a href='https://github.com/apache/airflow/blob/f2108892e89085f695f8a3f52e076b39288497c6/tests/utils/test_session.py#L30'>tests/utils/test_session.py:30:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
+ <a href='https://github.com/apache/airflow/blob/f2108892e89085f695f8a3f52e076b39288497c6/tests/utils/test_task_group.py#L1091'>tests/utils/test_task_group.py:1091:5:</a> PT012 `pytest.raises()` block should contain a single simple statement
</pre>

</p>
</details>
<details><summary>disnake (+5, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a309929c9b7c43eba5fa6103571d3fa562cb6def/tests/ext/commands/test_base_core.py#L33'>tests/ext/commands/test_base_core.py:33:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
+ <a href='https://github.com/DisnakeDev/disnake/blob/a309929c9b7c43eba5fa6103571d3fa562cb6def/tests/ext/tasks/test_loops.py#L22'>tests/ext/tasks/test_loops.py:22:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
+ <a href='https://github.com/DisnakeDev/disnake/blob/a309929c9b7c43eba5fa6103571d3fa562cb6def/tests/ext/tasks/test_loops.py#L35'>tests/ext/tasks/test_loops.py:35:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
+ <a href='https://github.com/DisnakeDev/disnake/blob/a309929c9b7c43eba5fa6103571d3fa562cb6def/tests/ext/tasks/test_loops.py#L76'>tests/ext/tasks/test_loops.py:76:9:</a> PT012 `pytest.raises()` block should contain a single simple statement
+ <a href='https://github.com/DisnakeDev/disnake/blob/a309929c9b7c43eba5fa6103571d3fa562cb6def/tests/test_flags.py#L96'>tests/test_flags.py:96:5:</a> PT012 `pytest.raises()` block should contain a single simple statement
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PT012 | 23 | 9 | 14 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.2±0.03ms     4.4 MB/sec    1.00      9.2±0.02ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1859.1±4.43µs     9.0 MB/sec    1.00   1857.8±4.57µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    207.5±0.67µs    14.2 MB/sec    1.00    207.9±2.25µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.02ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.01     12.6±0.04ms     3.2 MB/sec    1.00     12.5±0.03ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.2±0.00ms     5.2 MB/sec    1.00      3.2±0.00ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    425.3±0.83µs     6.9 MB/sec    1.00    424.6±3.67µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.02ms     6.2 MB/sec    1.00      6.6±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1402.5±4.29µs    11.9 MB/sec    1.00   1405.7±2.99µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.6±0.63µs    19.0 MB/sec    1.00    155.6±0.20µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.00ms     8.6 MB/sec    1.01      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.7±0.09ms     3.8 MB/sec    1.00     10.7±0.08ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.1±0.03ms     7.8 MB/sec    1.00      2.1±0.02ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    240.0±4.27µs    12.3 MB/sec    1.01    242.5±7.49µs    12.2 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.06ms     5.5 MB/sec    1.00      4.7±0.06ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.00     14.8±0.11ms     2.7 MB/sec    1.00     14.8±0.10ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.03ms     4.2 MB/sec    1.00      3.9±0.04ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    477.8±5.79µs     6.2 MB/sec    1.00    475.9±9.10µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.07ms     3.7 MB/sec    1.00      6.8±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.06ms     5.1 MB/sec    1.00      8.0±0.05ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1670.5±16.21µs    10.0 MB/sec    1.00  1664.0±16.15µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    194.9±3.60µs    15.1 MB/sec    1.00    193.5±2.44µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.06ms     7.1 MB/sec    1.00      3.6±0.03ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @harupy on 2023-07-26 01:59_

https://github.com/apache/airflow/blob/f2108892e89085f695f8a3f52e076b39288497c6/tests/utils/test_task_group.py#L1091

```python
def test_decorator_unknown_args():
    """Test that unknown args passed to the decorator cause an error at parse time"""
    with pytest.raises(TypeError):

        @task_group_decorator(b=2)
        def tg():
            ...
```

Should function/class be allowed to test decorators?

---

_Comment by @harupy on 2023-07-26 02:21_

@charliermarsh wdyt? I think function/class def should be allowed.

---
