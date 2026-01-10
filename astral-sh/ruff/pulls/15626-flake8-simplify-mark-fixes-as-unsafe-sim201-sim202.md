```yaml
number: 15626
title: "[`flake8-simplify`] Mark fixes as unsafe (`SIM201`, `SIM202`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - fixes
assignees: []
merged: true
base: main
head: SIM201
created_at: 2025-01-21T01:33:37Z
updated_at: 2025-01-21T17:34:12Z
url: https://github.com/astral-sh/ruff/pull/15626
synced_at: 2026-01-10T20:05:43Z
```

# [`flake8-simplify`] Mark fixes as unsafe (`SIM201`, `SIM202`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-21 01:33_

## Summary

Resolves #15619.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-21 01:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -32 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -26 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/airflow/models/baseoperator.py#L167'>airflow/models/baseoperator.py:167:38:</a> SIM201 Use `sentinel != _sentinel` instead of `not sentinel == _sentinel`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/airflow/models/baseoperator.py#L167'>airflow/models/baseoperator.py:167:38:</a> SIM201 [*] Use `sentinel != _sentinel` instead of `not sentinel == _sentinel`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/airflow/models/dag.py#L822'>airflow/models/dag.py:822:44:</a> SIM201 Use `ti.state != State.NONE` instead of `not ti.state == State.NONE`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/airflow/models/dag.py#L822'>airflow/models/dag.py:822:44:</a> SIM201 [*] Use `ti.state != State.NONE` instead of `not ti.state == State.NONE`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/airflow/www/views.py#L5625'>airflow/www/views.py:5625:12:</a> SIM201 Use `os.environ.get("AIRFLOW_ENV", None) != "development"` instead of `not os.environ.get("AIRFLOW_ENV", None) == "development"`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/airflow/www/views.py#L5625'>airflow/www/views.py:5625:12:</a> SIM201 [*] Use `os.environ.get("AIRFLOW_ENV", None) != "development"` instead of `not os.environ.get("AIRFLOW_ENV", None) == "development"`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/dev/breeze/src/airflow_breeze/params/shell_params.py#L358'>dev/breeze/src/airflow_breeze/params/shell_params.py:358:35:</a> SIM201 Use `self.project_name != "pre-commit"` instead of `not self.project_name == "pre-commit"`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/dev/breeze/src/airflow_breeze/params/shell_params.py#L358'>dev/breeze/src/airflow_breeze/params/shell_params.py:358:35:</a> SIM201 [*] Use `self.project_name != "pre-commit"` instead of `not self.project_name == "pre-commit"`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/dev/breeze/src/airflow_breeze/params/shell_params.py#L708'>dev/breeze/src/airflow_breeze/params/shell_params.py:708:17:</a> SIM201 Use `self.backend != POSTGRES_BACKEND` instead of `not self.backend == POSTGRES_BACKEND`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/dev/breeze/src/airflow_breeze/params/shell_params.py#L708'>dev/breeze/src/airflow_breeze/params/shell_params.py:708:17:</a> SIM201 [*] Use `self.backend != POSTGRES_BACKEND` instead of `not self.backend == POSTGRES_BACKEND`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/dev/breeze/src/airflow_breeze/utils/parallel.py#L400'>dev/breeze/src/airflow_breeze/utils/parallel.py:400:11:</a> SIM201 Use `len(completed_list) != total_number_of_results` instead of `not len(completed_list) == total_number_of_results`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/dev/breeze/src/airflow_breeze/utils/parallel.py#L400'>dev/breeze/src/airflow_breeze/utils/parallel.py:400:11:</a> SIM201 [*] Use `len(completed_list) != total_number_of_results` instead of `not len(completed_list) == total_number_of_results`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/providers/src/airflow/providers/cncf/kubernetes/triggers/pod.py#L204'>providers/src/airflow/providers/cncf/kubernetes/triggers/pod.py:204:16:</a> SIM201 Use `pod.status.phase != "Pending"` instead of `not pod.status.phase == "Pending"`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/providers/src/airflow/providers/cncf/kubernetes/triggers/pod.py#L204'>providers/src/airflow/providers/cncf/kubernetes/triggers/pod.py:204:16:</a> SIM201 [*] Use `pod.status.phase != "Pending"` instead of `not pod.status.phase == "Pending"`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/providers/tests/amazon/aws/executors/ecs/test_ecs_executor.py#L1045'>providers/tests/amazon/aws/executors/ecs/test_ecs_executor.py:1045:12:</a> SIM201 Use `set_task_state != State.REMOVED` instead of `not set_task_state == State.REMOVED`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/providers/tests/amazon/aws/executors/ecs/test_ecs_executor.py#L1045'>providers/tests/amazon/aws/executors/ecs/test_ecs_executor.py:1045:12:</a> SIM201 [*] Use `set_task_state != State.REMOVED` instead of `not set_task_state == State.REMOVED`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/providers/tests/amazon/aws/system/utils/test_helpers.py#L129'>providers/tests/amazon/aws/system/utils/test_helpers.py:129:16:</a> SIM201 Use `result != env_id` instead of `not result == env_id`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/providers/tests/amazon/aws/system/utils/test_helpers.py#L129'>providers/tests/amazon/aws/system/utils/test_helpers.py:129:16:</a> SIM201 [*] Use `result != env_id` instead of `not result == env_id`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/providers/tests/system/docker/example_docker_copy_data.py#L64'>providers/tests/system/docker/example_docker_copy_data.py:64:45:</a> SIM201 Use `task_output != ""` instead of `not task_output == ""`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/providers/tests/system/docker/example_docker_copy_data.py#L64'>providers/tests/system/docker/example_docker_copy_data.py:64:45:</a> SIM201 [*] Use `task_output != ""` instead of `not task_output == ""`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/scripts/ci/pre_commit/generate_airflow_diagrams.py#L45'>scripts/ci/pre_commit/generate_airflow_diagrams.py:45:38:</a> SIM201 Use `hash_file.read_text().strip() != str(checksum).strip()` instead of `not hash_file.read_text().strip() == str(checksum).strip()`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/scripts/ci/pre_commit/generate_airflow_diagrams.py#L45'>scripts/ci/pre_commit/generate_airflow_diagrams.py:45:38:</a> SIM201 [*] Use `hash_file.read_text().strip() != str(checksum).strip()` instead of `not hash_file.read_text().strip() == str(checksum).strip()`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/scripts/in_container/run_provider_yaml_files_check.py#L127'>scripts/in_container/run_provider_yaml_files_check.py:127:12:</a> SIM201 Use `provider["state"] != "suspended"` instead of `not provider["state"] == "suspended"`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/scripts/in_container/run_provider_yaml_files_check.py#L127'>scripts/in_container/run_provider_yaml_files_check.py:127:12:</a> SIM201 [*] Use `provider["state"] != "suspended"` instead of `not provider["state"] == "suspended"`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/tests/serialization/test_serialized_objects.py#L83'>tests/serialization/test_serialized_objects.py:83:16:</a> SIM201 Use `elem.func.value.id != "cls"` instead of `not elem.func.value.id == "cls"`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/tests/serialization/test_serialized_objects.py#L83'>tests/serialization/test_serialized_objects.py:83:16:</a> SIM201 [*] Use `elem.func.value.id != "cls"` instead of `not elem.func.value.id == "cls"`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/binary-husky/gpt_academic/blob/2716606f0cff88f3f6875aa346fb82584e23c9bc/crazy_functions/vector_fns/vector_database.py#L314'>crazy_functions/vector_fns/vector_database.py:314:12:</a> SIM201 Use `self.current_id != id` instead of `not self.current_id == id`
- <a href='https://github.com/binary-husky/gpt_academic/blob/2716606f0cff88f3f6875aa346fb82584e23c9bc/crazy_functions/vector_fns/vector_database.py#L314'>crazy_functions/vector_fns/vector_database.py:314:12:</a> SIM201 [*] Use `self.current_id != id` instead of `not self.current_id == id`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/views/ws.py#L168'>src/bokeh/server/views/ws.py:168:12:</a> SIM201 Use `len(subprotocols) != 2` instead of `not len(subprotocols) == 2`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/views/ws.py#L168'>src/bokeh/server/views/ws.py:168:12:</a> SIM201 [*] Use `len(subprotocols) != 2` instead of `not len(subprotocols) == 2`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/768a8a4381bda35a916f9de9f8e4b0cf64db8e9b/src/latch_cli/services/sync.py#L142'>src/latch_cli/services/sync.py:142:12:</a> SIM201 Use `dest[-1] != "/"` instead of `not dest[-1] == "/"`
- <a href='https://github.com/latchbio/latch/blob/768a8a4381bda35a916f9de9f8e4b0cf64db8e9b/src/latch_cli/services/sync.py#L142'>src/latch_cli/services/sync.py:142:12:</a> SIM201 [*] Use `dest[-1] != "/"` instead of `not dest[-1] == "/"`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM201 | 32 | 0 | 0 | 0 | 32 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -32 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -26 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/airflow/models/baseoperator.py#L167'>airflow/models/baseoperator.py:167:38:</a> SIM201 Use `sentinel != _sentinel` instead of `not sentinel == _sentinel`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/airflow/models/baseoperator.py#L167'>airflow/models/baseoperator.py:167:38:</a> SIM201 [*] Use `sentinel != _sentinel` instead of `not sentinel == _sentinel`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/airflow/models/dag.py#L822'>airflow/models/dag.py:822:44:</a> SIM201 Use `ti.state != State.NONE` instead of `not ti.state == State.NONE`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/airflow/models/dag.py#L822'>airflow/models/dag.py:822:44:</a> SIM201 [*] Use `ti.state != State.NONE` instead of `not ti.state == State.NONE`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/airflow/www/views.py#L5625'>airflow/www/views.py:5625:12:</a> SIM201 Use `os.environ.get("AIRFLOW_ENV", None) != "development"` instead of `not os.environ.get("AIRFLOW_ENV", None) == "development"`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/airflow/www/views.py#L5625'>airflow/www/views.py:5625:12:</a> SIM201 [*] Use `os.environ.get("AIRFLOW_ENV", None) != "development"` instead of `not os.environ.get("AIRFLOW_ENV", None) == "development"`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/dev/breeze/src/airflow_breeze/params/shell_params.py#L358'>dev/breeze/src/airflow_breeze/params/shell_params.py:358:35:</a> SIM201 Use `self.project_name != "pre-commit"` instead of `not self.project_name == "pre-commit"`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/dev/breeze/src/airflow_breeze/params/shell_params.py#L358'>dev/breeze/src/airflow_breeze/params/shell_params.py:358:35:</a> SIM201 [*] Use `self.project_name != "pre-commit"` instead of `not self.project_name == "pre-commit"`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/dev/breeze/src/airflow_breeze/params/shell_params.py#L708'>dev/breeze/src/airflow_breeze/params/shell_params.py:708:17:</a> SIM201 Use `self.backend != POSTGRES_BACKEND` instead of `not self.backend == POSTGRES_BACKEND`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/dev/breeze/src/airflow_breeze/params/shell_params.py#L708'>dev/breeze/src/airflow_breeze/params/shell_params.py:708:17:</a> SIM201 [*] Use `self.backend != POSTGRES_BACKEND` instead of `not self.backend == POSTGRES_BACKEND`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/dev/breeze/src/airflow_breeze/utils/parallel.py#L400'>dev/breeze/src/airflow_breeze/utils/parallel.py:400:11:</a> SIM201 Use `len(completed_list) != total_number_of_results` instead of `not len(completed_list) == total_number_of_results`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/dev/breeze/src/airflow_breeze/utils/parallel.py#L400'>dev/breeze/src/airflow_breeze/utils/parallel.py:400:11:</a> SIM201 [*] Use `len(completed_list) != total_number_of_results` instead of `not len(completed_list) == total_number_of_results`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/providers/src/airflow/providers/cncf/kubernetes/triggers/pod.py#L204'>providers/src/airflow/providers/cncf/kubernetes/triggers/pod.py:204:16:</a> SIM201 Use `pod.status.phase != "Pending"` instead of `not pod.status.phase == "Pending"`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/providers/src/airflow/providers/cncf/kubernetes/triggers/pod.py#L204'>providers/src/airflow/providers/cncf/kubernetes/triggers/pod.py:204:16:</a> SIM201 [*] Use `pod.status.phase != "Pending"` instead of `not pod.status.phase == "Pending"`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/providers/tests/amazon/aws/executors/ecs/test_ecs_executor.py#L1045'>providers/tests/amazon/aws/executors/ecs/test_ecs_executor.py:1045:12:</a> SIM201 Use `set_task_state != State.REMOVED` instead of `not set_task_state == State.REMOVED`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/providers/tests/amazon/aws/executors/ecs/test_ecs_executor.py#L1045'>providers/tests/amazon/aws/executors/ecs/test_ecs_executor.py:1045:12:</a> SIM201 [*] Use `set_task_state != State.REMOVED` instead of `not set_task_state == State.REMOVED`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/providers/tests/amazon/aws/system/utils/test_helpers.py#L129'>providers/tests/amazon/aws/system/utils/test_helpers.py:129:16:</a> SIM201 Use `result != env_id` instead of `not result == env_id`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/providers/tests/amazon/aws/system/utils/test_helpers.py#L129'>providers/tests/amazon/aws/system/utils/test_helpers.py:129:16:</a> SIM201 [*] Use `result != env_id` instead of `not result == env_id`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/providers/tests/system/docker/example_docker_copy_data.py#L64'>providers/tests/system/docker/example_docker_copy_data.py:64:45:</a> SIM201 Use `task_output != ""` instead of `not task_output == ""`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/providers/tests/system/docker/example_docker_copy_data.py#L64'>providers/tests/system/docker/example_docker_copy_data.py:64:45:</a> SIM201 [*] Use `task_output != ""` instead of `not task_output == ""`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/scripts/ci/pre_commit/generate_airflow_diagrams.py#L45'>scripts/ci/pre_commit/generate_airflow_diagrams.py:45:38:</a> SIM201 Use `hash_file.read_text().strip() != str(checksum).strip()` instead of `not hash_file.read_text().strip() == str(checksum).strip()`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/scripts/ci/pre_commit/generate_airflow_diagrams.py#L45'>scripts/ci/pre_commit/generate_airflow_diagrams.py:45:38:</a> SIM201 [*] Use `hash_file.read_text().strip() != str(checksum).strip()` instead of `not hash_file.read_text().strip() == str(checksum).strip()`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/scripts/in_container/run_provider_yaml_files_check.py#L127'>scripts/in_container/run_provider_yaml_files_check.py:127:12:</a> SIM201 Use `provider["state"] != "suspended"` instead of `not provider["state"] == "suspended"`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/scripts/in_container/run_provider_yaml_files_check.py#L127'>scripts/in_container/run_provider_yaml_files_check.py:127:12:</a> SIM201 [*] Use `provider["state"] != "suspended"` instead of `not provider["state"] == "suspended"`
+ <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/tests/serialization/test_serialized_objects.py#L83'>tests/serialization/test_serialized_objects.py:83:16:</a> SIM201 Use `elem.func.value.id != "cls"` instead of `not elem.func.value.id == "cls"`
- <a href='https://github.com/apache/airflow/blob/287b1d800d8748d23387936a88c943c2df241c57/tests/serialization/test_serialized_objects.py#L83'>tests/serialization/test_serialized_objects.py:83:16:</a> SIM201 [*] Use `elem.func.value.id != "cls"` instead of `not elem.func.value.id == "cls"`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/binary-husky/gpt_academic/blob/2716606f0cff88f3f6875aa346fb82584e23c9bc/crazy_functions/vector_fns/vector_database.py#L314'>crazy_functions/vector_fns/vector_database.py:314:12:</a> SIM201 Use `self.current_id != id` instead of `not self.current_id == id`
- <a href='https://github.com/binary-husky/gpt_academic/blob/2716606f0cff88f3f6875aa346fb82584e23c9bc/crazy_functions/vector_fns/vector_database.py#L314'>crazy_functions/vector_fns/vector_database.py:314:12:</a> SIM201 [*] Use `self.current_id != id` instead of `not self.current_id == id`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/views/ws.py#L168'>src/bokeh/server/views/ws.py:168:12:</a> SIM201 Use `len(subprotocols) != 2` instead of `not len(subprotocols) == 2`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/views/ws.py#L168'>src/bokeh/server/views/ws.py:168:12:</a> SIM201 [*] Use `len(subprotocols) != 2` instead of `not len(subprotocols) == 2`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/768a8a4381bda35a916f9de9f8e4b0cf64db8e9b/src/latch_cli/services/sync.py#L142'>src/latch_cli/services/sync.py:142:12:</a> SIM201 Use `dest[-1] != "/"` instead of `not dest[-1] == "/"`
- <a href='https://github.com/latchbio/latch/blob/768a8a4381bda35a916f9de9f8e4b0cf64db8e9b/src/latch_cli/services/sync.py#L142'>src/latch_cli/services/sync.py:142:12:</a> SIM201 [*] Use `dest[-1] != "/"` instead of `not dest[-1] == "/"`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM201 | 32 | 0 | 0 | 0 | 32 |

