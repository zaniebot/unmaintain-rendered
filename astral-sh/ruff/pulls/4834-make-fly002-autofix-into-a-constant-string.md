```yaml
number: 4834
title: "Make FLY002 autofix into a constant string instead of an f-string if all `join()` arguments are strings"
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: 4769_fix_fly002
created_at: 2023-06-03T15:27:44Z
updated_at: 2023-06-03T21:22:41Z
url: https://github.com/astral-sh/ruff/pull/4834
synced_at: 2026-01-12T15:55:16Z
```

# Make FLY002 autofix into a constant string instead of an f-string if all `join()` arguments are strings

---

_@evanrittenhouse_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Fixes #4769. If all arguments to `join()` are strings, we want to autofix the code into a string instead of an f-string. For example:
```python
# OLD
', '.join('apple', 'banana', 'cherry') # currently suggests f'apple, banana, cherry'

# NEW
', '.join('apple', 'banana', 'cherry') # now suggests 'apple, banana, cherry' (no f-string)
``` 

## Test Plan
`cargo t`


---

_Renamed from "4769 fix fly002" to "Make FLY002 autofix into a constant string instead of an f-string if all `join()` arguments are strings" by @evanrittenhouse on 2023-06-03 15:28_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flynt/rules/static_join_to_fstring.rs`:63 on 2023-06-03 15:32_

`range` and `kind` are irrelevant since the autofix uses the range of the original `expr` (see the `Diagnostic`'s construction in `static_join_to_fstring`)

---

_@evanrittenhouse reviewed on 2023-06-03 15:33_

---

_Comment by @github-actions[bot] on 2023-06-03 15:45_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+10, -10, 0 error(s))

<details><summary>airflow (+10, -10)</summary>
<p>

```diff
+ airflow/settings.py:63:10: FLY002 [*] Consider `"  ____________       _____________\n ____    |__( )_________  __/__  /________      __\n____  /| |_  /__  ___/_  /_ __  /_  __ \\_ | /| / /\n___  ___ |  / _  /   _  __/ _  / / /_/ /_ |/ |/ /\n _/_/  |_/_/  /_/    /_/    /_/  \\____/____/|__/"` instead of string join
- airflow/settings.py:63:10: FLY002 [*] Consider `f"  ____________       _____________\n ____    |__( )_________  __/__  /________      __\n____  /| |_  /__  ___/_  /_ __  /_  __ \\_ | /| / /\n___  ___ |  / _  /   _  __/ _  / / /_/ /_ |/ |/ /\n _/_/  |_/_/  /_/    /_/    /_/  \\____/____/|__/"` instead of string join
+ tests/cli/commands/test_kubernetes_command.py:60:22: FLY002 [*] Consider `"dag_id,task_id,try_number,airflow_version"` instead of string join
- tests/cli/commands/test_kubernetes_command.py:60:22: FLY002 [*] Consider `f"dag_id,task_id,try_number,airflow_version"` instead of string join
+ tests/providers/google/common/hooks/test_base_google.py:527:17: FLY002 [*] Consider `"https://www.googleapis.com/auth/bigquery,https://www.googleapis.com/auth/devstorage.read_only"` instead of string join
- tests/providers/google/common/hooks/test_base_google.py:527:17: FLY002 [*] Consider `f"https://www.googleapis.com/auth/bigquery,https://www.googleapis.com/auth/devstorage.read_only"` instead of string join
+ tests/providers/google/common/hooks/test_base_google.py:594:17: FLY002 [*] Consider `"https://www.googleapis.com/auth/bigquery,https://www.googleapis.com/auth/devstorage.read_only"` instead of string join
- tests/providers/google/common/hooks/test_base_google.py:594:17: FLY002 [*] Consider `f"https://www.googleapis.com/auth/bigquery,https://www.googleapis.com/auth/devstorage.read_only"` instead of string join
+ tests/utils/test_dot_renderer.py:181:38: FLY002 [*] Consider `'digraph example_task_group {\n\tgraph [label=example_task_group labelloc=t rankdir=LR]\n\tend [color="#000000" fillcolor="#e8f7e4" label=end shape=rectangle style="filled,rounded"]\n\tsubgraph cluster_section_1 {\n\t\tcolor="#000000" fillcolor="#6495ed7f" label=section_1 shape=rectangle style=filled\n\t\t"section_1.upstream_join_id" [color="#000000" fillcolor=CornflowerBlue height=0.2 label="" shape=circle style="filled,rounded" width=0.2]\n\t\t"section_1.downstream_join_id" [color="#000000" fillcolor=CornflowerBlue height=0.2 label="" shape=circle style="filled,rounded" width=0.2]\n\t\t"section_1.task_1" [color="#000000" fillcolor="#e8f7e4" label=task_1 shape=rectangle style="filled,rounded"]\n\t\t"section_1.task_2" [color="#000000" fillcolor="#f0ede4" label=task_2 shape=rectangle style="filled,rounded"]\n\t\t"section_1.task_3" [color="#000000" fillcolor="#e8f7e4" label=task_3 shape=rectangle style="filled,rounded"]\n\t}\n\tsubgraph cluster_section_2 {\n\t\tcolor="#000000" fillcolor="#6495ed7f" label=section_2 shape=rectangle style=filled\n\t\t"section_2.upstream_join_id" [color="#000000" fillcolor=CornflowerBlue height=0.2 label="" shape=circle style="filled,rounded" width=0.2]\n\t\t"section_2.downstream_join_id" [color="#000000" fillcolor=CornflowerBlue height=0.2 label="" shape=circle style="filled,rounded" width=0.2]\n\t\tsubgraph "cluster_section_2.inner_section_2" {\n\t\t\tcolor="#000000" fillcolor="#6495ed7f" label=inner_section_2 shape=rectangle style=filled\n\t\t\t"section_2.inner_section_2.task_2" [color="#000000" fillcolor="#f0ede4" label=task_2 shape=rectangle style="filled,rounded"]\n\t\t\t"section_2.inner_section_2.task_3" [color="#000000" fillcolor="#e8f7e4" label=task_3 shape=rectangle style="filled,rounded"]\n\t\t\t"section_2.inner_section_2.task_4" [color="#000000" fillcolor="#e8f7e4" label=task_4 shape=rectangle style="filled,rounded"]\n\t\t}\n\t\t"section_2.task_1" [color="#000000" fillcolor="#e8f7e4" label=task_1 shape=rectangle style="filled,rounded"]\n\t}\n\tstart [color="#000000" fillcolor="#e8f7e4" label=start shape=rectangle style="filled,rounded"]\n\t"section_1.downstream_join_id" -> "section_2.upstream_join_id"\n\t"section_1.task_1" -> "section_1.task_2"\n\t"section_1.task_1" -> "section_1.task_3"\n\t"section_1.task_2" -> "section_1.downstream_join_id"\n\t"section_1.task_3" -> "section_1.downstream_join_id"\n\t"section_1.upstream_join_id" -> "section_1.task_1"\n\t"section_2.downstream_join_id" -> end\n\t"section_2.inner_section_2.task_2" -> "section_2.inner_section_2.task_4"\n\t"section_2.inner_section_2.task_3" -> "section_2.inner_section_2.task_4"\n\t"section_2.inner_section_2.task_4" -> "section_2.downstream_join_id"\n\t"section_2.task_1" -> "section_2.downstream_join_id"\n\t"section_2.upstream_join_id" -> "section_2.inner_section_2.task_2"\n\t"section_2.upstream_join_id" -> "section_2.inner_section_2.task_3"\n\t"section_2.upstream_join_id" -> "section_2.task_1"\n\tstart -> "section_1.upstream_join_id"\n}'` instead of string join
- tests/utils/test_dot_renderer.py:181:38: FLY002 [*] Consider `f'digraph example_task_group {{\n\tgraph [label=example_task_group labelloc=t rankdir=LR]\n\tend [color="#000000" fillcolor="#e8f7e4" label=end shape=rectangle style="filled,rounded"]\n\tsubgraph cluster_section_1 {{\n\t\tcolor="#000000" fillcolor="#6495ed7f" label=section_1 shape=rectangle style=filled\n\t\t"section_1.upstream_join_id" [color="#000000" fillcolor=CornflowerBlue height=0.2 label="" shape=circle style="filled,rounded" width=0.2]\n\t\t"section_1.downstream_join_id" [color="#000000" fillcolor=CornflowerBlue height=0.2 label="" shape=circle style="filled,rounded" width=0.2]\n\t\t"section_1.task_1" [color="#000000" fillcolor="#e8f7e4" label=task_1 shape=rectangle style="filled,rounded"]\n\t\t"section_1.task_2" [color="#000000" fillcolor="#f0ede4" label=task_2 shape=rectangle style="filled,rounded"]\n\t\t"section_1.task_3" [color="#000000" fillcolor="#e8f7e4" label=task_3 shape=rectangle style="filled,rounded"]\n\t}}\n\tsubgraph cluster_section_2 {{\n\t\tcolor="#000000" fillcolor="#6495ed7f" label=section_2 shape=rectangle style=filled\n\t\t"section_2.upstream_join_id" [color="#000000" fillcolor=CornflowerBlue height=0.2 label="" shape=circle style="filled,rounded" width=0.2]\n\t\t"section_2.downstream_join_id" [color="#000000" fillcolor=CornflowerBlue height=0.2 label="" shape=circle style="filled,rounded" width=0.2]\n\t\tsubgraph "cluster_section_2.inner_section_2" {{\n\t\t\tcolor="#000000" fillcolor="#6495ed7f" label=inner_section_2 shape=rectangle style=filled\n\t\t\t"section_2.inner_section_2.task_2" [color="#000000" fillcolor="#f0ede4" label=task_2 shape=rectangle style="filled,rounded"]\n\t\t\t"section_2.inner_section_2.task_3" [color="#000000" fillcolor="#e8f7e4" label=task_3 shape=rectangle style="filled,rounded"]\n\t\t\t"section_2.inner_section_2.task_4" [color="#000000" fillcolor="#e8f7e4" label=task_4 shape=rectangle style="filled,rounded"]\n\t\t}}\n\t\t"section_2.task_1" [color="#000000" fillcolor="#e8f7e4" label=task_1 shape=rectangle style="filled,rounded"]\n\t}}\n\tstart [color="#000000" fillcolor="#e8f7e4" label=start shape=rectangle style="filled,rounded"]\n\t"section_1.downstream_join_id" -> "section_2.upstream_join_id"\n\t"section_1.task_1" -> "section_1.task_2"\n\t"section_1.task_1" -> "section_1.task_3"\n\t"section_1.task_2" -> "section_1.downstream_join_id"\n\t"section_1.task_3" -> "section_1.downstream_join_id"\n\t"section_1.upstream_join_id" -> "section_1.task_1"\n\t"section_2.downstream_join_id" -> end\n\t"section_2.inner_section_2.task_2" -> "section_2.inner_section_2.task_4"\n\t"section_2.inner_section_2.task_3" -> "section_2.inner_section_2.task_4"\n\t"section_2.inner_section_2.task_4" -> "section_2.downstream_join_id"\n\t"section_2.task_1" -> "section_2.downstream_join_id"\n\t"section_2.upstream_join_id" -> "section_2.inner_section_2.task_2"\n\t"section_2.upstream_join_id" -> "section_2.inner_section_2.task_3"\n\t"section_2.upstream_join_id" -> "section_2.task_1"\n\tstart -> "section_1.upstream_join_id"\n}}'` instead of string join
+ tests/utils/test_log_handlers.py:414:13: FLY002 [*] Consider `"airflow_version=.+?,dag_id=dag_for_testing_file_task_handler,kubernetes_executor=True,run_id=manual__2016-01-01T0000000000-2b88d1d57,task_id=task_for_testing_file_log_handler,try_number=2,airflow-worker"` instead of string join
- tests/utils/test_log_handlers.py:414:13: FLY002 [*] Consider `f"airflow_version=.+?,dag_id=dag_for_testing_file_task_handler,kubernetes_executor=True,run_id=manual__2016-01-01T0000000000-2b88d1d57,task_id=task_for_testing_file_log_handler,try_number=2,airflow-worker"` instead of string join
+ tests/utils/test_log_handlers.py:589:19: FLY002 [*] Consider `"[2022-11-16T00:05:54.278-0800] {taskinstance.py:1258} INFO - Starting attempt 1 of 1"` instead of string join
- tests/utils/test_log_handlers.py:589:19: FLY002 [*] Consider `f"[2022-11-16T00:05:54.278-0800] {{taskinstance.py:1258}} INFO - Starting attempt 1 of 1"` instead of string join
+ tests/utils/test_log_handlers.py:594:19: FLY002 [*] Consider `"[2022-11-16T00:05:54.295-0800] {taskinstance.py:1278} INFO - Executing <Task(TimeDeltaSensorAsync): wait> on 2022-11-16 08:05:52.324532+00:00\n[2022-11-16T00:05:54.300-0800] {standard_task_runner.py:55} INFO - Started process 52536 to run task\n[2022-11-16T00:05:54.300-0800] {standard_task_runner.py:55} INFO - Started process 52536 to run task\n[2022-11-16T00:05:54.300-0800] {standard_task_runner.py:55} INFO - Started process 52536 to run task\n[2022-11-16T00:05:54.306-0800] {standard_task_runner.py:82} INFO - Running: ['airflow', 'tasks', 'run', 'simple_async_timedelta', 'wait', 'manual__2022-11-16T08:05:52.324532+00:00', '--job-id', '33648', '--raw', '--subdir', '/Users/dstandish/code/airflow/airflow/example_dags/example_time_delta_sensor_async.py', '--cfg-path', '/var/folders/7_/1xx0hqcs3txd7kqt0ngfdjth0000gn/T/tmp725r305n']\n[2022-11-16T00:05:54.309-0800] {standard_task_runner.py:83} INFO - Job 33648: Subtask wait"` instead of string join
- tests/utils/test_log_handlers.py:594:19: FLY002 [*] Consider `f"[2022-11-16T00:05:54.295-0800] {{taskinstance.py:1278}} INFO - Executing <Task(TimeDeltaSensorAsync): wait> on 2022-11-16 08:05:52.324532+00:00\n[2022-11-16T00:05:54.300-0800] {{standard_task_runner.py:55}} INFO - Started process 52536 to run task\n[2022-11-16T00:05:54.300-0800] {{standard_task_runner.py:55}} INFO - Started process 52536 to run task\n[2022-11-16T00:05:54.300-0800] {{standard_task_runner.py:55}} INFO - Started process 52536 to run task\n[2022-11-16T00:05:54.306-0800] {{standard_task_runner.py:82}} INFO - Running: ['airflow', 'tasks', 'run', 'simple_async_timedelta', 'wait', 'manual__2022-11-16T08:05:52.324532+00:00', '--job-id', '33648', '--raw', '--subdir', '/Users/dstandish/code/airflow/airflow/example_dags/example_time_delta_sensor_async.py', '--cfg-path', '/var/folders/7_/1xx0hqcs3txd7kqt0ngfdjth0000gn/T/tmp725r305n']\n[2022-11-16T00:05:54.309-0800] {{standard_task_runner.py:83}} INFO - Job 33648: Subtask wait"` instead of string join
+ tests/utils/test_log_handlers.py:604:19: FLY002 [*] Consider `"[2022-11-16T00:05:54.457-0800] {task_command.py:376} INFO - Running <TaskInstance: simple_async_timedelta.wait manual__2022-11-16T08:05:52.324532+00:00 [running]> on host daniels-mbp-2.lan\n[2022-11-16T00:05:54.592-0800] {taskinstance.py:1485} INFO - Exporting env vars: AIRFLOW_CTX_DAG_OWNER=airflow\nAIRFLOW_CTX_DAG_ID=simple_async_timedelta\nAIRFLOW_CTX_TASK_ID=wait\nAIRFLOW_CTX_EXECUTION_DATE=2022-11-16T08:05:52.324532+00:00\nAIRFLOW_CTX_TRY_NUMBER=1\nAIRFLOW_CTX_DAG_RUN_ID=manual__2022-11-16T08:05:52.324532+00:00\n[2022-11-16T00:05:54.604-0800] {taskinstance.py:1360} INFO - Pausing task as DEFERRED. dag_id=simple_async_timedelta, task_id=wait, execution_date=20221116T080552, start_date=20221116T080554"` instead of string join
- tests/utils/test_log_handlers.py:604:19: FLY002 [*] Consider `f"[2022-11-16T00:05:54.457-0800] {{task_command.py:376}} INFO - Running <TaskInstance: simple_async_timedelta.wait manual__2022-11-16T08:05:52.324532+00:00 [running]> on host daniels-mbp-2.lan\n[2022-11-16T00:05:54.592-0800] {{taskinstance.py:1485}} INFO - Exporting env vars: AIRFLOW_CTX_DAG_OWNER=airflow\nAIRFLOW_CTX_DAG_ID=simple_async_timedelta\nAIRFLOW_CTX_TASK_ID=wait\nAIRFLOW_CTX_EXECUTION_DATE=2022-11-16T08:05:52.324532+00:00\nAIRFLOW_CTX_TRY_NUMBER=1\nAIRFLOW_CTX_DAG_RUN_ID=manual__2022-11-16T08:05:52.324532+00:00\n[2022-11-16T00:05:54.604-0800] {{taskinstance.py:1360}} INFO - Pausing task as DEFERRED. dag_id=simple_async_timedelta, task_id=wait, execution_date=20221116T080552, start_date=20221116T080554"` instead of string join
+ tests/utils/test_log_handlers.py:616:16: FLY002 [*] Consider `"[2022-11-16T00:05:54.278-0800] {taskinstance.py:1258} INFO - Starting attempt 1 of 1\n[2022-11-16T00:05:54.295-0800] {taskinstance.py:1278} INFO - Executing <Task(TimeDeltaSensorAsync): wait> on 2022-11-16 08:05:52.324532+00:00\n[2022-11-16T00:05:54.300-0800] {standard_task_runner.py:55} INFO - Started process 52536 to run task\n[2022-11-16T00:05:54.306-0800] {standard_task_runner.py:82} INFO - Running: ['airflow', 'tasks', 'run', 'simple_async_timedelta', 'wait', 'manual__2022-11-16T08:05:52.324532+00:00', '--job-id', '33648', '--raw', '--subdir', '/Users/dstandish/code/airflow/airflow/example_dags/example_time_delta_sensor_async.py', '--cfg-path', '/var/folders/7_/1xx0hqcs3txd7kqt0ngfdjth0000gn/T/tmp725r305n']\n[2022-11-16T00:05:54.309-0800] {standard_task_runner.py:83} INFO - Job 33648: Subtask wait\n[2022-11-16T00:05:54.457-0800] {task_command.py:376} INFO - Running <TaskInstance: simple_async_timedelta.wait manual__2022-11-16T08:05:52.324532+00:00 [running]> on host daniels-mbp-2.lan\n[2022-11-16T00:05:54.592-0800] {taskinstance.py:1485} INFO - Exporting env vars: AIRFLOW_CTX_DAG_OWNER=airflow\nAIRFLOW_CTX_DAG_ID=simple_async_timedelta\nAIRFLOW_CTX_TASK_ID=wait\nAIRFLOW_CTX_EXECUTION_DATE=2022-11-16T08:05:52.324532+00:00\nAIRFLOW_CTX_TRY_NUMBER=1\nAIRFLOW_CTX_DAG_RUN_ID=manual__2022-11-16T08:05:52.324532+00:00\n[2022-11-16T00:05:54.604-0800] {taskinstance.py:1360} INFO - Pausing task as DEFERRED. dag_id=simple_async_timedelta, task_id=wait, execution_date=20221116T080552, start_date=20221116T080554"` instead of string join
- tests/utils/test_log_handlers.py:616:16: FLY002 [*] Consider `f"[2022-11-16T00:05:54.278-0800] {{taskinstance.py:1258}} INFO - Starting attempt 1 of 1\n[2022-11-16T00:05:54.295-0800] {{taskinstance.py:1278}} INFO - Executing <Task(TimeDeltaSensorAsync): wait> on 2022-11-16 08:05:52.324532+00:00\n[2022-11-16T00:05:54.300-0800] {{standard_task_runner.py:55}} INFO - Started process 52536 to run task\n[2022-11-16T00:05:54.306-0800] {{standard_task_runner.py:82}} INFO - Running: ['airflow', 'tasks', 'run', 'simple_async_timedelta', 'wait', 'manual__2022-11-16T08:05:52.324532+00:00', '--job-id', '33648', '--raw', '--subdir', '/Users/dstandish/code/airflow/airflow/example_dags/example_time_delta_sensor_async.py', '--cfg-path', '/var/folders/7_/1xx0hqcs3txd7kqt0ngfdjth0000gn/T/tmp725r305n']\n[2022-11-16T00:05:54.309-0800] {{standard_task_runner.py:83}} INFO - Job 33648: Subtask wait\n[2022-11-16T00:05:54.457-0800] {{task_command.py:376}} INFO - Running <TaskInstance: simple_async_timedelta.wait manual__2022-11-16T08:05:52.324532+00:00 [running]> on host daniels-mbp-2.lan\n[2022-11-16T00:05:54.592-0800] {{taskinstance.py:1485}} INFO - Exporting env vars: AIRFLOW_CTX_DAG_OWNER=airflow\nAIRFLOW_CTX_DAG_ID=simple_async_timedelta\nAIRFLOW_CTX_TASK_ID=wait\nAIRFLOW_CTX_EXECUTION_DATE=2022-11-16T08:05:52.324532+00:00\nAIRFLOW_CTX_TRY_NUMBER=1\nAIRFLOW_CTX_DAG_RUN_ID=manual__2022-11-16T08:05:52.324532+00:00\n[2022-11-16T00:05:54.604-0800] {{taskinstance.py:1360}} INFO - Pausing task as DEFERRED. dag_id=simple_async_timedelta, task_id=wait, execution_date=20221116T080552, start_date=20221116T080554"` instead of string join
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| FLY002 | 20 | 10 | 10 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     19.2±0.62ms     2.1 MB/sec    1.00     18.7±0.49ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.5±0.17ms     3.7 MB/sec    1.00      4.4±0.19ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   553.7±19.47µs     5.3 MB/sec    1.00   554.0±28.76µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.9±0.27ms     3.2 MB/sec    1.00      7.7±0.31ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.01      8.6±0.14ms     4.7 MB/sec    1.00      8.6±0.17ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1871.4±41.78µs     8.9 MB/sec    1.00  1864.6±57.62µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.03    228.0±9.62µs    12.9 MB/sec    1.00    221.4±8.23µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.08ms     6.5 MB/sec    1.00      3.9±0.13ms     6.6 MB/sec
parser/large/dataset.py                    1.02      6.8±0.16ms     6.0 MB/sec    1.00      6.6±0.12ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.02  1337.6±53.30µs    12.4 MB/sec    1.00  1309.4±43.14µs    12.7 MB/sec
parser/numpy/globals.py                    1.01    133.7±5.32µs    22.1 MB/sec    1.00    132.7±8.02µs    22.2 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.07ms     8.7 MB/sec    1.00      2.9±0.09ms     8.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.2±0.46ms     2.4 MB/sec    1.00     17.0±0.33ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.16ms     3.8 MB/sec    1.00      4.3±0.14ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   498.6±11.39µs     5.9 MB/sec    1.02   508.5±13.41µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.18ms     3.5 MB/sec    1.00      7.2±0.15ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.10ms     4.9 MB/sec    1.01      8.4±0.14ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1776.3±38.40µs     9.4 MB/sec    1.02  1803.2±96.30µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    204.4±7.08µs    14.4 MB/sec    1.02    207.6±6.39µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.07ms     6.8 MB/sec    1.03      3.9±0.13ms     6.6 MB/sec
parser/large/dataset.py                    1.07      7.0±0.11ms     5.8 MB/sec    1.00      6.5±0.10ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.07  1311.8±42.61µs    12.7 MB/sec    1.00  1225.1±29.52µs    13.6 MB/sec
parser/numpy/globals.py                    1.05    131.4±3.47µs    22.5 MB/sec    1.00    125.7±2.56µs    23.5 MB/sec
parser/pydantic/types.py                   1.06      3.0±0.07ms     8.6 MB/sec    1.00      2.8±0.06ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @evanrittenhouse on 2023-06-03 16:33_

