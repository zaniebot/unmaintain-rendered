```yaml
number: 3924
title: Implement flak8-bandit shell injection rules
type: pull_request
state: merged
author: robyoung
labels: []
assignees: []
merged: true
base: main
head: implement-bandit-shell-injection-rules
created_at: 2023-04-09T20:59:52Z
updated_at: 2023-12-11T11:41:26Z
url: https://github.com/astral-sh/ruff/pull/3924
synced_at: 2026-01-10T23:40:55Z
```

# Implement flak8-bandit shell injection rules

---

_Pull request opened by @robyoung on 2023-04-09 20:59_

This includes rules S602 - S607. Partially addresses #1646.

This is my first PR on this project so I apologise if I have missed some things.

---

_Comment by @github-actions[bot] on 2023-04-09 21:26_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+357, -0, 0 error(s))

<details><summary>airflow (+246, -0)</summary>
<p>

```diff
+ airflow/cli/commands/dag_command.py:248:31: S603 `subprocess` call: check for execution of untrusted input
+ airflow/cli/commands/dag_command.py:248:31: S607 Starting a process with a partial executable path
+ airflow/cli/commands/info_command.py:197:35: S603 `subprocess` call: check for execution of untrusted input
+ airflow/cli/commands/internal_api_command.py:184:38: S603 `subprocess` call: check for execution of untrusted input
+ airflow/cli/commands/internal_api_command.py:199:35: S603 `subprocess` call: check for execution of untrusted input
+ airflow/cli/commands/standalone_command.py:290:13: S603 `subprocess` call: check for execution of untrusted input
+ airflow/cli/commands/webserver_command.py:482:38: S603 `subprocess` call: check for execution of untrusted input
+ airflow/cli/commands/webserver_command.py:497:35: S603 `subprocess` call: check for execution of untrusted input
+ airflow/configuration.py:106:9: S603 `subprocess` call: check for execution of untrusted input
+ airflow/example_dags/example_kubernetes_executor.py:134:45: S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
+ airflow/example_dags/example_kubernetes_executor.py:134:45: S607 Starting a process with a partial executable path
+ airflow/example_dags/example_kubernetes_executor.py:96:37: S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
+ airflow/example_dags/example_kubernetes_executor.py:96:37: S607 Starting a process with a partial executable path
+ airflow/executors/celery_executor.py:154:33: S603 `subprocess` call: check for execution of untrusted input
+ airflow/executors/dask_executor.py:93:42: S603 `subprocess` call: check for execution of untrusted input
+ airflow/executors/local_executor.py:98:35: S603 `subprocess` call: check for execution of untrusted input
+ airflow/executors/sequential_executor.py:77:39: S603 `subprocess` call: check for execution of untrusted input
+ airflow/hooks/subprocess.py:78:17: S603 `subprocess` call: check for execution of untrusted input
+ airflow/operators/python.py:664:46: S603 `subprocess` call: check for execution of untrusted input
+ airflow/operators/python.py:679:35: S603 `subprocess` call: check for execution of untrusted input
+ airflow/operators/python.py:696:17: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/amazon/aws/operators/s3.py:585:21: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/apache/beam/hooks/beam.py:134:9: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/apache/beam/hooks/beam.py:266:21: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/apache/hive/hooks/hive.py:278:21: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/apache/pig/hooks/pig.py:88:21: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/apache/pinot/hooks/pinot.py:228:13: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/apache/spark/hooks/spark_sql.py:173:13: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/apache/spark/hooks/spark_submit.py:401:13: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/apache/spark/hooks/spark_submit.py:563:17: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/apache/spark/hooks/spark_submit.py:609:35: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/apache/spark/hooks/spark_submit.py:631:21: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/apache/sqoop/hooks/sqoop.py:107:31: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/google/cloud/example_dags/example_cloud_sql_query.py:197:53: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/google/cloud/example_dags/example_cloud_sql_query.py:197:53: S607 Starting a process with a partial executable path
+ airflow/providers/google/cloud/hooks/cloud_sql.py:575:44: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/google/cloud/hooks/cloud_sql.py:633:42: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/google/cloud/hooks/dataflow.py:1017:35: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/google/common/hooks/base_google.py:545:21: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/google/common/hooks/base_google.py:545:21: S607 Starting a process with a partial executable path
+ airflow/providers/google/common/hooks/base_google.py:558:34: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/google/common/hooks/base_google.py:558:34: S607 Starting a process with a partial executable path
+ airflow/providers/google/common/hooks/base_google.py:561:25: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/google/common/hooks/base_google.py:561:25: S607 Starting a process with a partial executable path
+ airflow/providers/google/common/hooks/base_google.py:565:25: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/google/common/hooks/base_google.py:565:25: S607 Starting a process with a partial executable path
+ airflow/providers/google/common/hooks/base_google.py:576:30: S603 `subprocess` call: check for execution of untrusted input
+ airflow/providers/google/common/hooks/base_google.py:576:30: S607 Starting a process with a partial executable path
+ airflow/security/kerberos.py:143:27: S603 `subprocess` call: check for execution of untrusted input
+ airflow/security/kerberos.py:92:9: S603 `subprocess` call: check for execution of untrusted input
+ airflow/sensors/bash.py:81:21: S603 `subprocess` call: check for execution of untrusted input
+ airflow/sensors/bash.py:81:21: S607 Starting a process with a partial executable path
+ airflow/task/task_runner/base_task_runner.py:136:21: S603 `subprocess` call: check for execution of untrusted input
+ airflow/task/task_runner/base_task_runner.py:145:21: S603 `subprocess` call: check for execution of untrusted input
+ airflow/task/task_runner/base_task_runner.py:184:33: S603 `subprocess` call: check for execution of untrusted input
+ airflow/task/task_runner/base_task_runner.py:184:33: S607 Starting a process with a partial executable path
+ airflow/task/task_runner/base_task_runner.py:77:35: S603 `subprocess` call: check for execution of untrusted input
+ airflow/task/task_runner/base_task_runner.py:77:35: S607 Starting a process with a partial executable path
+ airflow/utils/process_utils.py:107:47: S603 `subprocess` call: check for execution of untrusted input
+ airflow/utils/process_utils.py:107:47: S607 Starting a process with a partial executable path
+ airflow/utils/process_utils.py:183:9: S603 `subprocess` call: check for execution of untrusted input
+ airflow/utils/process_utils.py:214:13: S603 `subprocess` call: check for execution of untrusted input
+ airflow/utils/process_utils.py:93:21: S603 `subprocess` call: check for execution of untrusted input
+ dev/assign_cherry_picked_prs_with_milestone.py:225:9: S603 `subprocess` call: check for execution of untrusted input
+ dev/breeze/src/airflow_breeze/commands/ci_image_commands.py:485:17: S603 `subprocess` call: check for execution of untrusted input
+ dev/breeze/src/airflow_breeze/commands/main_command.py:123:50: S603 `subprocess` call: check for execution of untrusted input
+ dev/breeze/src/airflow_breeze/commands/main_command.py:123:50: S607 Starting a process with a partial executable path
+ dev/breeze/src/airflow_breeze/commands/main_command.py:170:13: S603 `subprocess` call: check for execution of untrusted input
+ dev/breeze/src/airflow_breeze/commands/main_command.py:170:13: S607 Starting a process with a partial executable path
+ dev/breeze/src/airflow_breeze/commands/release_candidate_command.py:106:81: S604 Function call with `shell=True` parameter identified, security issue
+ dev/breeze/src/airflow_breeze/commands/release_candidate_command.py:148:96: S604 Function call with `shell=True` parameter identified, security issue
+ dev/breeze/src/airflow_breeze/commands/release_candidate_command.py:160:72: S604 Function call with `shell=True` parameter identified, security issue
+ dev/breeze/src/airflow_breeze/commands/setup_commands.py:148:35: S603 `subprocess` call: check for execution of untrusted input
+ dev/breeze/src/airflow_breeze/commands/setup_commands.py:148:35: S607 Starting a process with a partial executable path
+ dev/breeze/src/airflow_breeze/commands/setup_commands.py:150:41: S603 `subprocess` call: check for execution of untrusted input
+ dev/breeze/src/airflow_breeze/commands/setup_commands.py:150:41: S607 Starting a process with a partial executable path
+ dev/breeze/src/airflow_breeze/utils/reinstall.py:38:27: S603 `subprocess` call: check for execution of untrusted input
+ dev/breeze/src/airflow_breeze/utils/reinstall.py:38:27: S607 Starting a process with a partial executable path
+ dev/breeze/src/airflow_breeze/utils/reinstall.py:43:9: S606 Starting a process without a shell
+ dev/breeze/src/airflow_breeze/utils/run_utils.py:136:31: S603 `subprocess` call: check for execution of untrusted input
+ dev/breeze/src/airflow_breeze/utils/run_utils.py:152:35: S603 `subprocess` call: check for execution of untrusted input
+ dev/perf/scheduler_dag_execution_timing.py:291:9: S606 Starting a process without a shell
+ dev/prepare_release_issue.py:166:9: S603 `subprocess` call: check for execution of untrusted input
+ dev/provider_packages/prepare_provider_packages.py:1024:13: S603 `subprocess` call: check for execution of untrusted input
+ dev/provider_packages/prepare_provider_packages.py:1036:9: S603 `subprocess` call: check for execution of untrusted input
+ dev/provider_packages/prepare_provider_packages.py:1365:33: S603 `subprocess` call: check for execution of untrusted input
+ dev/provider_packages/prepare_provider_packages.py:1365:33: S607 Starting a process with a partial executable path
+ dev/provider_packages/prepare_provider_packages.py:1583:9: S603 `subprocess` call: check for execution of untrusted input
+ dev/provider_packages/prepare_provider_packages.py:691:21: S603 `subprocess` call: check for execution of untrusted input
+ dev/provider_packages/prepare_provider_packages.py:695:27: S603 `subprocess` call: check for execution of untrusted input
+ dev/provider_packages/prepare_provider_packages.py:721:13: S603 `subprocess` call: check for execution of untrusted input
+ dev/provider_packages/prepare_provider_packages.py:742:21: S603 `subprocess` call: check for execution of untrusted input
+ dev/provider_packages/prepare_provider_packages.py:753:33: S603 `subprocess` call: check for execution of untrusted input
+ dev/provider_packages/prepare_provider_packages.py:753:33: S607 Starting a process with a partial executable path
+ dev/provider_packages/prepare_provider_packages.py:767:31: S603 `subprocess` call: check for execution of untrusted input
+ dev/provider_packages/prepare_provider_packages.py:952:9: S603 `subprocess` call: check for execution of untrusted input
+ dev/provider_packages/prepare_provider_packages.py:960:13: S603 `subprocess` call: check for execution of untrusted input
+ dev/provider_packages/prepare_provider_packages.py:974:25: S603 `subprocess` call: check for execution of untrusted input
+ dev/provider_packages/remove_old_releases.py:81:32: S603 `subprocess` call: check for execution of untrusted input
+ dev/retag_docker_images.py:65:17: S603 `subprocess` call: check for execution of untrusted input
+ dev/retag_docker_images.py:65:17: S607 Starting a process with a partial executable path
+ docker_tests/command_utils.py:27:44: S603 `subprocess` call: check for execution of untrusted input
+ docker_tests/command_utils.py:29:28: S603 `subprocess` call: check for execution of untrusted input
+ docker_tests/test_docker_compose_quick_start.py:137:37: S603 `subprocess` call: check for execution of untrusted input
+ docker_tests/test_docker_compose_quick_start.py:137:37: S607 Starting a process with a partial executable path
+ docker_tests/test_docker_compose_quick_start.py:54:33: S603 `subprocess` call: check for execution of untrusted input
+ docker_tests/test_docker_compose_quick_start.py:54:33: S607 Starting a process with a partial executable path
+ docker_tests/test_docker_compose_quick_start.py:63:37: S603 `subprocess` call: check for execution of untrusted input
+ docker_tests/test_docker_compose_quick_start.py:63:37: S607 Starting a process with a partial executable path
+ docker_tests/test_docker_compose_quick_start.py:70:21: S603 `subprocess` call: check for execution of untrusted input
+ docker_tests/test_docker_compose_quick_start.py:70:21: S607 Starting a process with a partial executable path
+ docs/exts/docs_build/docs_builder.py:167:17: S603 `subprocess` call: check for execution of untrusted input
+ docs/exts/docs_build/docs_builder.py:246:17: S603 `subprocess` call: check for execution of untrusted input
+ kubernetes_tests/test_base.py:104:32: S603 `subprocess` call: check for execution of untrusted input
+ kubernetes_tests/test_base.py:104:32: S607 Starting a process with a partial executable path
+ kubernetes_tests/test_base.py:112:32: S603 `subprocess` call: check for execution of untrusted input
+ kubernetes_tests/test_base.py:112:32: S607 Starting a process with a partial executable path
+ kubernetes_tests/test_base.py:116:24: S603 `subprocess` call: check for execution of untrusted input
+ kubernetes_tests/test_base.py:116:24: S607 Starting a process with a partial executable path
+ kubernetes_tests/test_base.py:163:32: S603 `subprocess` call: check for execution of untrusted input
+ kubernetes_tests/test_base.py:163:32: S607 Starting a process with a partial executable path
+ kubernetes_tests/test_base.py:178:28: S603 `subprocess` call: check for execution of untrusted input
+ kubernetes_tests/test_base.py:178:28: S607 Starting a process with a partial executable path
+ kubernetes_tests/test_base.py:201:24: S603 `subprocess` call: check for execution of untrusted input
+ kubernetes_tests/test_base.py:201:24: S607 Starting a process with a partial executable path
+ kubernetes_tests/test_base.py:80:17: S603 `subprocess` call: check for execution of untrusted input
+ kubernetes_tests/test_base.py:80:17: S607 Starting a process with a partial executable path
+ kubernetes_tests/test_base.py:88:17: S603 `subprocess` call: check for execution of untrusted input
+ kubernetes_tests/test_base.py:88:17: S607 Starting a process with a partial executable path
+ kubernetes_tests/test_base.py:96:17: S603 `subprocess` call: check for execution of untrusted input
+ kubernetes_tests/test_base.py:96:17: S607 Starting a process with a partial executable path
+ scripts/ci/pre_commit/pre_commit_boring_cyborg.py:36:41: S603 `subprocess` call: check for execution of untrusted input
+ scripts/ci/pre_commit/pre_commit_boring_cyborg.py:36:41: S607 Starting a process with a partial executable path
+ scripts/ci/pre_commit/pre_commit_boring_cyborg.py:37:41: S603 `subprocess` call: check for execution of untrusted input
+ scripts/ci/pre_commit/pre_commit_boring_cyborg.py:37:41: S607 Starting a process with a partial executable path
+ scripts/ci/pre_commit/pre_commit_breeze_cmd_line.py:68:9: S603 `subprocess` call: check for execution of untrusted input
+ scripts/ci/pre_commit/pre_commit_check_license.py:53:5: S603 `subprocess` call: check for execution of untrusted input
+ scripts/ci/pre_commit/pre_commit_compile_www_assets.py:47:27: S603 `subprocess` call: check for execution of untrusted input
+ scripts/ci/pre_commit/pre_commit_compile_www_assets.py:47:27: S607 Starting a process with a partial executable path
+ scripts/ci/pre_commit/pre_commit_compile_www_assets.py:48:27: S603 `subprocess` call: check for execution of untrusted input
+ scripts/ci/pre_commit/pre_commit_compile_www_assets.py:48:27: S607 Starting a process with a partial executable path
+ scripts/ci/pre_commit/pre_commit_compile_www_assets_dev.py:50:13: S603 `subprocess` call: check for execution of untrusted input
+ scripts/ci/pre_commit/pre_commit_compile_www_assets_dev.py:50:13: S607 Starting a process with a partial executable path
+ scripts/ci/pre_commit/pre_commit_compile_www_assets_dev.py:57:13: S603 `subprocess` call: check for execution of untrusted input
+ scripts/ci/pre_commit/pre_commit_compile_www_assets_dev.py:57:13: S607 Starting a process with a partial executable path
+ scripts/ci/pre_commit/pre_commit_lint_dockerfile.py:47:5: S603 `subprocess` call: check for execution of untrusted input
+ scripts/ci/pre_commit/pre_commit_update_common_sql_api_stubs.py:336:9: S603 `subprocess` call: check for execution of untrusted input
+ scripts/ci/pre_commit/pre_commit_update_common_sql_api_stubs.py:336:9: S607 Starting a process with a partial executable path
+ scripts/ci/pre_commit/pre_commit_www_lint.py:31:27: S603 `subprocess` call: check for execution of untrusted input
+ scripts/ci/pre_commit/pre_commit_www_lint.py:31:27: S607 Starting a process with a partial executable path
+ scripts/ci/pre_commit/pre_commit_www_lint.py:32:27: S603 `subprocess` call: check for execution of untrusted input
+ scripts/ci/pre_commit/pre_commit_www_lint.py:32:27: S607 Starting a process with a partial executable path
+ scripts/ci/pre_commit/pre_commit_www_lint.py:33:27: S603 `subprocess` call: check for execution of untrusted input
+ scripts/ci/pre_commit/pre_commit_www_lint.py:33:27: S607 Starting a process with a partial executable path
+ scripts/ci/pre_commit/pre_commit_www_lint.py:34:27: S603 `subprocess` call: check for execution of untrusted input
+ scripts/ci/pre_commit/pre_commit_www_lint.py:34:27: S607 Starting a process with a partial executable path
+ scripts/in_container/remove_arm_packages.py:47:20: S603 `subprocess` call: check for execution of untrusted input
+ scripts/in_container/verify_providers.py:816:20: S603 `subprocess` call: check for execution of untrusted input
+ scripts/in_container/verify_providers.py:816:20: S607 Starting a process with a partial executable path
+ scripts/in_container/verify_providers.py:818:20: S603 `subprocess` call: check for execution of untrusted input
+ scripts/in_container/verify_providers.py:818:20: S607 Starting a process with a partial executable path
+ scripts/in_container/verify_providers.py:820:20: S603 `subprocess` call: check for execution of untrusted input
+ scripts/in_container/verify_providers.py:820:20: S607 Starting a process with a partial executable path
+ scripts/in_container/verify_providers.py:822:20: S603 `subprocess` call: check for execution of untrusted input
+ scripts/in_container/verify_providers.py:822:20: S607 Starting a process with a partial executable path
+ scripts/in_container/verify_providers.py:824:20: S603 `subprocess` call: check for execution of untrusted input
+ scripts/in_container/verify_providers.py:824:20: S607 Starting a process with a partial executable path
+ scripts/in_container/verify_providers.py:826:20: S603 `subprocess` call: check for execution of untrusted input
+ scripts/in_container/verify_providers.py:826:20: S607 Starting a process with a partial executable path
+ scripts/in_container/verify_providers.py:828:20: S603 `subprocess` call: check for execution of untrusted input
+ scripts/in_container/verify_providers.py:828:20: S607 Starting a process with a partial executable path
+ scripts/in_container/verify_providers.py:830:20: S603 `subprocess` call: check for execution of untrusted input
+ scripts/in_container/verify_providers.py:830:20: S607 Starting a process with a partial executable path
+ scripts/tools/check_if_limited_dependencies.py:46:16: S603 `subprocess` call: check for execution of untrusted input
+ scripts/tools/check_if_limited_dependencies.py:46:16: S607 Starting a process with a partial executable path
+ scripts/tools/initialize_virtualenv.py:172:20: S603 `subprocess` call: check for execution of untrusted input
+ scripts/tools/initialize_virtualenv.py:172:20: S607 Starting a process with a partial executable path
+ scripts/tools/initialize_virtualenv.py:181:20: S603 `subprocess` call: check for execution of untrusted input
+ scripts/tools/initialize_virtualenv.py:181:20: S607 Starting a process with a partial executable path
+ scripts/tools/initialize_virtualenv.py:97:24: S603 `subprocess` call: check for execution of untrusted input
+ setup.py:139:31: S603 `subprocess` call: check for execution of untrusted input
+ setup.py:139:31: S607 Starting a process with a partial executable path
+ setup.py:140:31: S603 `subprocess` call: check for execution of untrusted input
+ setup.py:140:31: S607 Starting a process with a partial executable path
+ setup.py:870:41: S603 `subprocess` call: check for execution of untrusted input
+ setup.py:870:41: S607 Starting a process with a partial executable path
+ setup.py:878:35: S603 `subprocess` call: check for execution of untrusted input
+ setup.py:878:35: S607 Starting a process with a partial executable path
+ tests/charts/helm_template_generator.py:138:45: S603 `subprocess` call: check for execution of untrusted input
+ tests/cli/commands/test_internal_api_command.py:103:21: S603 `subprocess` call: check for execution of untrusted input
+ tests/cli/commands/test_internal_api_command.py:103:21: S607 Starting a process with a partial executable path
+ tests/cli/commands/test_webserver_command.py:257:21: S603 `subprocess` call: check for execution of untrusted input
+ tests/cli/commands/test_webserver_command.py:257:21: S607 Starting a process with a partial executable path
+ tests/conftest.py:273:35: S603 `subprocess` call: check for execution of untrusted input
+ tests/conftest.py:273:35: S607 Starting a process with a partial executable path
+ tests/core/test_impersonation_tests.py:64:88: S602 `subprocess` call with `shell=True` identified, security issue
+ tests/core/test_impersonation_tests.py:68:88: S602 `subprocess` call with `shell=True` identified, security issue
+ tests/core/test_impersonation_tests.py:76:13: S603 `subprocess` call: check for execution of untrusted input
+ tests/core/test_impersonation_tests.py:76:13: S607 Starting a process with a partial executable path
+ tests/core/test_impersonation_tests.py:88:27: S603 `subprocess` call: check for execution of untrusted input
+ tests/core/test_impersonation_tests.py:88:27: S607 Starting a process with a partial executable path
+ tests/dags/test_on_kill.py:41:23: S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
+ tests/dags/test_on_kill.py:41:23: S607 Starting a process with a partial executable path
+ tests/decorators/test_external_python.py:59:25: S603 `subprocess` call: check for execution of untrusted input
+ tests/providers/google/cloud/log/test_gcs_task_handler_system.py:77:42: S603 `subprocess` call: check for execution of untrusted input
+ tests/providers/google/cloud/log/test_gcs_task_handler_system.py:77:42: S607 Starting a process with a partial executable path
+ tests/providers/google/cloud/log/test_gcs_task_handler_system.py:78:42: S603 `subprocess` call: check for execution of untrusted input
+ tests/providers/google/cloud/log/test_gcs_task_handler_system.py:78:42: S607 Starting a process with a partial executable path
+ tests/providers/google/cloud/log/test_stackdriver_task_handler_system.py:67:42: S603 `subprocess` call: check for execution of untrusted input
+ tests/providers/google/cloud/log/test_stackdriver_task_handler_system.py:67:42: S607 Starting a process with a partial executable path
+ tests/providers/google/cloud/log/test_stackdriver_task_handler_system.py:68:42: S603 `subprocess` call: check for execution of untrusted input
+ tests/providers/google/cloud/log/test_stackdriver_task_handler_system.py:68:42: S607 Starting a process with a partial executable path
+ tests/providers/google/cloud/log/test_stackdriver_task_handler_system.py:83:42: S603 `subprocess` call: check for execution of untrusted input
+ tests/providers/google/cloud/log/test_stackdriver_task_handler_system.py:83:42: S607 Starting a process with a partial executable path
+ tests/providers/google/cloud/log/test_stackdriver_task_handler_system.py:84:42: S603 `subprocess` call: check for execution of untrusted input
+ tests/providers/google/cloud/log/test_stackdriver_task_handler_system.py:84:42: S607 Starting a process with a partial executable path
+ tests/providers/google/cloud/secrets/test_secret_manager_system.py:44:24: S603 `subprocess` call: check for execution of untrusted input
+ tests/providers/google/cloud/secrets/test_secret_manager_system.py:44:24: S607 Starting a process with a partial executable path
+ tests/providers/google/cloud/secrets/test_secret_manager_system.py:45:42: S603 `subprocess` call: check for execution of untrusted input
+ tests/providers/google/cloud/secrets/test_secret_manager_system.py:45:42: S607 Starting a process with a partial executable path
+ tests/providers/google/cloud/secrets/test_secret_manager_system.py:50:24: S603 `subprocess` call: check for execution of untrusted input
+ tests/providers/google/cloud/secrets/test_secret_manager_system.py:50:24: S607 Starting a process with a partial executable path
+ tests/providers/google/cloud/secrets/test_secret_manager_system.py:65:24: S603 `subprocess` call: check for execution of untrusted input
+ tests/providers/google/cloud/secrets/test_secret_manager_system.py:65:24: S607 Starting a process with a partial executable path
+ tests/providers/google/cloud/secrets/test_secret_manager_system.py:66:42: S603 `subprocess` call: check for execution of untrusted input
+ tests/providers/google/cloud/secrets/test_secret_manager_system.py:66:42: S607 Starting a process with a partial executable path
+ tests/providers/google/cloud/secrets/test_secret_manager_system.py:72:24: S603 `subprocess` call: check for execution of untrusted input
+ tests/providers/google/cloud/secrets/test_secret_manager_system.py:72:24: S607 Starting a process with a partial executable path
+ tests/providers/google/cloud/utils/gcp_authenticator.py:206:17: S603 `subprocess` call: check for execution of untrusted input
+ tests/providers/google/cloud/utils/gcp_authenticator.py:206:17: S607 Starting a process with a partial executable path
+ tests/system/providers/amazon/aws/example_emr_eks.py:130:9: S602 `subprocess` call with `shell=True` identified, security issue
+ tests/system/providers/amazon/aws/example_emr_eks.py:86:9: S602 `subprocess` call with `shell=True` identified, security issue
+ tests/system/providers/amazon/aws/example_sagemaker.py:173:13: S602 `subprocess` call with `shell=True` identified, security issue
+ tests/system/providers/amazon/aws/example_sagemaker.py:473:9: S602 `subprocess` call with `shell=True` identified, security issue
+ tests/task/task_runner/test_standard_task_runner.py:290:19: S605 Starting a process with a shell, possible injection detected
+ tests/test_utils/perf/perf_kit/python.py:55:17: S606 Starting a process without a shell
+ tests/utils/test_process_utils.py:141:54: S603 `subprocess` call: check for execution of untrusted input
+ tests/utils/test_process_utils.py:141:54: S607 Starting a process with a partial executable path
+ tests/utils/test_process_utils.py:147:47: S603 `subprocess` call: check for execution of untrusted input
+ tests/utils/test_process_utils.py:147:47: S607 Starting a process with a partial executable path
+ tests/utils/test_process_utils.py:152:47: S603 `subprocess` call: check for execution of untrusted input
+ tests/utils/test_process_utils.py:152:47: S607 Starting a process with a partial executable path
+ tests/utils/test_process_utils.py:161:49: S603 `subprocess` call: check for execution of untrusted input
+ tests/utils/test_process_utils.py:161:49: S607 Starting a process with a partial executable path
+ tests/utils/test_process_utils.py:169:49: S603 `subprocess` call: check for execution of untrusted input
+ tests/utils/test_process_utils.py:169:49: S607 Starting a process with a partial executable path
```

