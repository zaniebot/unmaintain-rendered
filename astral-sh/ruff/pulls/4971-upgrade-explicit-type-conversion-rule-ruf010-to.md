```yaml
number: 4971
title: "Upgrade explicit-type-conversion rule (`RUF010`) to remove unnecessary `str` calls"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/str
created_at: 2023-06-08T19:54:51Z
updated_at: 2023-06-08T20:26:28Z
url: https://github.com/astral-sh/ruff/pull/4971
synced_at: 2026-01-12T15:55:17Z
```

# Upgrade explicit-type-conversion rule (`RUF010`) to remove unnecessary `str` calls

---

_@charliermarsh_

## Summary

It turns out that `f"{str(bla)}"` is redundant, as it's equivalent to `f"{bla}"`. Similarly, `f"{bla!s}"` can be rewritten to `f"{bla}"`.

In both cases, it's only necessary to retain the `!s` if the string contains a format spec, like `f"{bla!s:20}"`.

Closes #4958.


---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-08 19:54_

---

_Label `rule` added by @charliermarsh on 2023-06-08 19:54_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-06-08 19:54_

---

_Merged by @charliermarsh on 2023-06-08 20:02_

---

_Closed by @charliermarsh on 2023-06-08 20:02_

---

_Branch deleted on 2023-06-08 20:02_

---

_Comment by @github-actions[bot] on 2023-06-08 20:06_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+62, -40, 0 error(s))

<details><summary>airflow (+48, -40)</summary>
<p>