Not sure if we want to change this to work with other types as well? For example: 
```python
', '.join([1, 2, 3]) # resolves to f"{1}, {2}, {3}"
```

If so, I'll have to make another change.

---

_Comment by @charliermarsh on 2023-06-03 19:08_

Thank you :) Do you mind enabling edits so that I can update the branch?

---

_Comment by @charliermarsh on 2023-06-03 19:09_

> Not sure if we want to change this to work with other types as well?

I think it's fine to skip this for now.

---

_Comment by @evanrittenhouse on 2023-06-03 19:43_

> Thank you :) Do you mind enabling edits so that I can update the branch?

Done!

---

_@charliermarsh reviewed on 2023-06-03 20:25_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flynt/rules/static_join_to_fstring.rs`:66 on 2023-06-03 20:25_

This is a little more verbose, and it returns two passes, but it avoids allocating two vectors in every case.

---

_Merged by @charliermarsh on 2023-06-03 20:35_

---

_Closed by @charliermarsh on 2023-06-03 20:35_

---

_Branch deleted on 2023-06-03 20:35_

---

_@evanrittenhouse reviewed on 2023-06-03 20:36_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flynt/rules/static_join_to_fstring.rs`:66 on 2023-06-03 20:36_

@charliermarsh Why do we want to avoid allocations instead of 2 passes in this case?

---

_@charliermarsh reviewed on 2023-06-03 20:40_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flynt/rules/static_join_to_fstring.rs`:66 on 2023-06-03 20:40_

My intuition is that the allocations would be more expensive than iterating over the vector a second time.

---

_@evanrittenhouse reviewed on 2023-06-03 20:41_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flynt/rules/static_join_to_fstring.rs`:66 on 2023-06-03 20:41_

Gotcha, thanks - just trying to get better :)

---

_@charliermarsh reviewed on 2023-06-03 21:22_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flynt/rules/static_join_to_fstring.rs`:66 on 2023-06-03 21:22_

Of course, I appreciate it — me too :)

I’m not an expert but I generally try to consider allocations expensive. In this case, we’re already iterating over this vec, so iterating over it again should be cheap, compared to the allocations at least.

---
