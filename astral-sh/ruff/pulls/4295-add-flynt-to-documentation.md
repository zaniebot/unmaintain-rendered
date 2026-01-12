```yaml
number: 4295
title: "Add `flynt` to documentation"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: flynt-static-string-joins
created_at: 2023-05-09T00:47:55Z
updated_at: 2023-05-09T01:16:21Z
url: https://github.com/astral-sh/ruff/pull/4295
synced_at: 2026-01-12T15:55:15Z
```

# Add `flynt` to documentation

---

_@charliermarsh_

## Summary

Also removing the unused `fixable` property from the violation.

---

_Merged by @charliermarsh on 2023-05-09 00:52_

---

_Closed by @charliermarsh on 2023-05-09 00:52_

---

_Branch deleted on 2023-05-09 00:52_

---

_Comment by @github-actions[bot] on 2023-05-09 00:57_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+34, -0, 0 error(s))

<details><summary>airflow (+25, -0)</summary>
<p>

```diff
+ airflow/api/auth/backend/kerberos_auth.py:150:60: FLY002 [*] Consider `f"negotiate {ctx.kerberos_token}"` instead of string join
+ airflow/providers/amazon/aws/log/s3_task_handler.py:203:23: FLY002 [*] Consider `f"{old_log}\n{log}"` instead of string join
+ airflow/providers/cncf/kubernetes/decorators/kubernetes.py:103:13: FLY002 [*] Consider `f"{write_local_script_file_cmd} && {write_local_input_file_cmd} && {make_xcom_dir_cmd} && {exec_python_cmd}"` instead of string join
+ airflow/providers/google/cloud/log/gcs_task_handler.py:243:19: FLY002 [*] Consider `f"{old_log}\n{log}"` instead of string join
+ airflow/providers/microsoft/azure/log/wasb_task_handler.py:238:19: FLY002 [*] Consider `f"{old_log}\n{log}"` instead of string join
+ airflow/settings.py:63:10: FLY002 [*] Consider `f"  ____________       _____________\n ____    |__( )_________  __/__  /________      __\n____  /| |_  /__  ___/_  /_ __  /_  __ \\_ | /| / /\n___  ___ |  / _  /   _  __/ _  / / /_/ /_ |/ |/ /\n _/_/  |_/_/  /_/    /_/    /_/  \\____/____/|__/"` instead of string join
+ airflow/www/views.py:1388:39: FLY002 [*] Consider `f"{template_field}.{key}"` instead of string join
+ airflow/www/views.py:1390:39: FLY002 [*] Consider `f"{template_field}.{key}"` instead of string join
+ airflow/www/views.py:1395:41: FLY002 [*] Consider `f"{template_field}.{dict_keys}"` instead of string join
+ airflow/www/views.py:496:23: FLY002 [*] Consider `f"{key}.{sub_key}"` instead of string join
+ tests/always/test_connection.py:106:49: FLY002 [*] Consider `f"{key2.decode()},{key1.decode()}"` instead of string join
+ tests/cli/commands/test_kubernetes_command.py:60:22: FLY002 [*] Consider `f"dag_id,task_id,try_number,airflow_version"` instead of string join
+ tests/cli/commands/test_rotate_fernet_key_command.py:100:38: FLY002 [*] Consider `f"{fernet_key2.decode()},{fernet_key1.decode()}"` instead of string join
+ tests/cli/commands/test_rotate_fernet_key_command.py:64:38: FLY002 [*] Consider `f"{fernet_key2.decode()},{fernet_key1.decode()}"` instead of string join
+ tests/dags/test_clear_subdag.py:32:16: FLY002 [*] Consider `f"{dag_name}.{subdag_name}"` instead of string join
+ tests/models/test_variable.py:87:49: FLY002 [*] Consider `f"{key2.decode()},{key1.decode()}"` instead of string join
+ tests/providers/cncf/kubernetes/utils/test_pod_manager.py:250:59: FLY002 [*] Consider `f"{real_timestamp} {log_message}"` instead of string join
+ tests/providers/google/common/hooks/test_base_google.py:522:17: FLY002 [*] Consider `f"https://www.googleapis.com/auth/bigquery,https://www.googleapis.com/auth/devstorage.read_only"` instead of string join
+ tests/providers/google/common/hooks/test_base_google.py:589:17: FLY002 [*] Consider `f"https://www.googleapis.com/auth/bigquery,https://www.googleapis.com/auth/devstorage.read_only"` instead of string join
+ tests/utils/test_dot_renderer.py:181:38: FLY002 [*] Consider `f'digraph example_task_group {{\n\tgraph [label=example_task_group labelloc=t rankdir=LR]\n\tend [color="#000000" fillcolor="#e8f7e4" label=end shape=rectangle style="filled,rounded"]\n\tsubgraph cluster_section_1 {{\n\t\tcolor="#000000" fillcolor="#6495ed7f" label=section_1 shape=rectangle style=filled\n\t\t"section_1.upstream_join_id" [color="#000000" fillcolor=CornflowerBlue height=0.2 label="" shape=circle style="filled,rounded" width=0.2]\n\t\t"section_1.downstream_join_id" [color="#000000" fillcolor=CornflowerBlue height=0.2 label="" shape=circle style="filled,rounded" width=0.2]\n\t\t"section_1.task_1" [color="#000000" fillcolor="#e8f7e4" label=task_1 shape=rectangle style="filled,rounded"]\n\t\t"section_1.task_2" [color="#000000" fillcolor="#f0ede4" label=task_2 shape=rectangle style="filled,rounded"]\n\t\t"section_1.task_3" [color="#000000" fillcolor="#e8f7e4" label=task_3 shape=rectangle style="filled,rounded"]\n\t}}\n\tsubgraph cluster_section_2 {{\n\t\tcolor="#000000" fillcolor="#6495ed7f" label=section_2 shape=rectangle style=filled\n\t\t"section_2.upstream_join_id" [color="#000000" fillcolor=CornflowerBlue height=0.2 label="" shape=circle style="filled,rounded" width=0.2]\n\t\t"section_2.downstream_join_id" [color="#000000" fillcolor=CornflowerBlue height=0.2 label="" shape=circle style="filled,rounded" width=0.2]\n\t\tsubgraph "cluster_section_2.inner_section_2" {{\n\t\t\tcolor="#000000" fillcolor="#6495ed7f" label=inner_section_2 shape=rectangle style=filled\n\t\t\t"section_2.inner_section_2.task_2" [color="#000000" fillcolor="#f0ede4" label=task_2 shape=rectangle style="filled,rounded"]\n\t\t\t"section_2.inner_section_2.task_3" [color="#000000" fillcolor="#e8f7e4" label=task_3 shape=rectangle style="filled,rounded"]\n\t\t\t"section_2.inner_section_2.task_4" [color="#000000" fillcolor="#e8f7e4" label=task_4 shape=rectangle style="filled,rounded"]\n\t\t}}\n\t\t"section_2.task_1" [color="#000000" fillcolor="#e8f7e4" label=task_1 shape=rectangle style="filled,rounded"]\n\t}}\n\tstart [color="#000000" fillcolor="#e8f7e4" label=start shape=rectangle style="filled,rounded"]\n\t"section_1.downstream_join_id" -> "section_2.upstream_join_id"\n\t"section_1.task_1" -> "section_1.task_2"\n\t"section_1.task_1" -> "section_1.task_3"\n\t"section_1.task_2" -> "section_1.downstream_join_id"\n\t"section_1.task_3" -> "section_1.downstream_join_id"\n\t"section_1.upstream_join_id" -> "section_1.task_1"\n\t"section_2.downstream_join_id" -> end\n\t"section_2.inner_section_2.task_2" -> "section_2.inner_section_2.task_4"\n\t"section_2.inner_section_2.task_3" -> "section_2.inner_section_2.task_4"\n\t"section_2.inner_section_2.task_4" -> "section_2.downstream_join_id"\n\t"section_2.task_1" -> "section_2.downstream_join_id"\n\t"section_2.upstream_join_id" -> "section_2.inner_section_2.task_2"\n\t"section_2.upstream_join_id" -> "section_2.inner_section_2.task_3"\n\t"section_2.upstream_join_id" -> "section_2.task_1"\n\tstart -> "section_1.upstream_join_id"\n}}'` instead of string join
+ tests/utils/test_log_handlers.py:414:13: FLY002 [*] Consider `f"airflow_version=.+?,dag_id=dag_for_testing_file_task_handler,kubernetes_executor=True,run_id=manual__2016-01-01T0000000000-2b88d1d57,task_id=task_for_testing_file_log_handler,try_number=2,airflow-worker"` instead of string join
+ tests/utils/test_log_handlers.py:589:19: FLY002 [*] Consider `f"[2022-11-16T00:05:54.278-0800] {{taskinstance.py:1258}} INFO - Starting attempt 1 of 1"` instead of string join
+ tests/utils/test_log_handlers.py:594:19: FLY002 [*] Consider `f"[2022-11-16T00:05:54.295-0800] {{taskinstance.py:1278}} INFO - Executing <Task(TimeDeltaSensorAsync): wait> on 2022-11-16 08:05:52.324532+00:00\n[2022-11-16T00:05:54.300-0800] {{standard_task_runner.py:55}} INFO - Started process 52536 to run task\n[2022-11-16T00:05:54.300-0800] {{standard_task_runner.py:55}} INFO - Started process 52536 to run task\n[2022-11-16T00:05:54.300-0800] {{standard_task_runner.py:55}} INFO - Started process 52536 to run task\n[2022-11-16T00:05:54.306-0800] {{standard_task_runner.py:82}} INFO - Running: ['airflow', 'tasks', 'run', 'simple_async_timedelta', 'wait', 'manual__2022-11-16T08:05:52.324532+00:00', '--job-id', '33648', '--raw', '--subdir', '/Users/dstandish/code/airflow/airflow/example_dags/example_time_delta_sensor_async.py', '--cfg-path', '/var/folders/7_/1xx0hqcs3txd7kqt0ngfdjth0000gn/T/tmp725r305n']\n[2022-11-16T00:05:54.309-0800] {{standard_task_runner.py:83}} INFO - Job 33648: Subtask wait"` instead of string join
+ tests/utils/test_log_handlers.py:604:19: FLY002 [*] Consider `f"[2022-11-16T00:05:54.457-0800] {{task_command.py:376}} INFO - Running <TaskInstance: simple_async_timedelta.wait manual__2022-11-16T08:05:52.324532+00:00 [running]> on host daniels-mbp-2.lan\n[2022-11-16T00:05:54.592-0800] {{taskinstance.py:1485}} INFO - Exporting env vars: AIRFLOW_CTX_DAG_OWNER=airflow\nAIRFLOW_CTX_DAG_ID=simple_async_timedelta\nAIRFLOW_CTX_TASK_ID=wait\nAIRFLOW_CTX_EXECUTION_DATE=2022-11-16T08:05:52.324532+00:00\nAIRFLOW_CTX_TRY_NUMBER=1\nAIRFLOW_CTX_DAG_RUN_ID=manual__2022-11-16T08:05:52.324532+00:00\n[2022-11-16T00:05:54.604-0800] {{taskinstance.py:1360}} INFO - Pausing task as DEFERRED. dag_id=simple_async_timedelta, task_id=wait, execution_date=20221116T080552, start_date=20221116T080554"` instead of string join
+ tests/utils/test_log_handlers.py:616:16: FLY002 [*] Consider `f"[2022-11-16T00:05:54.278-0800] {{taskinstance.py:1258}} INFO - Starting attempt 1 of 1\n[2022-11-16T00:05:54.295-0800] {{taskinstance.py:1278}} INFO - Executing <Task(TimeDeltaSensorAsync): wait> on 2022-11-16 08:05:52.324532+00:00\n[2022-11-16T00:05:54.300-0800] {{standard_task_runner.py:55}} INFO - Started process 52536 to run task\n[2022-11-16T00:05:54.306-0800] {{standard_task_runner.py:82}} INFO - Running: ['airflow', 'tasks', 'run', 'simple_async_timedelta', 'wait', 'manual__2022-11-16T08:05:52.324532+00:00', '--job-id', '33648', '--raw', '--subdir', '/Users/dstandish/code/airflow/airflow/example_dags/example_time_delta_sensor_async.py', '--cfg-path', '/var/folders/7_/1xx0hqcs3txd7kqt0ngfdjth0000gn/T/tmp725r305n']\n[2022-11-16T00:05:54.309-0800] {{standard_task_runner.py:83}} INFO - Job 33648: Subtask wait\n[2022-11-16T00:05:54.457-0800] {{task_command.py:376}} INFO - Running <TaskInstance: simple_async_timedelta.wait manual__2022-11-16T08:05:52.324532+00:00 [running]> on host daniels-mbp-2.lan\n[2022-11-16T00:05:54.592-0800] {{taskinstance.py:1485}} INFO - Exporting env vars: AIRFLOW_CTX_DAG_OWNER=airflow\nAIRFLOW_CTX_DAG_ID=simple_async_timedelta\nAIRFLOW_CTX_TASK_ID=wait\nAIRFLOW_CTX_EXECUTION_DATE=2022-11-16T08:05:52.324532+00:00\nAIRFLOW_CTX_TRY_NUMBER=1\nAIRFLOW_CTX_DAG_RUN_ID=manual__2022-11-16T08:05:52.324532+00:00\n[2022-11-16T00:05:54.604-0800] {{taskinstance.py:1360}} INFO - Pausing task as DEFERRED. dag_id=simple_async_timedelta, task_id=wait, execution_date=20221116T080552, start_date=20221116T080554"` instead of string join
```