</p>
</details>
<details><summary>bokeh (+51, -0)</summary>
<p>

```diff
+ examples/output/apis/server_document/flask_server.py:46:5: S603 `subprocess` call: check for execution of untrusted input
+ examples/output/apis/server_document/flask_server.py:46:5: S607 Starting a process with a partial executable path
+ release/system.py:43:34: S602 `subprocess` call with `shell=True` identified, security issue
+ scripts/hooks/install.py:5:20: S603 `subprocess` call: check for execution of untrusted input
+ scripts/hooks/protect_branches.py:10:26: S603 `subprocess` call: check for execution of untrusted input
+ scripts/hooks/protect_branches.py:10:26: S607 Starting a process with a partial executable path
+ scripts/hooks/uninstall.py:5:20: S603 `subprocess` call: check for execution of untrusted input
+ scripts/sri.py:18:16: S603 `subprocess` call: check for execution of untrusted input
+ scripts/sri.py:21:16: S603 `subprocess` call: check for execution of untrusted input
+ setup.py:125:40: S603 `subprocess` call: check for execution of untrusted input
+ setup.py:51:31: S603 `subprocess` call: check for execution of untrusted input
+ setup.py:51:31: S607 Starting a process with a partial executable path
+ src/bokeh/ext.py:117:18: S603 `subprocess` call: check for execution of untrusted input
+ src/bokeh/resources.py:663:16: S603 `subprocess` call: check for execution of untrusted input
+ src/bokeh/resources.py:666:16: S603 `subprocess` call: check for execution of untrusted input
+ src/bokeh/util/compiler.py:398:26: S603 `subprocess` call: check for execution of untrusted input
+ src/bokeh/util/compiler.py:440:18: S603 `subprocess` call: check for execution of untrusted input
+ tests/codebase/test_code_quality.py:118:37: S603 `subprocess` call: check for execution of untrusted input
+ tests/codebase/test_code_quality.py:118:37: S607 Starting a process with a partial executable path
+ tests/codebase/test_eslint.py:37:16: S603 `subprocess` call: check for execution of untrusted input
+ tests/codebase/test_eslint.py:37:16: S607 Starting a process with a partial executable path
+ tests/codebase/test_isort.py:58:16: S603 `subprocess` call: check for execution of untrusted input
+ tests/codebase/test_isort.py:58:16: S607 Starting a process with a partial executable path
+ tests/codebase/test_js_license_set.py:50:16: S603 `subprocess` call: check for execution of untrusted input
+ tests/codebase/test_license.py:40:16: S603 `subprocess` call: check for execution of untrusted input
+ tests/codebase/test_license.py:40:16: S607 Starting a process with a partial executable path
+ tests/codebase/test_no_client_server_common.py:48:16: S603 `subprocess` call: check for execution of untrusted input
+ tests/codebase/test_no_client_server_common.py:57:16: S603 `subprocess` call: check for execution of untrusted input
+ tests/codebase/test_no_ipython_common.py:51:16: S603 `subprocess` call: check for execution of untrusted input
+ tests/codebase/test_no_pandas_common.py:53:16: S603 `subprocess` call: check for execution of untrusted input
+ tests/codebase/test_no_request_host.py:50:26: S603 `subprocess` call: check for execution of untrusted input
+ tests/codebase/test_no_request_host.py:50:26: S607 Starting a process with a partial executable path
+ tests/codebase/test_no_selenium_common.py:52:16: S603 `subprocess` call: check for execution of untrusted input
+ tests/codebase/test_no_tornado_common.py:55:16: S603 `subprocess` call: check for execution of untrusted input
+ tests/codebase/test_no_typing_extensions_common.py:49:16: S603 `subprocess` call: check for execution of untrusted input
+ tests/codebase/test_python_execution_with_OO.py:45:18: S603 `subprocess` call: check for execution of untrusted input
+ tests/codebase/test_ruff.py:33:16: S603 `subprocess` call: check for execution of untrusted input
+ tests/codebase/test_ruff.py:33:16: S607 Starting a process with a partial executable path
+ tests/support/plugins/bokeh_server.py:66:33: S603 `subprocess` call: check for execution of untrusted input
+ tests/support/plugins/jupyter_notebook.py:125:33: S603 `subprocess` call: check for execution of untrusted input
+ tests/support/util/project.py:46:16: S603 `subprocess` call: check for execution of untrusted input
+ tests/support/util/project.py:46:16: S607 Starting a process with a partial executable path
+ tests/support/util/screenshot.py:101:33: S603 `subprocess` call: check for execution of untrusted input
+ tests/test_bokehjs.py:34:33: S603 `subprocess` call: check for execution of untrusted input
+ tests/test_bokehjs.py:34:33: S607 Starting a process with a partial executable path
+ tests/test_defaults.py:55:29: S603 `subprocess` call: check for execution of untrusted input
+ tests/test_defaults.py:55:29: S607 Starting a process with a partial executable path
+ tests/test_examples.py:293:9: S603 `subprocess` call: check for execution of untrusted input
+ tests/unit/bokeh/command/subcommands/test_serve.py:430:82: S603 `subprocess` call: check for execution of untrusted input
+ tests/unit/bokeh/command/subcommands/test_serve.py:477:33: S603 `subprocess` call: check for execution of untrusted input
+ tests/unit/bokeh/test_resources.py:324:35: S603 `subprocess` call: check for execution of untrusted input
```

