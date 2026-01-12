```yaml
number: 5227
title: "Add `unexplained-noqa` rule"
type: pull_request
state: closed
author: tjkuson
labels: []
assignees: []
base: main
head: unexplained-noqa
created_at: 2023-06-20T22:31:40Z
updated_at: 2023-07-10T09:55:16Z
url: https://github.com/astral-sh/ruff/pull/5227
synced_at: 2026-01-12T03:36:54Z
```

# Add `unexplained-noqa` rule

---

_Pull request opened by @tjkuson on 2023-06-20 22:31_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add `unexplained-noqa` rule as `RUF101` that checks for `noqa` directives without comments. For example,

```python
foo == ""  # noqa: PLC1901
```

would be flagged and

```python
foo == ""  # noqa: PLC1901  # Check for empty string, not just falsiness.
```

would not.

Includes documentation. Related to #5182.

## Test Plan

`cargo test`


---

_Renamed from "Add `unexplained-noqa` rlue" to "Add `unexplained-noqa` rule" by @tjkuson on 2023-06-20 22:55_

---

_Comment by @github-actions[bot] on 2023-06-20 23:26_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+546, -0, 0 error(s))

<details><summary>airflow (+415, -0)</summary>
<p>

```diff
+ airflow/api/common/experimental/delete_dag.py:22:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/api/common/experimental/mark_tasks.py:23:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/api/common/experimental/trigger_dag.py:22:47: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/cli/cli_config.py:94:43: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ airflow/configuration.py:159:65: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: UP006`
+ airflow/dag_processing/processor.py:496:88: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/executors/celery_executor_utils.py:79:24: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ airflow/executors/celery_executor_utils.py:85:38: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ airflow/executors/celery_executor_utils.py:88:23: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ airflow/executors/celery_executor_utils.py:93:35: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ airflow/hooks/dbapi.py:24:25: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/hooks/dbapi.py:25:17: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/kubernetes/pod_launcher.py:24:80: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: autoflake`
+ airflow/kubernetes/pod_runtime_info_env.py:27:102: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/kubernetes/volume.py:27:77: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: autoflake`
+ airflow/kubernetes/volume_mount.py:27:88: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: autoflake`
+ airflow/macros/__init__.py:20:14: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/macros/__init__.py:21:14: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/macros/__init__.py:22:14: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/macros/__init__.py:24:28: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/macros/__init__.py:27:18: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/macros/__init__.py:30:36: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/models/dagparam.py:23:44: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/models/dagrun.py:1271:52: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/models/xcom.py:64:21: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ airflow/providers/amazon/aws/hooks/rds.py:28:43: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/providers/amazon/aws/hooks/redshift_data.py:28:72: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/providers/apache/hdfs/sensors/hdfs.py:41:37: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: D101 Ignore missing docstring`
+ airflow/providers/apache/hdfs/sensors/hdfs.py:46:38: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: D101 Ignore missing docstring`
+ airflow/providers/apprise/hooks/apprise.py:90:14: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: D301`
+ airflow/providers/apprise/notifications/apprise.py:52:10: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: D301`
+ airflow/providers/cncf/kubernetes/operators/kubernetes_pod.py:23:64: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/providers/cncf/kubernetes/triggers/kubernetes_pod.py:23:63: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/providers/cncf/kubernetes/utils/pod_manager.py:563:171: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/providers/google/cloud/hooks/bigquery.py:1729:9: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `#   https://cloud.google.com/bigquery/docs/reference/rest/v2/jobs#configuration.query.tableDefinitions.(key).sourceFormat # noqa`
+ airflow/providers/google/cloud/hooks/bigquery.py:2141:9: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `#   https://cloud.google.com/bigquery/docs/reference/rest/v2/jobs#configuration.query.schemaUpdateOptions  # noqa`
+ airflow/providers/google/cloud/hooks/bigquery.py:54:42: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/providers/google/cloud/operators/dataproc.py:136:10: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ airflow/providers/microsoft/azure/hooks/asb.py:47:156: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/providers/papermill/operators/papermill.py:45:52: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: UP007`
+ airflow/providers/papermill/operators/papermill.py:46:38: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: UP007`
+ airflow/providers/slack/notifications/slack_notifier.py:23:72: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/providers/snowflake/operators/snowflake.py:433:10: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/sensors/base.py:50:54: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ airflow/sentry.py:61:33: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ airflow/task/task_runner/base_task_runner.py:31:31: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/utils/db.py:43:81: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ airflow/utils/yaml.py:33:56: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/www/security.py:481:45: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ airflow/www/security.py:482:47: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ dev/breeze/src/airflow_breeze/breeze.py:26:56: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# isort: skip # noqa`
+ dev/breeze/src/airflow_breeze/breeze.py:31:57: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ dev/breeze/src/airflow_breeze/breeze.py:32:59: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ dev/breeze/src/airflow_breeze/breeze.py:33:65: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ dev/breeze/src/airflow_breeze/breeze.py:34:75: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ dev/breeze/src/airflow_breeze/breeze.py:35:75: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ dev/breeze/src/airflow_breeze/breeze.py:36:85: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ dev/breeze/src/airflow_breeze/breeze.py:37:57: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ dev/breeze/src/airflow_breeze/breeze.py:38:59: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ dev/breeze/src/airflow_breeze/breeze.py:39:73: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ dev/breeze/src/airflow_breeze/commands/release_candidate_command.py:205:234: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: 501`
+ dev/breeze/src/airflow_breeze/configure_rich_click.py:21:45: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# isort:skip # noqa`
+ dev/breeze/src/airflow_breeze/utils/click_utils.py:22:45: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# type: ignore[assignment] # noqa`
+ dev/provider_packages/prepare_provider_packages.py:122:58: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# type: ignore[attr-defined] # isort:skip # noqa`
+ docs/conf.py:50:69: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ scripts/ci/pre_commit/common_precommit_black_utils.py:28:65: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# isort: skip # noqa E402`
+ scripts/ci/pre_commit/pre_commit_check_pre_commit_hooks.py:29:39: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# isort: skip # noqa: E402`
+ scripts/ci/pre_commit/pre_commit_check_pre_commit_hooks.py:34:56: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# isort: skip # noqa E402`
+ scripts/ci/pre_commit/pre_commit_check_pre_commit_hooks.py:36:38: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ scripts/ci/pre_commit/pre_commit_check_pre_commit_hooks.py:37:25: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ scripts/ci/pre_commit/pre_commit_check_pre_commit_hooks.py:39:14: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ scripts/ci/pre_commit/pre_commit_check_pre_commit_hooks.py:40:35: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ scripts/ci/pre_commit/pre_commit_check_pre_commit_hooks.py:41:32: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ scripts/ci/pre_commit/pre_commit_compile_www_assets.py:26:56: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# isort: skip # noqa E402`
+ scripts/ci/pre_commit/pre_commit_update_common_sql_api_stubs.py:39:63: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# isort: skip # noqa E402`
+ scripts/ci/pre_commit/pre_commit_update_common_sql_api_stubs.py:40:56: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# isort: skip # noqa E402`
+ scripts/ci/pre_commit/pre_commit_update_versions.py:26:85: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ scripts/ci/pre_commit/pre_commit_version_heads_map.py:33:58: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/always/test_connection.py:187:164: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/charts/helm_template_generator.py:44:142: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/charts/helm_template_generator.py:46:157: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/conftest.py:43:31: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/conftest.py:44:49: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/conftest.py:45:44: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/dags/subdir1/test_ignore_this.py:21:33: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ tests/decorators/test_external_python.py:67:26: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/decorators/test_python_virtualenv.py:112:30: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/decorators/test_python_virtualenv.py:38:26: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/decorators/test_python_virtualenv.py:61:34: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/decorators/test_python_virtualenv.py:79:30: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/executors/test_celery_executor.py:29:38: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/executors/test_dask_executor.py:37:36: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ tests/integration/executors/test_celery_executor.py:29:38: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/models/test_dagbag.py:169:33: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ tests/operators/test_python.py:820:26: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/operators/test_python.py:835:34: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/operators/test_python.py:844:30: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/operators/test_python.py:859:30: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/operators/test_python.py:865:30: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/operators/test_python.py:871:30: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/operators/test_python.py:878:30: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/providers/amazon/aws/hooks/test_base_aws.py:114:31: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/providers/amazon/aws/log/test_s3_task_handler.py:146:151: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/providers/apache/flink/sensors/test_flink_kubernetes.py:238:935: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/providers/apache/flink/sensors/test_flink_kubernetes.py:382:935: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/providers/apache/flink/sensors/test_flink_kubernetes.py:534:1143: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/providers/apache/flink/sensors/test_flink_kubernetes.py:693:935: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/providers/apache/flink/sensors/test_flink_kubernetes.py:835:938: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/providers/common/sql/operators/test_sql.py:133:10: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa 501`
+ tests/providers/common/sql/operators/test_sql.py:141:10: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa 501`
+ tests/providers/google/cloud/hooks/test_automl.py:74:47: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ tests/providers/google/cloud/log/test_gcs_task_handler.py:241:117: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/providers/google/cloud/operators/test_dataproc.py:137:114: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ tests/providers/google/cloud/transfers/test_postgres_to_gcs.py:38:165: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ tests/providers/google/marketing_platform/hooks/test_display_video.py:499:121: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ tests/system/providers/airbyte/example_airbyte_trigger_job.py:64:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/alibaba/example_oss_bucket.py:52:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/alibaba/example_oss_object.py:74:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_appflow.py:129:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_athena.py:183:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_batch.py:283:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_cloudformation.py:117:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_datasync.py:243:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_dms.py:437:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_dynamodb.py:109:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_dynamodb_to_s3.py:207:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_ec2.py:176:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_ecs.py:221:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_ecs_fargate.py:166:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_eks_templated.py:159:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_eks_with_fargate_in_one_step.py:150:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_eks_with_fargate_profile.py:186:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_eks_with_nodegroup_in_one_step.py:164:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_eks_with_nodegroups.py:209:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_emr.py:229:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_emr_eks.py:347:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_emr_notebook_execution.py:120:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_emr_serverless.py:162:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_ftp_to_s3.py:80:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_gcs_to_s3.py:79:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_glacier_to_gcs.py:116:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_glue.py:211:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_google_api_sheets_to_s3.py:90:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_google_api_youtube_to_s3.py:203:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:157:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_imap_attachment_to_s3.py:97:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_lambda.py:137:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_local_to_s3.py:100:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_mongo_to_s3.py:91:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_quicksight.py:227:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_rds_event.py:128:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_rds_export.py:185:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_rds_instance.py:120:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_rds_snapshot.py:147:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_redshift.py:285:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_redshift_s3_transfers.py:279:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_s3.py:306:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_s3_to_ftp.py:79:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_s3_to_sftp.py:79:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_s3_to_sql.py:282:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_sagemaker.py:229:5663: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/system/providers/amazon/aws/example_sagemaker.py:703:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_sagemaker_endpoint.py:159:118: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/system/providers/amazon/aws/example_sagemaker_endpoint.py:299:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_salesforce_to_s3.py:85:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_sftp_to_s3.py:79:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_sns.py:84:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_sql_to_s3.py:233:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_sqs.py:109:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/amazon/aws/example_step_functions.py:121:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/apache/cassandra/example_cassandra_dag.py:51:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/apache/drill/example_drill_dag.py:49:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/apache/druid/example_druid_dag.py:57:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/apache/hive/example_twitter_dag.py:163:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/apache/kafka/example_dag_event_listener.py:147:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/apache/kafka/example_dag_hello_kafka.py:243:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/apache/kylin/example_kylin_dag.py:117:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/apache/livy/example_livy.py:82:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/apache/pig/example_pig.py:47:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/apache/spark/example_spark_dag.py:77:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/asana/example_asana.py:108:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/cncf/kubernetes/example_kubernetes.py:175:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/cncf/kubernetes/example_kubernetes_async.py:179:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/cncf/kubernetes/example_kubernetes_resource.py:80:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/cncf/kubernetes/example_spark_kubernetes.py:81:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/common/sql/example_sql_column_table_check.py:82:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/common/sql/example_sql_execute_query.py:59:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/databricks/example_databricks.py:88:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/databricks/example_databricks_repos.py:87:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/databricks/example_databricks_sensors.py:99:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/databricks/example_databricks_sql.py:121:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/dbt/cloud/example_dbt_cloud.py:110:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/dingding/example_dingding.py:217:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/docker/example_docker.py:62:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/docker/example_docker_copy_data.py:108:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/docker/example_docker_swarm.py:51:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/docker/example_taskflow_api_docker_virtualenv.py:119:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/elasticsearch/example_elasticsearch_query.py:84:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/ftp/example_ftp.py:95:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/github/example_github.py:102:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/ads/example_ads.py:124:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/automl/example_automl_dataset.py:198:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/automl/example_automl_model.py:284:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/automl/example_automl_nl_text_extraction.py:158:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/automl/example_automl_vision_classification.py:155:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/azure/example_azure_fileshare_to_gcs.py:87:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/bigquery/example_bigquery_dataset.py:94:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/bigquery/example_bigquery_dts.py:188:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/bigquery/example_bigquery_operations.py:106:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/bigquery/example_bigquery_operations_location.py:86:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/bigquery/example_bigquery_queries_async.py:269:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/bigquery/example_bigquery_sensors.py:171:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/bigquery/example_bigquery_tables.py:231:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/bigquery/example_bigquery_to_bigquery.py:109:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/bigquery/example_bigquery_to_gcs.py:105:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/bigquery/example_bigquery_to_gcs_async.py:104:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/bigquery/example_bigquery_to_mssql.py:97:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/bigquery/example_bigquery_to_mysql.py:96:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/bigquery/example_bigquery_to_postgres.py:96:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/bigquery/example_bigquery_transfer.py:123:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/bigtable/example_bigtable.py:220:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/cloud_build/example_cloud_build.py:213:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/cloud_build/example_cloud_build_async.py:217:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/cloud_build/example_cloud_build_trigger.py:159:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/cloud_functions/example_functions.py:155:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/cloud_memorystore/example_cloud_memorystore_memcached.py:150:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/cloud_memorystore/example_cloud_memorystore_redis.py:271:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:320:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/composer/example_cloud_composer.py:133:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/composer/example_cloud_composer_deferrable.py:123:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/compute/example_compute.py:273:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/compute/example_compute_igm.py:241:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/compute/example_compute_ssh.py:144:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/data_loss_prevention/example_dlp_deidentify_content.py:165:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/data_loss_prevention/example_dlp_info_types.py:159:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/data_loss_prevention/example_dlp_inspect_template.py:123:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/data_loss_prevention/example_dlp_job.py:98:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/data_loss_prevention/example_dlp_job_trigger.py:103:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataflow/example_dataflow_go.py:151:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataflow/example_dataflow_native_java.py:121:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataflow/example_dataflow_native_python.py:122:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataflow/example_dataflow_native_python_async.py:196:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataflow/example_dataflow_template.py:138:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataform/example_dataform.py:304:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/datafusion/example_datafusion.py:298:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/datafusion/example_datafusion_async.py:278:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataplex/example_dataplex.py:211:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataprep/example_dataprep.py:172:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc/example_dataproc_batch.py:179:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc/example_dataproc_batch_deferrable.py:87:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc/example_dataproc_batch_persistent.py:120:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc/example_dataproc_cluster_deferrable.py:121:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc/example_dataproc_cluster_generator.py:120:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc/example_dataproc_cluster_update.py:114:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc/example_dataproc_gke.py:143:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc/example_dataproc_gke.py:69:121: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ tests/system/providers/google/cloud/dataproc/example_dataproc_hadoop.py:139:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc/example_dataproc_hive.py:112:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc/example_dataproc_pig.py:106:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc/example_dataproc_pyspark.py:136:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc/example_dataproc_spark.py:108:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc/example_dataproc_spark_async.py:116:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc/example_dataproc_spark_deferrable.py:109:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc/example_dataproc_spark_sql.py:104:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc/example_dataproc_sparkr.py:130:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc/example_dataproc_workflow.py:101:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc/example_dataproc_workflow_deferrable.py:106:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc_metastore/example_dataproc_metastore.py:196:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc_metastore/example_dataproc_metastore_backup.py:133:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/dataproc_metastore/example_dataproc_metastore_hive_partition_sensor.py:68:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/datastore/example_datastore_commit.py:96:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/datastore/example_datastore_export_import.py:111:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/datastore/example_datastore_query.py:81:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/datastore/example_datastore_rollback.py:64:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_calendar_to_gcs.py:75:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_firestore.py:163:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_gcs_acl.py:119:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_gcs_copy_delete.py:128:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_gcs_sensor.py:201:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_gcs_to_bigquery.py:88:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_gcs_to_bigquery_async.py:180:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_gcs_to_gcs.py:243:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_gcs_to_gdrive.py:126:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_gcs_to_sheets.py:83:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_gcs_transform.py:99:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_gcs_transform_timespan.py:118:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_gcs_upload_download.py:99:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_gdrive_to_gcs.py:108:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_mssql_to_gcs.py:86:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_mysql_to_gcs.py:79:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_oracle_to_gcs.py:71:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_s3_to_gcs.py:105:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_sftp_to_gcs.py:127:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_sheets.py:108:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_sheets_to_gcs.py:73:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/gcs/example_trino_to_gcs.py:229:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/kubernetes_engine/example_kubernetes_engine.py:116:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/kubernetes_engine/example_kubernetes_engine_async.py:120:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/life_sciences/example_life_sciences.py:139:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/ml_engine/example_mlengine.py:325:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/ml_engine/example_mlengine_async.py:326:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/natural_language/example_natural_language.py:121:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/pubsub/example_pubsub.py:155:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/pubsub/example_pubsub_deferrable.py:110:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/spanner/example_spanner.py:168:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/speech_to_text/example_speech_to_text.py:95:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/sql_to_sheets/example_sql_to_sheets.py:100:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/stackdriver/example_stackdriver.py:234:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/storage_transfer/example_cloud_storage_transfer_service_gcp.py:207:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/tasks/example_queue.py:170:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/tasks/example_tasks.py:166:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/text_to_speech/example_text_to_speech.py:86:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/transfers/example_gcs_to_sftp.py:178:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/transfers/example_gdrive_to_local.py:115:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/translate/example_translate.py:65:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/translate_speech/example_translate_speech.py:118:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/vertex_ai/example_vertex_ai_auto_ml_forecasting_training.py:192:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/vertex_ai/example_vertex_ai_auto_ml_image_training.py:182:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/vertex_ai/example_vertex_ai_auto_ml_list_training.py:56:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/vertex_ai/example_vertex_ai_auto_ml_tabular_training.py:186:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/vertex_ai/example_vertex_ai_auto_ml_text_training.py:180:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/vertex_ai/example_vertex_ai_auto_ml_video_training.py:176:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/vertex_ai/example_vertex_ai_batch_prediction_job.py:231:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/vertex_ai/example_vertex_ai_custom_container.py:181:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/vertex_ai/example_vertex_ai_custom_job.py:173:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/vertex_ai/example_vertex_ai_custom_job_python_package.py:188:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/vertex_ai/example_vertex_ai_dataset.py:304:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/vertex_ai/example_vertex_ai_endpoint.py:248:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/vertex_ai/example_vertex_ai_hyperparameter_tuning_job.py:155:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/vertex_ai/example_vertex_ai_list_custom_jobs.py:54:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/vertex_ai/example_vertex_ai_model_service.py:243:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/video_intelligence/example_video_intelligence.py:165:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/vision/example_vision_annotate_image.py:198:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/vision/example_vision_autogenerated.py:275:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/vision/example_vision_explicit.py:286:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/cloud/workflows/example_workflows.py:240:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/datacatalog/example_datacatalog_entries.py:208:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/datacatalog/example_datacatalog_search_catalog.py:229:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/datacatalog/example_datacatalog_tag_templates.py:191:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/datacatalog/example_datacatalog_tags.py:241:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/leveldb/example_leveldb.py:69:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/marketing_platform/example_analytics.py:97:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/marketing_platform/example_campaign_manager.py:213:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/marketing_platform/example_search_ads.py:86:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/google/suite/example_local_to_drive.py:82:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/http/example_http.py:116:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/influxdb/example_influxdb.py:70:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/influxdb/example_influxdb_query.py:46:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/jdbc/example_jdbc_queries.py:73:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/jenkins/example_jenkins_job_trigger.py:75:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/microsoft/azure/example_adf_run_pipeline.py:114:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/microsoft/azure/example_adls_delete.py:56:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/microsoft/azure/example_azure_blob_to_gcs.py:91:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/microsoft/azure/example_azure_container_instances.py:56:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/microsoft/azure/example_azure_cosmosdb.py:72:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/microsoft/azure/example_azure_service_bus.py:181:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/microsoft/azure/example_azure_synapse.py:70:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/microsoft/azure/example_fileshare.py:64:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/microsoft/azure/example_local_to_adls.py:56:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/microsoft/azure/example_local_to_wasb.py:52:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/microsoft/azure/example_sftp_to_wasb.py:84:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/microsoft/mssql/example_mssql.py:154:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/microsoft/winrm/example_winrm.py:79:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/mysql/example_mysql.py:61:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/neo4j/example_neo4j.py:50:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/opsgenie/example_opsgenie_alert.py:55:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/papermill/example_papermill.py:54:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/papermill/example_papermill_verify.py:76:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/plexus/example_plexus.py:53:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/postgres/example_postgres.py:89:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/presto/example_gcs_to_presto.py:52:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/qubole/example_qubole.py:232:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/qubole/example_qubole_sensors.py:91:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/redis/example_redis_publish.py:92:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/salesforce/example_bulk.py:94:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/salesforce/example_salesforce_apex_rest.py:45:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/singularity/example_singularity.py:54:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/slack/example_slack.py:55:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/slack/example_sql_to_slack.py:53:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/snowflake/example_s3_to_snowflake.py:63:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/snowflake/example_snowflake.py:95:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/snowflake/example_snowflake_to_slack.py:63:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/sqlite/example_sqlite.py:100:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/tableau/example_tableau.py:73:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/tabular/example_tabular.py:52:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/telegram/example_telegram.py:48:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/trino/example_gcs_to_trino.py:52:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/trino/example_trino.py:96:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/yandex/example_yandexcloud.py:195:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/yandex/example_yandexcloud_dataproc.py:171:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/yandex/example_yandexcloud_dataproc_lightweight.py:79:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/system/providers/zendesk/example_zendesk_custom_get.py:48:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ tests/test_utils/perf/perf_kit/memory.py:86:25: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ tests/utils/test_db.py:47:22: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/utils/test_log_handlers.py:243:152: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:556:6: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:596:159: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:597:118: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:598:118: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:599:118: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:600:408: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:601:108: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:606:206: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:607:128: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:613:208: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:618:102: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:619:159: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:620:118: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:621:408: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:622:108: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:623:206: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:624:128: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:630:208: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:694:6: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ tests/utils/test_log_handlers.py:708:10: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
```

</p>
</details>
<details><summary>bokeh (+20, -0)</summary>
<p>

```diff
+ examples/basic/data/color_mappers.py:9:5: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ examples/interaction/js_callbacks/color_sliders.py:9:5: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ examples/models/buttons.py:15:5: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ examples/models/calendars.py:14:5: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ examples/models/sliders.py:9:5: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ examples/models/widgets.py:11:5: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ examples/plotting/glyphs.py:8:5: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ examples/plotting/sprint.py:13:5: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ examples/topics/stats/splom.py:13:5: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E501`
+ src/bokeh/core/property/pd.py:31:35: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ src/bokeh/core/property/pd.py:32:54: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ src/bokeh/embed/util.py:143:32: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa`
+ src/bokeh/model/util.py:167:28: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ src/bokeh/model/util.py:168:30: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/codebase/test_importlib_metadata.py:25:18: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# isort:skip # noqa`
+ tests/support/defaults.py:88:28: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/unit/bokeh/model/test_model.py:22:29: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F403`
+ tests/unit/bokeh/model/test_model.py:24:31: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F403`
+ tests/unit/bokeh/test_objects.py:354:23: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F841`
+ tests/unit/bokeh/test_objects.py:492:23: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F841`
```

</p>
</details>
<details><summary>cibuildwheel (+12, -0)</summary>
<p>

```diff
+ cibuildwheel/_compat/tomllib.py:6:31: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: TID251`
+ cibuildwheel/_compat/tomllib.py:8:29: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: TID251`
+ cibuildwheel/_compat/typing.py:13:51: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: TID251`
+ cibuildwheel/_compat/typing.py:8:74: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: TID251`
+ cibuildwheel/bashlex_eval.py:6:42: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: TID251`
+ cibuildwheel/linux.py:379:55: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: ARG001`
+ cibuildwheel/options.py:168:47: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: PLC1901`
+ cibuildwheel/windows.py:186:17: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: ARG001`
+ cibuildwheel/windows.py:188:30: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: ARG001`
+ test/test_projects/base.py:36:70: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: PLW2901`
+ unit_test/main_tests/conftest.py:50:50: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: ARG001`
+ unit_test/wheel_print_test.py:26:85: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: PT012`
```

</p>
</details>
<details><summary>disnake (+37, -0)</summary>
<p>

```diff
+ disnake/enums.py:750:30: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: RUF001`
+ disnake/enums.py:758:25: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: RUF001`
+ disnake/enums.py:796:26: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: RUF001`
+ disnake/enums.py:804:31: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: RUF001`
+ disnake/ext/commands/bot_base.py:133:83: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: TRY004`
+ disnake/ext/commands/core.py:537:25: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: B012`
+ disnake/ext/commands/ctx_menus_core.py:121:25: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: B012`
+ disnake/ext/commands/ctx_menus_core.py:217:25: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: B012`
+ disnake/ext/commands/params.py:86:30: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F821`
+ disnake/ext/commands/slash_core.py:650:25: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: B012`
+ disnake/ext/commands/view.py:18:16: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: RUF001`
+ disnake/ext/commands/view.py:21:16: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: RUF001`
+ disnake/ext/commands/view.py:8:16: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: RUF001`
+ disnake/ext/commands/view.py:9:16: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: RUF001`
+ disnake/gateway.py:208:29: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: B012`
+ disnake/http.py:99:26: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: PLW0603`
+ disnake/interactions/message.py:135:82: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: TRY002`
+ disnake/member.py:189:57: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: B023`
+ disnake/member.py:893:82: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: TRY002`
+ disnake/member.py:962:74: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: TRY002`
+ disnake/mentions.py:125:33: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E712`
+ disnake/mentions.py:127:36: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E712`
+ disnake/mentions.py:130:33: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E712`
+ disnake/mentions.py:132:36: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E712`
+ disnake/message.py:1085:60: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: B007`
+ disnake/opus.py:230:18: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: PLW0603`
+ disnake/opus.py:237:47: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: PLW0603`
+ disnake/opus.py:242:42: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: PLW0603`
+ disnake/opus.py:284:18: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: PLW0603`
+ disnake/player.py:10:20: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: TID251`
+ disnake/player.py:559:36: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: B012`
+ docs/conf.py:19:20: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: TID251`
+ docs/extensions/redirects.py:35:40: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: S101`
+ noxfile.py:235:28: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: PLW0603`
+ scripts/codemods/typed_flags.py:121:49: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: B007`
+ setup.py:17:28: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: TID251`
+ tests/test_utils.py:399:35: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: DTZ005`
```