</p>
</details>




---

_Comment by @MichaReiser on 2025-01-21 08:28_

Could you add a `Fix safety` section explaining why the fix is unsafe. It will help users to decide if they want to opt-in to this fix or not.

---

_Label `fixes` added by @MichaReiser on 2025-01-21 08:28_

---

_@MichaReiser reviewed on 2025-01-21 15:16_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_unary_op.rs`:29 on 2025-01-21 15:16_

I think the message is slightly confusing because `a == b` generally returns a boolean, at least for all types that override `__eq__` in a conferment way. 

I'd suggest rephrasing the message to saying that the fix is unsafe for types that override `__eq__` in a non standard way (return something other than `boolean`). I think we did something similar for `round` or some other rule that you worked on

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_unary_op.rs`:29 on 2025-01-21 15:47_

(Did you mean to say "conformant"?) That was [`RUF046`](https://docs.astral.sh/ruff/rules/unnecessary-cast-to-int/). Done.

---

_@InSyncWithFoo reviewed on 2025-01-21 15:47_

---

_@MichaReiser approved on 2025-01-21 17:17_

This looks good, thanks

---

_Merged by @MichaReiser on 2025-01-21 17:17_

---

_Closed by @MichaReiser on 2025-01-21 17:17_

---

_Branch deleted on 2025-01-21 17:34_

---