</p>
</details>
<details><summary>disnake (+9, -0)</summary>
<p>

```diff
+ disnake/player.py:164:37: S603 `subprocess` call: check for execution of untrusted input
+ disnake/player.py:577:42: S603 `subprocess` call: check for execution of untrusted input
+ disnake/player.py:596:13: S603 `subprocess` call: check for execution of untrusted input
+ docs/conf.py:119:36: S603 `subprocess` call: check for execution of untrusted input
+ docs/conf.py:119:36: S607 Starting a process with a partial executable path
+ setup.py:20:13: S603 `subprocess` call: check for execution of untrusted input
+ setup.py:20:13: S607 Starting a process with a partial executable path
+ setup.py:26:13: S603 `subprocess` call: check for execution of untrusted input
+ setup.py:26:13: S607 Starting a process with a partial executable path
```

</p>
</details>
<details><summary>zulip (+51, -0)</summary>
<p>

```diff
+ scripts/lib/check_rabbitmq_queue.py:136:9: S603 `subprocess` call: check for execution of untrusted input
+ scripts/lib/check_rabbitmq_queue.py:153:9: S603 `subprocess` call: check for execution of untrusted input
+ scripts/lib/hash_reqs.py:38:36: S603 `subprocess` call: check for execution of untrusted input
+ scripts/lib/puppet_cache.py:27:9: S603 `subprocess` call: check for execution of untrusted input
+ scripts/lib/puppet_cache.py:27:9: S607 Starting a process with a partial executable path
+ scripts/lib/setup_venv.py:177:55: S603 `subprocess` call: check for execution of untrusted input
+ scripts/lib/setup_venv.py:278:38: S603 `subprocess` call: check for execution of untrusted input
+ scripts/lib/sharding.py:52:13: S603 `subprocess` call: check for execution of untrusted input
+ scripts/lib/zulip_tools.py:114:13: S603 `subprocess` call: check for execution of untrusted input
+ scripts/lib/zulip_tools.py:239:31: S603 `subprocess` call: check for execution of untrusted input
+ scripts/lib/zulip_tools.py:597:27: S603 `subprocess` call: check for execution of untrusted input
+ scripts/lib/zulip_tools.py:597:27: S607 Starting a process with a partial executable path
+ scripts/lib/zulip_tools.py:679:13: S603 `subprocess` call: check for execution of untrusted input
+ tools/lib/pretty_print.py:183:24: S603 `subprocess` call: check for execution of untrusted input
+ tools/lib/pretty_print.py:183:24: S607 Starting a process with a partial executable path
+ tools/lib/provision.py:317:24: S603 `subprocess` call: check for execution of untrusted input
+ tools/lib/provision.py:317:24: S607 Starting a process with a partial executable path
+ tools/lib/provision.py:456:5: S606 Starting a process without a shell
+ tools/lib/test_script.py:125:27: S603 `subprocess` call: check for execution of untrusted input
+ tools/lib/test_script.py:125:27: S607 Starting a process with a partial executable path
+ tools/lib/test_server.py:78:35: S603 `subprocess` call: check for execution of untrusted input
+ tools/oneclickapps/prepare_digital_ocean_one_click_app_release.py:120:9: S603 `subprocess` call: check for execution of untrusted input
+ tools/oneclickapps/prepare_digital_ocean_one_click_app_release.py:120:9: S607 Starting a process with a partial executable path
+ tools/oneclickapps/prepare_digital_ocean_one_click_app_release.py:22:9: S603 `subprocess` call: check for execution of untrusted input
+ tools/oneclickapps/prepare_digital_ocean_one_click_app_release.py:22:9: S607 Starting a process with a partial executable path
+ zerver/data_import/mattermost.py:436:43: S603 `subprocess` call: check for execution of untrusted input
+ zerver/data_import/mattermost.py:436:43: S607 Starting a process with a partial executable path
+ zerver/lib/email_notifications.py:867:9: S603 `subprocess` call: check for execution of untrusted input
+ zerver/lib/export.py:1912:9: S603 `subprocess` call: check for execution of untrusted input
+ zerver/lib/export.py:1912:9: S607 Starting a process with a partial executable path
+ zerver/lib/export.py:1971:36: S603 `subprocess` call: check for execution of untrusted input
+ zerver/lib/mdiff.py:18:36: S603 `subprocess` call: check for execution of untrusted input
+ zerver/lib/test_fixtures.py:372:9: S603 `subprocess` call: check for execution of untrusted input
+ zerver/lib/test_fixtures.py:372:9: S607 Starting a process with a partial executable path
+ zerver/lib/tex.py:37:42: S603 `subprocess` call: check for execution of untrusted input
+ zerver/logging_handlers.py:24:13: S603 `subprocess` call: check for execution of untrusted input
+ zerver/logging_handlers.py:24:13: S607 Starting a process with a partial executable path
+ zerver/management/commands/compilemessages.py:73:31: S603 `subprocess` call: check for execution of untrusted input
+ zerver/management/commands/compilemessages.py:73:31: S607 Starting a process with a partial executable path
+ zerver/management/commands/export_single_user.py:50:13: S603 `subprocess` call: check for execution of untrusted input
+ zerver/management/commands/export_single_user.py:50:13: S607 Starting a process with a partial executable path
+ zerver/management/commands/import.py:57:31: S603 `subprocess` call: check for execution of untrusted input
+ zerver/management/commands/makemessages.py:203:13: S603 `subprocess` call: check for execution of untrusted input
+ zerver/management/commands/makemessages.py:203:13: S607 Starting a process with a partial executable path
+ zerver/management/commands/register_server.py:98:21: S603 `subprocess` call: check for execution of untrusted input
+ zerver/management/commands/register_server.py:98:21: S607 Starting a process with a partial executable path
+ zerver/openapi/test_curl_examples.py:100:61: S603 `subprocess` call: check for execution of untrusted input
+ zerver/tests/test_email_mirror.py:1474:13: S603 `subprocess` call: check for execution of untrusted input
+ zerver/tests/test_email_mirror.py:1489:13: S603 `subprocess` call: check for execution of untrusted input
+ zerver/views/zephyr.py:74:31: S603 `subprocess` call: check for execution of untrusted input
+ zerver/views/zephyr.py:74:31: S607 Starting a process with a partial executable path
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.3±0.35ms     2.4 MB/sec    1.05     18.2±0.52ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.09ms     3.9 MB/sec    1.04      4.5±0.10ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   562.5±30.23µs     5.2 MB/sec    1.01   568.4±18.01µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.15ms     3.5 MB/sec    1.03      7.6±0.31ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.16ms     4.8 MB/sec    1.09      9.2±0.19ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1917.8±52.02µs     8.7 MB/sec    1.06      2.0±0.06ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    225.3±6.95µs    13.1 MB/sec    1.04    234.6±7.74µs    12.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.09ms     6.5 MB/sec    1.07      4.2±0.14ms     6.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     20.8±0.83ms  1999.5 KB/sec    1.00     20.6±0.61ms  2024.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.22ms     3.2 MB/sec    1.03      5.3±0.28ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   634.6±41.01µs     4.6 MB/sec    1.00   631.5±54.32µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.42ms     3.0 MB/sec    1.02      8.8±0.43ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.3±0.40ms     4.0 MB/sec    1.01     10.4±0.40ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.11ms     7.3 MB/sec    1.00      2.3±0.10ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   245.7±20.00µs    12.0 MB/sec    1.02   249.7±21.63µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.20ms     5.5 MB/sec    1.02      4.8±0.23ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bandit/rules/shell_injection.rs`:221 on 2023-04-11 06:40_