</p>
</details>
<details><summary>zulip (+9, -0)</summary>
<p>

```diff
+ zerver/lib/request.py:351:27: FLY002 [*] Consider `f"{req_func.__module__}.{req_func.__name__}"` instead of string join
+ zerver/lib/upload/local.py:239:37: FLY002 [*] Consider `f"{emoji_path}.original"` instead of string join
+ zerver/lib/upload/s3.py:449:13: FLY002 [*] Consider `f"{emoji_path}.original"` instead of string join
+ zerver/migrations/0149_realm_emoji_drop_unique_constraint.py:85:12: FLY002 [*] Consider `f"{new_name}{image_ext}"` instead of string join
+ zerver/tests/test_invite.py:479:26: FLY002 [*] Consider `f"{cross_realm_bot_email},{legit_new_email}"` instead of string join
+ zerver/tests/test_message_fetch.py:329:28: FLY002 [*] Consider `f"{self.example_user('hamlet').email},{self.example_user('othello').email}"` instead of string join
+ zerver/tests/test_message_fetch.py:342:22: FLY002 [*] Consider `f"{self.example_user('cordelia').email},{self.example_user('othello').email}"` instead of string join
+ zerver/tests/test_message_fetch.py:354:28: FLY002 [*] Consider `f"{self.example_user('hamlet').email},{self.example_user('othello').email}"` instead of string join
+ zerver/tests/test_message_fetch.py:369:22: FLY002 [*] Consider `f"{self.example_user('cordelia').email},{self.example_user('othello').email}"` instead of string join
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.8±0.04ms     2.8 MB/sec    1.01     14.9±0.07ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.07ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    363.4±2.35µs     8.1 MB/sec    1.00    363.9±0.84µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.05ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.02ms     5.4 MB/sec    1.01      7.6±0.02ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1549.3±2.28µs    10.7 MB/sec    1.01   1558.6±4.10µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.0±0.24µs    17.7 MB/sec    1.00    167.4±1.29µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.02ms     7.7 MB/sec    1.01      3.3±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.00      5.9±0.06ms     6.9 MB/sec    1.02      6.0±0.01ms     6.7 MB/sec
parser/numpy/ctypeslib.py                  1.00   1153.8±1.85µs    14.4 MB/sec    1.02   1180.1±2.51µs    14.1 MB/sec
parser/numpy/globals.py                    1.00    117.2±0.23µs    25.2 MB/sec    1.03    120.4±0.34µs    24.5 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.2 MB/sec    1.02      2.6±0.00ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.1±0.31ms     2.5 MB/sec    1.01     16.2±0.16ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.05ms     4.0 MB/sec    1.00      4.1±0.06ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    483.7±6.15µs     6.1 MB/sec    1.00    484.6±6.92µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.08ms     3.7 MB/sec    1.00      6.8±0.11ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.07ms     5.0 MB/sec    1.01      8.3±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1730.3±17.68µs     9.6 MB/sec    1.01  1753.8±15.77µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.0±2.22µs    15.2 MB/sec    1.03    199.4±4.93µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.09ms     6.9 MB/sec    1.02      3.7±0.07ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.6±0.04ms     6.1 MB/sec    1.02      6.8±0.05ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1267.0±19.13µs    13.1 MB/sec    1.02  1287.7±10.63µs    12.9 MB/sec
parser/numpy/globals.py                    1.00    130.4±2.41µs    22.6 MB/sec    1.01    131.9±1.96µs    22.4 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.1 MB/sec    1.02      2.9±0.03ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