</p>
</details>
<details><summary>scikit-build (+11, -0)</summary>
<p>

```diff
+ docs/conf.py:33:17: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: E402`
+ skbuild/command/bdist.py:5:20: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ skbuild/command/build.py:5:20: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ skbuild/command/clean.py:8:20: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ skbuild/constants.py:103:32: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: PLW0603`
+ tests/__init__.py:3:20: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/samples/issue-668-symbol-visibility/hello/__init__.py:4:17: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/samples/issue-668-symbol-visibility/hello/__init__.py:5:13: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ tests/test_command_line.py:115:31: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: PT017`
+ tests/test_issue274_support_default_package_dir.py:33:35: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: PT017`
+ tests/test_issue274_support_one_package_without_package_dir.py:33:35: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: PT017`
```

</p>
</details>
<details><summary>scikit-build-core (+13, -0)</summary>
<p>

```diff
+ src/scikit_build_core/_logging.py:131:43: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# type: ignore[call-overload] # noqa: T201`
+ src/scikit_build_core/_shutil.py:97:61: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: PTH101`
+ src/scikit_build_core/builder/wheel_tag.py:134:17: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: T201`
+ src/scikit_build_core/file_api/_cattrs_converter.py:6:56: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: TID251`
+ src/scikit_build_core/file_api/reply.py:6:69: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: TID251`
+ src/scikit_build_core/resources/_editable_redirect.py:101:21: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: T201`
+ src/scikit_build_core/resources/_editable_redirect.py:75:73: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: T201`
+ src/scikit_build_core/resources/_editable_redirect.py:86:21: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: T201`
+ src/scikit_build_core/settings/sources.py:207:63: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: ARG002`
+ src/scikit_build_core/settings/sources.py:212:44: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: ARG002`
+ src/scikit_build_core/settings/sources.py:240:32: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: ARG002`
+ src/scikit_build_core/settings/sources.py:380:63: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: ARG002`
+ src/scikit_build_core/settings/sources.py:388:62: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: ARG002`
```

</p>
</details>
<details><summary>zulip (+38, -0)</summary>
<p>

```diff
+ scripts/lib/pythonrc.py:2:39: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ scripts/lib/pythonrc.py:4:37: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F403`
+ scripts/lib/pythonrc.py:5:34: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F403`
+ scripts/lib/setup_path.py:15:46: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: S102`
+ scripts/lib/zulip_tools.py:189:69: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: DTZ005`
+ scripts/lib/zulip_tools.py:305:69: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: DTZ005`
+ scripts/lib/zulip_tools.py:317:76: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: DTZ007`
+ scripts/lib/zulip_tools.py:334:84: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: DTZ005`
+ scripts/lib/zulip_tools.py:482:36: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: SIM115`
+ tools/lib/provision.py:399:60: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: SIM112`
+ tools/lib/provision.py:400:62: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: SIM112`
+ tools/lib/provision.py:401:56: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: SIM112`
+ tools/lib/provision.py:457:55: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: S102`
+ zerver/apps.py:30:32: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ zerver/lib/singleton_bmemcached.py:10:56: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: S301`
+ zerver/lib/templates.py:175:33: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: S308`
+ zerver/lib/templates.py:177:64: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: S308`
+ zerver/lib/test_helpers.py:232:65: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: SIM115`
+ zerver/lib/test_helpers.py:78:59: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: N801`
+ zerver/lib/test_helpers.py:81:49: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: N801`
+ zerver/lib/test_helpers.py:84:49: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: N801`
+ zerver/management/commands/import.py:1:1: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: N999`
+ zerver/models.py:849:52: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: N805`
+ zerver/models.py:853:59: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: N805`
+ zerver/models.py:954:59: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: N805`
+ zerver/views/upload.py:120:61: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: SIM115`
+ zerver/views/upload.py:255:76: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: SIM115`
+ zerver/webhooks/taiga/view.py:311:61: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: SIM118`
+ zproject/configured_settings.py:15:35: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F403 isort: skip`
+ zproject/configured_settings.py:19:44: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F403 isort: skip`
+ zproject/configured_settings.py:20:34: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F403 isort: skip`
+ zproject/configured_settings.py:8:34: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F403 isort: skip`
+ zproject/prod_settings.pyi:1:40: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F403`
+ zproject/prod_settings_template.py:163:83: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F401`
+ zproject/settings.py:26:37: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F403 isort: skip`
+ zproject/settings.py:27:35: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F403 isort: skip`
+ zproject/test_settings.py:20:26: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F403 isort: skip`
+ zproject/test_settings.py:21:37: RUF101 Unexplained `noqa` directive, consider adding an explanation comment to `# noqa: F403 isort: skip`
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| RUF101 | 546 | 546 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.01ms     5.2 MB/sec    1.09      8.6±0.03ms     4.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   1878.6±2.44µs     8.9 MB/sec    1.06   1984.4±4.68µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    227.6±0.54µs    13.0 MB/sec    1.04    237.4±3.82µs    12.4 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.1 MB/sec    1.05      4.4±0.02ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.09ms     2.6 MB/sec    1.04     16.2±0.05ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.01ms     4.2 MB/sec    1.03      4.0±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    503.5±1.46µs     5.9 MB/sec    1.03    520.7±1.26µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.04ms     3.7 MB/sec    1.03      7.1±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.01ms     5.2 MB/sec    1.08      8.5±0.02ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1725.5±4.66µs     9.6 MB/sec    1.07   1854.7±5.06µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.6±1.70µs    15.3 MB/sec    1.08    208.3±0.49µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.1 MB/sec    1.06      3.8±0.01ms     6.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.07     11.1±0.42ms     3.7 MB/sec    1.00     10.4±0.41ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.5±0.13ms     6.8 MB/sec    1.00      2.4±0.20ms     6.9 MB/sec
formatter/numpy/globals.py                 1.06   325.5±21.46µs     9.1 MB/sec    1.00   308.3±27.98µs     9.6 MB/sec
formatter/pydantic/types.py                1.09      5.8±0.32ms     4.4 MB/sec    1.00      5.3±0.29ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.06     20.9±0.73ms  1989.9 KB/sec    1.00     19.8±0.57ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      5.6±0.24ms     3.0 MB/sec    1.00      5.3±0.24ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   670.2±26.68µs     4.4 MB/sec    1.00   663.8±46.53µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      9.4±0.32ms     2.7 MB/sec    1.00      9.3±0.35ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.06     11.5±0.55ms     3.5 MB/sec    1.00     10.8±0.39ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06      2.5±0.11ms     6.8 MB/sec    1.00      2.3±0.13ms     7.1 MB/sec
linter/default-rules/numpy/globals.py      1.01   273.2±11.37µs    10.8 MB/sec    1.00   270.9±14.62µs    10.9 MB/sec
linter/default-rules/pydantic/types.py     1.03      5.0±0.20ms     5.1 MB/sec    1.00      4.9±0.20ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @tjkuson on 2023-06-21 16:46_