Nit
```suggestion
            .map_or(false, |arg| arg == "shell")
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bandit/rules/shell_injection.rs`:237 on 2023-04-11 06:54_

Scanning the list of all `subprocess` even if the module isn't `subprocess` is a lot of unnecessary work. I think we can optimize this a bit by:

* Only checking if the `call_path` has a length of two.
* First match by the module name
* Then by the submodule
* Avoid allocating vectors


```rust
    checker
        .ctx
        .resolve_call_path(func)
        .and_then(|call_path| match call_path.as_slice() {
            &[module, submodule] => match module {
                "os" => match submodule {
                    "execl" | "execle" | "execlp" | "execlpe" | "execv" | "execve" | "execvp"
                    | "execvpe" | "spawnl" => Some(CallKind::NoShell),
                    "system" | "popen" | "popen2" | "popen3" | "popen4" => Some(CallKind::Shell),
                    _ => None,
                },
                "subprocess" => match submodule {
                    "Popen" | "call" | "check_call" | "check_output" | "run" => {
                        Some(CallKind::Subprocess)
                    }
                    _ => None,
                },
                "popen2" => match submodule {
                    "popen2" | "popen3" | "popen4" | "Popen3" | "Popen4" => Some(CallKind::Shell),
                    _ => None,
                },
                "commands" => match submodule {
                    "getoutput" | "getstatusoutput" => Some(CallKind::Shell),
                    _ => None,
                },
                _ => None,
            },
            _ => None,
        })
```