```diff
+ airflow/api/common/experimental/get_code.py:40:34: RUF010 [*] Remove unnecessary `str` conversion
- airflow/api/common/experimental/get_code.py:40:34: RUF010 [*] Use conversion in f-string
+ airflow/cli/commands/task_command.py:546:89: RUF010 [*] Remove unnecessary `str` conversion
- airflow/cli/commands/task_command.py:546:89: RUF010 [*] Use conversion in f-string
- airflow/cli/commands/variable_command.py:97:50: RUF010 [*] Use conversion in f-string
+ airflow/cli/commands/variable_command.py:97:50: RUF010 [*] Use explicit conversion flag
+ airflow/exceptions.py:299:49: RUF010 [*] Remove unnecessary `str` conversion
- airflow/exceptions.py:299:49: RUF010 [*] Use conversion in f-string
+ airflow/executors/kubernetes_executor.py:850:66: RUF010 [*] Remove unnecessary `str` conversion
- airflow/executors/kubernetes_executor.py:850:66: RUF010 [*] Use conversion in f-string
+ airflow/policies.py:170:43: RUF010 [*] Remove unnecessary `str` conversion
- airflow/policies.py:170:43: RUF010 [*] Use conversion in f-string
+ airflow/providers/amazon/aws/hooks/dynamodb.py:70:82: RUF010 [*] Remove unnecessary `str` conversion
- airflow/providers/amazon/aws/hooks/dynamodb.py:70:82: RUF010 [*] Use conversion in f-string
+ airflow/providers/amazon/aws/log/cloudwatch_task_handler.py:99:40: RUF010 [*] Remove unnecessary `str` conversion
- airflow/providers/amazon/aws/log/cloudwatch_task_handler.py:99:40: RUF010 [*] Use conversion in f-string
- airflow/providers/apache/hive/hooks/hive.py:928:48: RUF010 [*] Use conversion in f-string
+ airflow/providers/apache/hive/hooks/hive.py:928:48: RUF010 [*] Use explicit conversion flag
- airflow/providers/apache/hive/hooks/hive.py:929:49: RUF010 [*] Use conversion in f-string
+ airflow/providers/apache/hive/hooks/hive.py:929:49: RUF010 [*] Use explicit conversion flag
+ airflow/providers/apache/livy/triggers/livy.py:104:80: RUF010 [*] Remove unnecessary `str` conversion
- airflow/providers/apache/livy/triggers/livy.py:104:80: RUF010 [*] Use conversion in f-string
+ airflow/providers/apache/spark/hooks/spark_submit.py:272:51: RUF010 [*] Remove unnecessary `str` conversion
- airflow/providers/apache/spark/hooks/spark_submit.py:272:51: RUF010 [*] Use conversion in f-string
+ airflow/providers/arangodb/hooks/arangodb.py:112:74: RUF010 [*] Remove unnecessary `str` conversion
- airflow/providers/arangodb/hooks/arangodb.py:112:74: RUF010 [*] Use conversion in f-string
+ airflow/providers/cncf/kubernetes/utils/delete_from.py:140:42: RUF010 [*] Remove unnecessary `str` conversion
- airflow/providers/cncf/kubernetes/utils/delete_from.py:140:42: RUF010 [*] Use conversion in f-string
+ airflow/providers/common/sql/operators/sql.py:415:44: RUF010 [*] Remove unnecessary conversion flag
+ airflow/providers/common/sql/operators/sql.py:620:64: RUF010 [*] Remove unnecessary conversion flag
+ airflow/providers/common/sql/operators/sql.py:714:82: RUF010 [*] Remove unnecessary conversion flag
- airflow/providers/docker/operators/docker.py:411:68: RUF010 [*] Use conversion in f-string
+ airflow/providers/docker/operators/docker.py:411:68: RUF010 [*] Use explicit conversion flag
+ airflow/providers/github/operators/github.py:77:80: RUF010 [*] Remove unnecessary `str` conversion
- airflow/providers/github/operators/github.py:77:80: RUF010 [*] Use conversion in f-string
+ airflow/providers/github/operators/github.py:79:62: RUF010 [*] Remove unnecessary `str` conversion
- airflow/providers/github/operators/github.py:79:62: RUF010 [*] Use conversion in f-string
+ airflow/providers/github/sensors/github.py:142:78: RUF010 [*] Remove unnecessary `str` conversion
- airflow/providers/github/sensors/github.py:142:78: RUF010 [*] Use conversion in f-string
+ airflow/providers/github/sensors/github.py:144:62: RUF010 [*] Remove unnecessary `str` conversion
- airflow/providers/github/sensors/github.py:144:62: RUF010 [*] Use conversion in f-string
+ airflow/providers/google/cloud/example_dags/example_vertex_ai.py:226:30: RUF010 [*] Remove unnecessary `str` conversion
- airflow/providers/google/cloud/example_dags/example_vertex_ai.py:226:30: RUF010 [*] Use conversion in f-string
+ airflow/providers/google/cloud/log/gcs_task_handler.py:250:88: RUF010 [*] Remove unnecessary `str` conversion
- airflow/providers/google/cloud/log/gcs_task_handler.py:250:88: RUF010 [*] Use conversion in f-string
+ airflow/providers/google/cloud/operators/bigquery.py:270:64: RUF010 [*] Remove unnecessary conversion flag
+ airflow/providers/google/cloud/operators/bigquery.py:653:44: RUF010 [*] Remove unnecessary conversion flag
+ airflow/providers/google/cloud/operators/bigquery.py:756:64: RUF010 [*] Remove unnecessary conversion flag
+ airflow/providers/google/cloud/triggers/bigquery_dts.py:138:70: RUF010 [*] Remove unnecessary `str` conversion
- airflow/providers/google/cloud/triggers/bigquery_dts.py:138:70: RUF010 [*] Use conversion in f-string
+ airflow/providers/grpc/hooks/grpc.py:115:33: RUF010 [*] Remove unnecessary `str` conversion
- airflow/providers/grpc/hooks/grpc.py:115:33: RUF010 [*] Use conversion in f-string
+ airflow/providers/microsoft/winrm/operators/winrm.py:144:61: RUF010 [*] Remove unnecessary `str` conversion
- airflow/providers/microsoft/winrm/operators/winrm.py:144:61: RUF010 [*] Use conversion in f-string
+ airflow/providers/sftp/operators/sftp.py:188:83: RUF010 [*] Remove unnecessary `str` conversion
- airflow/providers/sftp/operators/sftp.py:188:83: RUF010 [*] Use conversion in f-string
+ airflow/serialization/serialized_objects.py:565:45: RUF010 [*] Remove unnecessary conversion flag
+ airflow/task/task_runner/cgroup_task_runner.py:137:84: RUF010 [*] Remove unnecessary `str` conversion
- airflow/task/task_runner/cgroup_task_runner.py:137:84: RUF010 [*] Use conversion in f-string
+ airflow/utils/log/file_task_handler.py:521:60: RUF010 [*] Remove unnecessary `str` conversion
- airflow/utils/log/file_task_handler.py:521:60: RUF010 [*] Use conversion in f-string
+ airflow/www/views.py:1632:80: RUF010 [*] Remove unnecessary `str` conversion
- airflow/www/views.py:1632:80: RUF010 [*] Use conversion in f-string
+ tests/always/test_example_dags.py:85:61: RUF010 [*] Remove unnecessary `str` conversion
- tests/always/test_example_dags.py:85:61: RUF010 [*] Use conversion in f-string
+ tests/integration/executors/test_celery_executor.py:246:56: RUF010 [*] Remove unnecessary `str` conversion
- tests/integration/executors/test_celery_executor.py:246:56: RUF010 [*] Use conversion in f-string
+ tests/models/test_baseoperator.py:53:56: RUF010 [*] Remove unnecessary `str` conversion
- tests/models/test_baseoperator.py:53:56: RUF010 [*] Use conversion in f-string
+ tests/models/test_renderedtifields.py:51:56: RUF010 [*] Remove unnecessary `str` conversion
- tests/models/test_renderedtifields.py:51:56: RUF010 [*] Use conversion in f-string
+ tests/providers/amazon/aws/operators/test_emr_serverless.py:543:48: RUF010 [*] Remove unnecessary `str` conversion
- tests/providers/amazon/aws/operators/test_emr_serverless.py:543:48: RUF010 [*] Use conversion in f-string
+ tests/providers/amazon/aws/utils/eks_test_utils.py:117:42: RUF010 [*] Remove unnecessary `str` conversion
- tests/providers/amazon/aws/utils/eks_test_utils.py:117:42: RUF010 [*] Use conversion in f-string
+ tests/providers/amazon/aws/utils/eks_test_utils.py:140:39: RUF010 [*] Remove unnecessary `str` conversion
- tests/providers/amazon/aws/utils/eks_test_utils.py:140:39: RUF010 [*] Use conversion in f-string
+ tests/providers/amazon/aws/utils/eks_test_utils.py:95:48: RUF010 [*] Remove unnecessary `str` conversion
- tests/providers/amazon/aws/utils/eks_test_utils.py:95:48: RUF010 [*] Use conversion in f-string
+ tests/providers/google/cloud/operators/test_bigquery.py:1694:81: RUF010 [*] Remove unnecessary conversion flag
+ tests/serialization/test_dag_serialization.py:1055:49: RUF010 [*] Remove unnecessary `str` conversion
- tests/serialization/test_dag_serialization.py:1055:49: RUF010 [*] Use conversion in f-string
+ tests/system/providers/github/example_github.py:64:78: RUF010 [*] Remove unnecessary `str` conversion
- tests/system/providers/github/example_github.py:64:78: RUF010 [*] Use conversion in f-string
+ tests/system/providers/github/example_github.py:66:62: RUF010 [*] Remove unnecessary `str` conversion
- tests/system/providers/github/example_github.py:66:62: RUF010 [*] Use conversion in f-string
+ tests/system/providers/google/marketing_platform/example_campaign_manager.py:145:37: RUF010 [*] Remove unnecessary `str` conversion
- tests/system/providers/google/marketing_platform/example_campaign_manager.py:145:37: RUF010 [*] Use conversion in f-string
```

