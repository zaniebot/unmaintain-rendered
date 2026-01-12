```yaml
number: 4196
title: Implement Flynt static string join transform as FLY002
type: pull_request
state: merged
author: akx
labels: []
assignees: []
merged: true
base: main
head: flynt-static-string-joins
created_at: 2023-05-02T20:10:07Z
updated_at: 2023-05-09T00:46:39Z
url: https://github.com/astral-sh/ruff/pull/4196
synced_at: 2026-01-12T15:55:14Z
```

# Implement Flynt static string join transform as FLY002

---

_@akx_

Refs https://github.com/charliermarsh/ruff/issues/2102 (being one half of it).

I removed the concatenation feature (FLY001) from this PR because I found tricky corner cases that were kind of tough to gracefully handle, so this PR concentrates on turning `"x".join([a, "foo", b])` to `f"{a}xfoox{b}"`.

* <del>`"{'Finally'}, " + f"{greeting}" + f"{subject}"` causes an autofix syntax error, but I'm not quite sure what's happening in particular – what's the easiest way of seeing the autofixed-but-broken code?</del> That's dealt with by #4201 and #4202 :)



---

_Comment by @charliermarsh on 2023-05-02 20:28_

Sadly the best way to debug is to print out `transformed` where you see the `{}: Autofix introduced a syntax error. Reverting all changes.` error logging.

---

_Comment by @charliermarsh on 2023-05-02 20:36_

We should change it to print the syntax-erroring code in debug mode, since I find myself doing this a lot.

---