(I didn't write out all submodules)

```
if call_path.len() != 2 {
    return None;
}

let [module, submodule, ..rest] = call_path.as_slice();

if !
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bandit/rules/shell_injection.rs`:144 on 2023-04-11 06:55_

Let's derive some standard traits

```suggestion
#[derive(Copy, Clone, Debug)]
enum HasShell {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bandit/rules/shell_injection.rs`:295 on 2023-04-11 07:00_

It would be nice if we could avoid the `unwrap` on `find_shell_keyword`. While this is safe now, it may lead to future errors if someone changes `hash_shell`. 

How about we introduce a new `ShellKeyword` struct that has two fields: `keyword` and `has_shell` and return it from `find_shell_keyword`? We could then rewrite this to 

```
if let Some(shell_keyword) = find_shell_keyword(keywords) {
	match shell_keyword.has_shell {
		...
		
		Range::from(shell_keyword.keyword)
	}
}
```

---

_@MichaReiser approved on 2023-04-11 07:01_

---

_Comment by @MichaReiser on 2023-04-11 07:01_

Thank you for taking the time to work on this rule. This is looking great.

---

_@robyoung reviewed on 2023-04-12 20:39_

---

_Review comment by @robyoung on `crates/ruff/src/rules/flake8_bandit/rules/shell_injection.rs`:295 on 2023-04-12 20:39_

Good call, thanks. After applying the change I realise the `has_shell` name no longer makes sense so I've rejigged that a bit.

---

_@robyoung reviewed on 2023-04-12 21:47_

---

_Review comment by @robyoung on `crates/ruff/src/rules/flake8_bandit/rules/shell_injection.rs`:237 on 2023-04-12 21:47_

This looks like a great idea, if these will never change. In bandit these are configurable as you can see [here in the bandit docs](https://bandit.readthedocs.io/en/latest/config.html). I *think* `flake8-bandit` will make use of this configuration. If we want to support these being configurable in future it would be harder with this approach.

To address the performance concern I could flip the data structure to something like `HashMap<&str, CallKind>` and use the dotted module form as the key (eg `"os.system"`). I've implemented it a separate branch [over here](https://github.com/robyoung/ruff/blob/implement-bandit-shell-injection-rules-hashmap-config/crates/ruff/src/rules/flake8_bandit/rules/shell_injection.rs#L98) and it looks ok. Are there tools to help comparing the performance between the two approaches? 

---

_Review requested from @MichaReiser by @robyoung on 2023-04-12 21:48_

---

_@charliermarsh reviewed on 2023-04-13 18:39_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bandit/rules/shell_injection.rs`:237 on 2023-04-13 18:39_

My two cents: I think we should run with the match approach since it's super simple and satisfies our needs for now (and it lets us unblock + get this merged). If we want to extend these to be configurable, we can def continue exploring and benchmarking in a follow-up PR.

Looking at the Bandit docs, I _think_ that configuration is mostly used to turn rules on and off on a per-file or per-line basis, rather than to make (e.g.) the list of matching functions here configurable -- so it may not be needed anyway? But I could be misreading.

---

_@charliermarsh reviewed on 2023-04-13 18:40_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bandit/mod.rs`:40 on 2023-04-13 18:40_

Heads up: I added these here so that they get picked up in the fixture tests (i.e., when running `cargo test`).

---

_@charliermarsh reviewed on 2023-04-13 18:43_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bandit/rules/shell_injection.rs`:237 on 2023-04-13 18:43_

Looking back at the docs, maybe it _is_ configurable? So I might be wrong on that. But we can still revisit if we opt to respect and implement that configuration. 

https://github.com/PyCQA/bandit/blob/main/bandit/plugins/injection_shell.py#L137

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bandit/rules/shell_injection.rs`:237 on 2023-04-13 18:44_

Some examples of folks configuring this: https://cs.github.com/?q=%22shell_injection%22+language%3AYAML&p=1&pt=39cf6d9c09e5cd50a57cd21efcc19b97b3adf0be3156e284bd381526c71480c88c3689e17facd0388f6f735415b3e25423a39eb3425f52e3c01c1d3ede83c0856cf2ee13417f4e33c9f62672674d2694273a5a2987e885abd30da5d11bcfd6b7daf841dc69e2cea8b5c911767a46e9258d8a0792aef0dd660da846cb51b7c59a03c3fc0e30cd31d66374a2ddef50669d0b52857e08fa9bbddca9adb628bd9e505bdcd7c78d82cd9d7a568c7f7221e92be380d23ed1f561ec6599ee50f8f38465776e8b9918366148c7b57bbcd673de36a1a26468cddcdfb12e7b18715a436a456b5e662cd4e7dbcac99c2a11d41da69a89d2551dda249fce6f6be382cc3488752eff4ae2701ada51df2b290a515d6d20a92abfe2&scope=&scopeName=All+repos

---

_@charliermarsh reviewed on 2023-04-13 18:44_

---

_Merged by @charliermarsh on 2023-04-13 18:45_

---

_Closed by @charliermarsh on 2023-04-13 18:45_

---

_Comment by @charliermarsh on 2023-04-13 18:45_

Great PR, really grateful to have you involved in the project :)

---

_Branch deleted on 2023-12-11 11:41_

---