---

_@MichaReiser reviewed on 2023-06-21 16:52_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/noqa.rs`:41 on 2023-06-21 16:52_

Captures are kind of expensive. Can we use `text.trim_start().find("# noqa")` to find the starting position. Or @charliermarsh do we already have infrastructure to parse out noqa comments (we should, right?)

---

_@konstin reviewed on 2023-06-21 17:35_

---

_Review comment by @konstin on `crates/ruff/src/checkers/noqa.rs`:41 on 2023-06-21 17:35_

i think the regex is partially copied from https://github.com/astral-sh/ruff/blob/f401050878a3ca2506c65c29968a943557f1fdd8/crates/ruff/src/noqa.rs#L21-L26

---

_@tjkuson reviewed on 2023-06-21 18:09_

---

_Review comment by @tjkuson on `crates/ruff/src/checkers/noqa.rs`:41 on 2023-06-21 18:09_

Yes, I just adapted the current `noqa` regex as it seems to be how `noqa` directives are currently parsed. Unless there is a better way to parse the comments, I'll implement the suggestion to use `text.trim_start().find("# noqa")`.

---

_Review requested from @MichaReiser by @tjkuson on 2023-06-21 18:54_

---

_Comment by @tjkuson on 2023-06-21 19:52_

Changed the example used in the documentation, as the previous example is now irrelevant due to #5264.

---

_Converted to draft by @tjkuson on 2023-06-23 12:24_

---

_Comment by @tjkuson on 2023-06-23 12:24_

It doesn't work with codes that don't end in numbers, which is odd. I have made the PR a draft until this is fixed.

---

_Marked ready for review by @tjkuson on 2023-06-23 12:34_

---

_Comment by @charliermarsh on 2023-06-26 14:43_

I think we'll need to support some kind of pedantic mode before we can merge this.

---

_Comment by @tjkuson on 2023-06-27 17:29_

I will close it for now, then.

---

_Closed by @tjkuson on 2023-06-27 17:29_

---

_Branch deleted on 2023-07-10 09:55_

---