_Comment by @github-actions[bot] on 2023-05-02 21:03_

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
+ tests/utils/test_log_handlers.py:368:13: FLY002 [*] Consider `f"airflow_version=.+?,dag_id=dag_for_testing_file_task_handler,kubernetes_executor=True,run_id=manual__2016-01-01T0000000000-2b88d1d57,task_id=task_for_testing_file_log_handler,try_number=2,airflow-worker"` instead of string join
+ tests/utils/test_log_handlers.py:543:19: FLY002 [*] Consider `f"[2022-11-16T00:05:54.278-0800] {{taskinstance.py:1258}} INFO - Starting attempt 1 of 1"` instead of string join
+ tests/utils/test_log_handlers.py:548:19: FLY002 [*] Consider `f"[2022-11-16T00:05:54.295-0800] {{taskinstance.py:1278}} INFO - Executing <Task(TimeDeltaSensorAsync): wait> on 2022-11-16 08:05:52.324532+00:00\n[2022-11-16T00:05:54.300-0800] {{standard_task_runner.py:55}} INFO - Started process 52536 to run task\n[2022-11-16T00:05:54.300-0800] {{standard_task_runner.py:55}} INFO - Started process 52536 to run task\n[2022-11-16T00:05:54.300-0800] {{standard_task_runner.py:55}} INFO - Started process 52536 to run task\n[2022-11-16T00:05:54.306-0800] {{standard_task_runner.py:82}} INFO - Running: ['airflow', 'tasks', 'run', 'simple_async_timedelta', 'wait', 'manual__2022-11-16T08:05:52.324532+00:00', '--job-id', '33648', '--raw', '--subdir', '/Users/dstandish/code/airflow/airflow/example_dags/example_time_delta_sensor_async.py', '--cfg-path', '/var/folders/7_/1xx0hqcs3txd7kqt0ngfdjth0000gn/T/tmp725r305n']\n[2022-11-16T00:05:54.309-0800] {{standard_task_runner.py:83}} INFO - Job 33648: Subtask wait"` instead of string join
+ tests/utils/test_log_handlers.py:558:19: FLY002 [*] Consider `f"[2022-11-16T00:05:54.457-0800] {{task_command.py:376}} INFO - Running <TaskInstance: simple_async_timedelta.wait manual__2022-11-16T08:05:52.324532+00:00 [running]> on host daniels-mbp-2.lan\n[2022-11-16T00:05:54.592-0800] {{taskinstance.py:1485}} INFO - Exporting env vars: AIRFLOW_CTX_DAG_OWNER=airflow\nAIRFLOW_CTX_DAG_ID=simple_async_timedelta\nAIRFLOW_CTX_TASK_ID=wait\nAIRFLOW_CTX_EXECUTION_DATE=2022-11-16T08:05:52.324532+00:00\nAIRFLOW_CTX_TRY_NUMBER=1\nAIRFLOW_CTX_DAG_RUN_ID=manual__2022-11-16T08:05:52.324532+00:00\n[2022-11-16T00:05:54.604-0800] {{taskinstance.py:1360}} INFO - Pausing task as DEFERRED. dag_id=simple_async_timedelta, task_id=wait, execution_date=20221116T080552, start_date=20221116T080554"` instead of string join
+ tests/utils/test_log_handlers.py:570:16: FLY002 [*] Consider `f"[2022-11-16T00:05:54.278-0800] {{taskinstance.py:1258}} INFO - Starting attempt 1 of 1\n[2022-11-16T00:05:54.295-0800] {{taskinstance.py:1278}} INFO - Executing <Task(TimeDeltaSensorAsync): wait> on 2022-11-16 08:05:52.324532+00:00\n[2022-11-16T00:05:54.300-0800] {{standard_task_runner.py:55}} INFO - Started process 52536 to run task\n[2022-11-16T00:05:54.306-0800] {{standard_task_runner.py:82}} INFO - Running: ['airflow', 'tasks', 'run', 'simple_async_timedelta', 'wait', 'manual__2022-11-16T08:05:52.324532+00:00', '--job-id', '33648', '--raw', '--subdir', '/Users/dstandish/code/airflow/airflow/example_dags/example_time_delta_sensor_async.py', '--cfg-path', '/var/folders/7_/1xx0hqcs3txd7kqt0ngfdjth0000gn/T/tmp725r305n']\n[2022-11-16T00:05:54.309-0800] {{standard_task_runner.py:83}} INFO - Job 33648: Subtask wait\n[2022-11-16T00:05:54.457-0800] {{task_command.py:376}} INFO - Running <TaskInstance: simple_async_timedelta.wait manual__2022-11-16T08:05:52.324532+00:00 [running]> on host daniels-mbp-2.lan\n[2022-11-16T00:05:54.592-0800] {{taskinstance.py:1485}} INFO - Exporting env vars: AIRFLOW_CTX_DAG_OWNER=airflow\nAIRFLOW_CTX_DAG_ID=simple_async_timedelta\nAIRFLOW_CTX_TASK_ID=wait\nAIRFLOW_CTX_EXECUTION_DATE=2022-11-16T08:05:52.324532+00:00\nAIRFLOW_CTX_TRY_NUMBER=1\nAIRFLOW_CTX_DAG_RUN_ID=manual__2022-11-16T08:05:52.324532+00:00\n[2022-11-16T00:05:54.604-0800] {{taskinstance.py:1360}} INFO - Pausing task as DEFERRED. dag_id=simple_async_timedelta, task_id=wait, execution_date=20221116T080552, start_date=20221116T080554"` instead of string join
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
linter/all-rules/large/dataset.py          1.00     14.0±0.04ms     2.9 MB/sec    1.00     14.0±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    423.5±0.83µs     7.0 MB/sec    1.00    421.1±1.07µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.01      5.9±0.09ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.01ms     5.9 MB/sec    1.02      7.1±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1490.6±2.78µs    11.2 MB/sec    1.01   1511.5±2.69µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.6±0.36µs    17.7 MB/sec    1.02    169.1±0.64µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.02      3.1±0.02ms     8.1 MB/sec
parser/large/dataset.py                    1.00      5.4±0.02ms     7.5 MB/sec    1.02      5.6±0.01ms     7.3 MB/sec
parser/numpy/ctypeslib.py                  1.00   1060.3±3.34µs    15.7 MB/sec    1.02   1079.0±0.86µs    15.4 MB/sec
parser/numpy/globals.py                    1.00    108.0±0.18µs    27.3 MB/sec    1.02    110.0±1.95µs    26.8 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.0 MB/sec    1.02      2.4±0.01ms    10.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.11ms     2.4 MB/sec    1.01     16.8±0.14ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.02ms     3.9 MB/sec    1.00      4.3±0.02ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.02   439.4±29.07µs     6.7 MB/sec    1.00    430.4±5.33µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.07ms     3.6 MB/sec    1.01      7.2±0.08ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.05ms     4.8 MB/sec    1.02      8.6±0.05ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1802.8±13.54µs     9.2 MB/sec    1.01   1827.9±9.98µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.9±2.66µs    15.4 MB/sec    1.01    194.2±4.77µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.03ms     6.6 MB/sec    1.01      3.9±0.03ms     6.5 MB/sec
parser/large/dataset.py                    1.00      7.0±0.04ms     5.8 MB/sec    1.00      7.0±0.02ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1322.1±9.67µs    12.6 MB/sec    1.00   1323.9±9.59µs    12.6 MB/sec
parser/numpy/globals.py                    1.00    135.3±1.05µs    21.8 MB/sec    1.00    135.1±1.01µs    21.8 MB/sec
parser/pydantic/types.py                   1.00      3.0±0.07ms     8.6 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @MichaReiser on 2023-05-03 07:50_

