```yaml
number: 5466
title: Include BaseException in B017 rule
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/base
created_at: 2023-07-02T23:28:50Z
updated_at: 2023-07-03T00:27:58Z
url: https://github.com/astral-sh/ruff/pull/5466
synced_at: 2026-01-12T03:36:55Z
```

# Include BaseException in B017 rule

---

_Pull request opened by @charliermarsh on 2023-07-02 23:28_

Closes #5462.

---

_Comment by @github-actions[bot] on 2023-07-03 00:09_

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
formatter/large/dataset.py                 1.00      7.9±0.04ms     5.2 MB/sec    1.00      7.9±0.05ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1726.1±2.36µs     9.6 MB/sec    1.00   1726.6±2.37µs     9.6 MB/sec
formatter/numpy/globals.py                 1.00    193.1±2.00µs    15.3 MB/sec    1.00    192.8±0.51µs    15.3 MB/sec
formatter/pydantic/types.py                1.02      3.8±0.02ms     6.7 MB/sec    1.00      3.7±0.02ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.02     13.6±0.09ms     3.0 MB/sec    1.00     13.3±0.14ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.03ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    426.8±0.93µs     6.9 MB/sec    1.00    426.7±0.82µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.08ms     4.3 MB/sec    1.00      5.9±0.05ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.02      6.8±0.04ms     6.0 MB/sec    1.00      6.6±0.05ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1467.9±5.33µs    11.3 MB/sec    1.00   1461.1±1.48µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.05   173.9±35.49µs    17.0 MB/sec    1.00    165.4±0.32µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.1±0.01ms     8.3 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.6±0.32ms     4.3 MB/sec    1.00      9.6±0.26ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.07ms     8.1 MB/sec    1.01      2.1±0.08ms     8.0 MB/sec
formatter/numpy/globals.py                 1.02   236.7±36.45µs    12.5 MB/sec    1.00    231.4±6.66µs    12.8 MB/sec
formatter/pydantic/types.py                1.01      4.5±0.11ms     5.6 MB/sec    1.00      4.5±0.09ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.6±0.17ms     2.6 MB/sec    1.02     15.8±0.39ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.10ms     4.0 MB/sec    1.01      4.2±0.14ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    509.1±6.95µs     5.8 MB/sec    1.00   510.7±13.66µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.10ms     3.7 MB/sec    1.01      7.0±0.15ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.06ms     5.0 MB/sec    1.02      8.3±0.19ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1760.6±45.48µs     9.5 MB/sec    1.00  1769.3±78.23µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    210.3±8.82µs    14.0 MB/sec    1.00   208.1±10.76µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.8±0.13ms     6.8 MB/sec    1.00      3.7±0.09ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-03 00:18_

---

_Closed by @charliermarsh on 2023-07-03 00:18_

---

_Branch deleted on 2023-07-03 00:18_

---