</p>
</details>
<details><summary>bokeh (+4, -0)</summary>
<p>

```diff
+ src/bokeh/models/widgets/sliders.py:112:27: RUF010 [*] Remove unnecessary conversion flag
+ src/bokeh/models/widgets/sliders.py:112:47: RUF010 [*] Remove unnecessary conversion flag
+ src/bokeh/util/token.py:287:51: RUF010 [*] Remove unnecessary conversion flag
+ tests/test_cross.py:188:45: RUF010 [*] Remove unnecessary conversion flag
```

</p>
</details>
<details><summary>disnake (+10, -0)</summary>
<p>

```diff
+ disnake/activity.py:298:29: RUF010 [*] Remove unnecessary conversion flag
+ disnake/audit_logs.py:300:30: RUF010 [*] Remove unnecessary conversion flag
+ disnake/channel.py:1279:30: RUF010 [*] Remove unnecessary conversion flag
+ disnake/channel.py:1963:30: RUF010 [*] Remove unnecessary conversion flag
+ disnake/channel.py:215:30: RUF010 [*] Remove unnecessary conversion flag
+ disnake/channel.py:3262:30: RUF010 [*] Remove unnecessary conversion flag
+ disnake/client.py:181:101: RUF010 [*] Remove unnecessary conversion flag
+ disnake/guild.py:430:29: RUF010 [*] Remove unnecessary conversion flag
+ disnake/guild_scheduled_event.py:206:29: RUF010 [*] Remove unnecessary conversion flag
+ disnake/member.py:163:29: RUF010 [*] Remove unnecessary conversion flag
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| RUF010 | 102 | 62 | 40 |

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.05      7.8±0.47ms     5.2 MB/sec     1.00      7.4±0.42ms     5.5 MB/sec
formatter/numpy/ctypeslib.py               1.09  1428.2±101.88µs    11.7 MB/sec    1.00  1307.1±92.73µs    12.7 MB/sec
formatter/numpy/globals.py                 1.05    159.3±8.46µs    18.5 MB/sec     1.00   151.8±12.84µs    19.4 MB/sec
formatter/pydantic/types.py                1.11      3.4±0.15ms     7.6 MB/sec     1.00      3.0±0.18ms     8.5 MB/sec
linter/all-rules/large/dataset.py          1.00     16.7±0.81ms     2.4 MB/sec     1.04     17.3±1.21ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.0±0.21ms     4.1 MB/sec     1.00      3.9±0.23ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.06   531.4±20.80µs     5.6 MB/sec     1.00   501.2±29.35µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.07      7.4±0.30ms     3.5 MB/sec     1.00      6.9±0.39ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.43ms     5.0 MB/sec     1.01      8.1±0.34ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1689.6±94.98µs     9.9 MB/sec     1.01  1711.5±118.25µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.02   196.8±16.30µs    15.0 MB/sec     1.00   192.4±11.65µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.06      3.8±0.23ms     6.7 MB/sec     1.00      3.6±0.21ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.7±0.07ms     5.3 MB/sec    1.00      7.6±0.05ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1338.5±16.37µs    12.4 MB/sec    1.00  1333.4±34.61µs    12.5 MB/sec
formatter/numpy/globals.py                 1.00    147.0±3.65µs    20.1 MB/sec    1.00    147.2±3.66µs    20.0 MB/sec
formatter/pydantic/types.py                1.01      3.1±0.03ms     8.2 MB/sec    1.00      3.1±0.04ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.00     16.6±0.13ms     2.5 MB/sec    1.00     16.6±0.13ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.06ms     3.9 MB/sec    1.00      4.2±0.07ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   494.8±12.82µs     6.0 MB/sec    1.00    495.5±6.23µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.06ms     3.6 MB/sec    1.00      7.0±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.08ms     4.9 MB/sec    1.00      8.3±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1753.2±16.86µs     9.5 MB/sec    1.00  1757.8±20.06µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    198.1±3.93µs    14.9 MB/sec    1.01    200.0±2.96µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.8 MB/sec    1.00      3.7±0.03ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