> Autofix introduced a syntax error. Revertin

I made some changes to our test infra so that should help you narrow down the issue #4202

---

_Renamed from "Implement Flynt's static string join transforms" to "Implement Flynt static string join transform as FLY002" by @akx on 2023-05-04 07:50_

---

_Marked ready for review by @akx on 2023-05-04 07:54_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:2454 on 2023-05-04 14:28_

Nit: use `else_if` to help the compiler if it isn't clever enough to determine that these two branches are mutually exclusive. 
```suggestion
                            } else if attr == "format" {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flynt/rules/static_join_to_fstring.rs`:37 on 2023-05-04 14:30_

```suggestion
fn build_fstring(joiner: &str, joinees: &[Expr]) -> Option<Expr> {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flynt/helpers.rs`:36 on 2023-05-04 14:32_

Can we change the visibility of this functions (and other functions introduced by this PR) to `private` (remove the `pub`), `pub(super)`, or `pub(crate)`. 


---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flynt/helpers.rs`:36 on 2023-05-04 14:33_

I recommend taking the `Expr` as a reference and cloning it on demand, or (a little more complicated) to return an `Option<Cow<Expr>>` to avoid cloning the `Expr` altogether.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flynt/rules/static_join_to_fstring.rs`:52 on 2023-05-04 14:36_

Nit
```suggestion
	let mut first = true;
	
    for expr in joinees {
        if matches!(expr.node, ExprKind::JoinedStr { .. }) {
            // Oops, already an f-string. We don't know how to handle those
            // gracefully right now.
            return None;
        }
        let elem = helpers::to_fstring_elem(expr.clone())?;
        if !first {
            fstring_elems.push(helpers::to_constant_string(joiner));
        }
        fstring_elems.push(elem);
        first = false;
    }
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flynt/rules/static_join_to_fstring.rs`:83 on 2023-05-04 14:37_

Remove?

---

_@MichaReiser approved on 2023-05-04 14:38_

Impressive work! I only have a few nit comments. 

---

_@akx reviewed on 2023-05-05 06:44_

---

_Review comment by @akx on `crates/ruff/src/rules/flynt/helpers.rs`:36 on 2023-05-05 06:44_

Sure thing. I'm still not 100% on the visibility rules for Rust, getting there 😁 

---

_@MichaReiser reviewed on 2023-05-05 06:57_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flynt/helpers.rs`:36 on 2023-05-05 06:57_

Me neither. I always go with the lowest visibility and increase it until Rust stops complaining at me :laughing: 

---

_Review requested from @MichaReiser by @akx on 2023-05-05 07:27_

---

_@MichaReiser approved on 2023-05-05 12:10_

---

_Merged by @charliermarsh on 2023-05-09 00:46_

---

_Closed by @charliermarsh on 2023-05-09 00:46_

---
