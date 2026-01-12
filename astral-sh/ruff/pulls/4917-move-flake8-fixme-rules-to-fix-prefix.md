```yaml
number: 4917
title: Move flake8-fixme rules to FIX prefix
type: pull_request
state: merged
author: charliermarsh
labels:
  - breaking
assignees: []
merged: true
base: main
head: charlie/fixme
created_at: 2023-06-07T04:09:57Z
updated_at: 2023-06-07T21:39:25Z
url: https://github.com/astral-sh/ruff/pull/4917
synced_at: 2026-01-12T15:55:17Z
```

# Move flake8-fixme rules to FIX prefix

---

_@charliermarsh_

## Summary

We want to avoid overloading prefixes (like `T`) -- shipping these rules under the generic `T` prefix was probably an oversight.

Closes #4916.

---

_Comment by @github-actions[bot] on 2023-06-07 04:44_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+561, -561, 0 error(s))

<details><summary>airflow (+164, -164)</summary>
<p>

```diff
+ airflow/api_connexion/schemas/dag_schema.py:98:55: FIX002 Line contains TODO
- airflow/api_connexion/schemas/dag_schema.py:98:55: T002 Line contains TODO
+ airflow/cli/cli_config.py:277:6: FIX002 Line contains TODO
- airflow/cli/cli_config.py:277:6: T002 Line contains TODO
+ airflow/cli/cli_config.py:546:3: FIX002 Line contains TODO
- airflow/cli/cli_config.py:546:3: T002 Line contains TODO
+ airflow/cli/cli_config.py:562:3: FIX002 Line contains TODO
- airflow/cli/cli_config.py:562:3: T002 Line contains TODO
+ airflow/cli/commands/task_command.py:183:11: FIX002 Line contains TODO
- airflow/cli/commands/task_command.py:183:11: T002 Line contains TODO
+ airflow/cli/commands/task_command.py:224:15: FIX002 Line contains TODO
- airflow/cli/commands/task_command.py:224:15: T002 Line contains TODO
+ airflow/cli/commands/task_command.py:456:7: FIX002 Line contains TODO
- airflow/cli/commands/task_command.py:456:7: T002 Line contains TODO
+ airflow/cli/commands/task_command.py:663:11: FIX002 Line contains TODO
- airflow/cli/commands/task_command.py:663:11: T002 Line contains TODO
+ airflow/compat/functools.pyi:20:3: FIX002 Line contains TODO
- airflow/compat/functools.pyi:20:3: T002 Line contains TODO
+ airflow/dag_processing/manager.py:237:11: FIX002 Line contains TODO
- airflow/dag_processing/manager.py:237:11: T002 Line contains TODO
+ airflow/decorators/base.py:216:11: FIX002 Line contains TODO
- airflow/decorators/base.py:216:11: T002 Line contains TODO
+ airflow/decorators/task_group.py:117:11: FIX002 Line contains TODO
- airflow/decorators/task_group.py:117:11: T002 Line contains TODO
+ airflow/decorators/task_group.py:124:11: FIX002 Line contains TODO
- airflow/decorators/task_group.py:124:11: T002 Line contains TODO
+ airflow/executors/base_executor.py:168:11: FIX002 Line contains TODO
- airflow/executors/base_executor.py:168:11: T002 Line contains TODO
+ airflow/executors/executor_loader.py:205:3: FIX002 Line contains TODO
- airflow/executors/executor_loader.py:205:3: T002 Line contains TODO
+ airflow/jobs/backfill_job_runner.py:264:11: FIX002 Line contains TODO
- airflow/jobs/backfill_job_runner.py:264:11: T002 Line contains TODO
+ airflow/jobs/backfill_job_runner.py:943:15: FIX002 Line contains TODO
- airflow/jobs/backfill_job_runner.py:943:15: T002 Line contains TODO
+ airflow/jobs/job.py:191:19: FIX002 Line contains TODO
- airflow/jobs/job.py:191:19: T002 Line contains TODO
+ airflow/jobs/local_task_job_runner.py:79:41: FIX002 Line contains TODO
- airflow/jobs/local_task_job_runner.py:79:41: T002 Line contains TODO
+ airflow/jobs/scheduler_job_runner.py:1199:11: FIX002 Line contains TODO
- airflow/jobs/scheduler_job_runner.py:1199:11: T002 Line contains TODO
+ airflow/jobs/scheduler_job_runner.py:1339:19: FIX002 Line contains TODO
- airflow/jobs/scheduler_job_runner.py:1339:19: T002 Line contains TODO
+ airflow/jobs/scheduler_job_runner.py:1454:11: FIX002 Line contains TODO
- airflow/jobs/scheduler_job_runner.py:1454:11: T002 Line contains TODO
+ airflow/jobs/scheduler_job_runner.py:168:15: FIX002 Line contains TODO
- airflow/jobs/scheduler_job_runner.py:168:15: T002 Line contains TODO
+ airflow/jobs/scheduler_job_runner.py:189:15: FIX002 Line contains TODO
- airflow/jobs/scheduler_job_runner.py:189:15: T002 Line contains TODO
+ airflow/jobs/scheduler_job_runner.py:197:15: FIX002 Line contains TODO
- airflow/jobs/scheduler_job_runner.py:197:15: T002 Line contains TODO
+ airflow/jobs/scheduler_job_runner.py:207:15: FIX002 Line contains TODO
- airflow/jobs/scheduler_job_runner.py:207:15: T002 Line contains TODO
+ airflow/jobs/scheduler_job_runner.py:405:15: FIX002 Line contains TODO
- airflow/jobs/scheduler_job_runner.py:405:15: T002 Line contains TODO
+ airflow/jobs/scheduler_job_runner.py:603:23: FIX002 Line contains TODO
- airflow/jobs/scheduler_job_runner.py:603:23: T002 Line contains TODO
+ airflow/metrics/otel_logger.py:250:7: FIX002 Line contains TODO
- airflow/metrics/otel_logger.py:250:7: T002 Line contains TODO
+ airflow/models/abstractoperator.py:499:15: FIX002 Line contains TODO
- airflow/models/abstractoperator.py:499:15: T002 Line contains TODO
+ airflow/models/baseoperator.py:1233:11: FIX004 Line contains HACK
- airflow/models/baseoperator.py:1233:11: T004 Line contains HACK
+ airflow/models/baseoperator.py:1235:40: FIX002 Line contains TODO
- airflow/models/baseoperator.py:1235:40: T002 Line contains TODO
+ airflow/models/baseoperator.py:1601:3: FIX002 Line contains TODO
- airflow/models/baseoperator.py:1601:3: T002 Line contains TODO
+ airflow/models/baseoperator.py:918:15: FIX002 Line contains TODO
- airflow/models/baseoperator.py:918:15: T002 Line contains TODO
+ airflow/models/dag.py:1012:11: FIX004 Line contains HACK
- airflow/models/dag.py:1012:11: T004 Line contains HACK
+ airflow/models/dag.py:1152:11: FIX002 Line contains TODO
- airflow/models/dag.py:1152:11: T002 Line contains TODO
+ airflow/models/dag.py:129:3: FIX001 Line contains FIXME
- airflow/models/dag.py:129:3: T001 Line contains FIXME
+ airflow/models/dag.py:1476:19: FIX002 Line contains TODO
- airflow/models/dag.py:1476:19: T002 Line contains TODO
+ airflow/models/dag.py:458:15: FIX002 Line contains TODO
- airflow/models/dag.py:458:15: T002 Line contains TODO
+ airflow/models/dagbag.py:288:11: FIX002 Line contains TODO
- airflow/models/dagbag.py:288:11: T002 Line contains TODO
+ airflow/models/dagbag.py:397:23: FIX002 Line contains TODO
- airflow/models/dagbag.py:397:23: T002 Line contains TODO
+ airflow/models/dagrun.py:1202:15: FIX002 Line contains TODO
- airflow/models/dagrun.py:1202:15: T002 Line contains TODO
+ airflow/models/dagrun.py:318:11: FIX002 Line contains TODO
- airflow/models/dagrun.py:318:11: T002 Line contains TODO
+ airflow/models/dagrun.py:932:19: FIX002 Line contains TODO
- airflow/models/dagrun.py:932:19: T002 Line contains TODO
+ airflow/models/expandinput.py:137:11: FIX002 Line contains TODO
- airflow/models/expandinput.py:137:11: T002 Line contains TODO
+ airflow/models/expandinput.py:61:11: FIX002 Line contains TODO
- airflow/models/expandinput.py:61:11: T002 Line contains TODO
+ airflow/models/skipmixin.py:187:11: FIX002 Line contains TODO
- airflow/models/skipmixin.py:187:11: T002 Line contains TODO
+ airflow/models/taskinstance.py:147:7: FIX002 Line contains TODO
- airflow/models/taskinstance.py:147:7: T002 Line contains TODO
+ airflow/models/taskinstance.py:2408:11: FIX002 Line contains TODO
- airflow/models/taskinstance.py:2408:11: T002 Line contains TODO
+ airflow/models/xcom.py:872:24: FIX004 Line contains HACK
- airflow/models/xcom.py:872:24: T004 Line contains HACK
+ airflow/providers/amazon/aws/hooks/redshift_cluster.py:85:7: FIX002 Line contains TODO
- airflow/providers/amazon/aws/hooks/redshift_cluster.py:85:7: T002 Line contains TODO
+ airflow/providers/amazon/aws/operators/emr.py:208:11: FIX002 Line contains TODO
- airflow/providers/amazon/aws/operators/emr.py:208:11: T002 Line contains TODO
+ airflow/providers/amazon/aws/operators/emr.py:324:11: FIX002 Line contains TODO
- airflow/providers/amazon/aws/operators/emr.py:324:11: T002 Line contains TODO
+ airflow/providers/amazon/aws/operators/emr.py:623:11: FIX002 Line contains TODO
- airflow/providers/amazon/aws/operators/emr.py:623:11: T002 Line contains TODO
+ airflow/providers/amazon/aws/utils/connection_wrapper.py:226:11: FIX002 Line contains TODO
- airflow/providers/amazon/aws/utils/connection_wrapper.py:226:11: T002 Line contains TODO
+ airflow/providers/apache/hive/sensors/metastore_partition.py:63:11: FIX002 Line contains TODO
- airflow/providers/apache/hive/sensors/metastore_partition.py:63:11: T002 Line contains TODO
+ airflow/providers/cncf/kubernetes/decorators/kubernetes.py:80:7: FIX002 Line contains TODO
- airflow/providers/cncf/kubernetes/decorators/kubernetes.py:80:7: T002 Line contains TODO
+ airflow/providers/cncf/kubernetes/operators/pod.py:308:11: FIX002 Line contains TODO
- airflow/providers/cncf/kubernetes/operators/pod.py:308:11: T002 Line contains TODO
+ airflow/providers/cncf/kubernetes/operators/pod.py:315:11: FIX002 Line contains TODO
- airflow/providers/cncf/kubernetes/operators/pod.py:315:11: T002 Line contains TODO
+ airflow/providers/cncf/kubernetes/operators/pod.py:387:50: FIX002 Line contains TODO
- airflow/providers/cncf/kubernetes/operators/pod.py:387:50: T002 Line contains TODO
+ airflow/providers/databricks/hooks/databricks_base.py:494:11: FIX002 Line contains TODO
- airflow/providers/databricks/hooks/databricks_base.py:494:11: T002 Line contains TODO
+ airflow/providers/databricks/operators/databricks_sql.py:344:11: FIX002 Line contains TODO
- airflow/providers/databricks/operators/databricks_sql.py:344:11: T002 Line contains TODO
+ airflow/providers/dbt/cloud/sensors/dbt.py:58:19: FIX002 Line contains TODO
- airflow/providers/dbt/cloud/sensors/dbt.py:58:19: T002 Line contains TODO
+ airflow/providers/docker/decorators/docker.py:139:7: FIX002 Line contains TODO
- airflow/providers/docker/decorators/docker.py:139:7: T002 Line contains TODO
+ airflow/providers/elasticsearch/log/es_task_handler.py:368:11: FIX002 Line contains TODO
- airflow/providers/elasticsearch/log/es_task_handler.py:368:11: T002 Line contains TODO
+ airflow/providers/github/hooks/github.py:60:11: FIX002 Line contains TODO
- airflow/providers/github/hooks/github.py:60:11: T002 Line contains TODO
+ airflow/providers/google/cloud/hooks/cloud_storage_transfer_service.py:230:11: FIX002 Line contains TODO
- airflow/providers/google/cloud/hooks/cloud_storage_transfer_service.py:230:11: T002 Line contains TODO
+ airflow/providers/google/cloud/hooks/cloud_storage_transfer_service.py:361:11: FIX002 Line contains TODO
- airflow/providers/google/cloud/hooks/cloud_storage_transfer_service.py:361:11: T002 Line contains TODO
+ airflow/providers/google/cloud/hooks/compute_ssh.py:35:3: FIX002 Line contains TODO
- airflow/providers/google/cloud/hooks/compute_ssh.py:35:3: T002 Line contains TODO
+ airflow/providers/google/cloud/hooks/datacatalog.py:1013:11: FIX004 Line contains HACK
- airflow/providers/google/cloud/hooks/datacatalog.py:1013:11: T004 Line contains HACK
+ airflow/providers/google/cloud/hooks/datacatalog.py:1085:11: FIX004 Line contains HACK
- airflow/providers/google/cloud/hooks/datacatalog.py:1085:11: T004 Line contains HACK
+ airflow/providers/google/cloud/hooks/datacatalog.py:207:11: FIX004 Line contains HACK
- airflow/providers/google/cloud/hooks/datacatalog.py:207:11: T004 Line contains HACK
+ airflow/providers/google/cloud/hooks/datacatalog.py:263:11: FIX004 Line contains HACK
- airflow/providers/google/cloud/hooks/datacatalog.py:263:11: T004 Line contains HACK
+ airflow/providers/google/cloud/hooks/datacatalog.py:940:11: FIX004 Line contains HACK
- airflow/providers/google/cloud/hooks/datacatalog.py:940:11: T004 Line contains HACK
+ airflow/providers/google/cloud/hooks/datafusion.py:437:11: FIX002 Line contains TODO
- airflow/providers/google/cloud/hooks/datafusion.py:437:11: T002 Line contains TODO
+ airflow/providers/google/cloud/hooks/gcs.py:1146:15: FIX002 Line contains TODO
- airflow/providers/google/cloud/hooks/gcs.py:1146:15: T002 Line contains TODO
+ airflow/providers/google/cloud/hooks/gcs.py:327:11: FIX002 Line contains TODO
- airflow/providers/google/cloud/hooks/gcs.py:327:11: T002 Line contains TODO
+ airflow/providers/google/cloud/hooks/kubernetes_engine.py:108:7: FIX002 Line contains TODO
- airflow/providers/google/cloud/hooks/kubernetes_engine.py:108:7: T002 Line contains TODO
+ airflow/providers/google/cloud/hooks/kubernetes_engine.py:99:7: FIX002 Line contains TODO
- airflow/providers/google/cloud/hooks/kubernetes_engine.py:99:7: T002 Line contains TODO
+ airflow/providers/google/cloud/hooks/pubsub.py:143:15: FIX002 Line contains TODO
- airflow/providers/google/cloud/hooks/pubsub.py:143:15: T002 Line contains TODO
+ airflow/providers/google/cloud/log/stackdriver_task_handler.py:173:11: FIX002 Line contains TODO
- airflow/providers/google/cloud/log/stackdriver_task_handler.py:173:11: T002 Line contains TODO
+ airflow/providers/google/cloud/log/stackdriver_task_handler.py:38:7: FIX002 Line contains TODO
- airflow/providers/google/cloud/log/stackdriver_task_handler.py:38:7: T002 Line contains TODO
+ airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py:547:11: FIX002 Line contains TODO
- airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py:547:11: T002 Line contains TODO
+ airflow/providers/google/cloud/operators/dataflow.py:1135:11: FIX002 Line contains TODO
- airflow/providers/google/cloud/operators/dataflow.py:1135:11: T002 Line contains TODO
+ airflow/providers/google/cloud/operators/dataflow.py:348:11: FIX002 Line contains TODO
- airflow/providers/google/cloud/operators/dataflow.py:348:11: T002 Line contains TODO
+ airflow/providers/google/cloud/operators/dataproc.py:1161:11: FIX002 Line contains TODO
- airflow/providers/google/cloud/operators/dataproc.py:1161:11: T002 Line contains TODO
+ airflow/providers/google/cloud/operators/dataproc.py:1236:11: FIX002 Line contains TODO
- airflow/providers/google/cloud/operators/dataproc.py:1236:11: T002 Line contains TODO
+ airflow/providers/google/cloud/operators/dataproc.py:1312:11: FIX002 Line contains TODO
- airflow/providers/google/cloud/operators/dataproc.py:1312:11: T002 Line contains TODO
+ airflow/providers/google/cloud/operators/dataproc.py:1390:11: FIX002 Line contains TODO
- airflow/providers/google/cloud/operators/dataproc.py:1390:11: T002 Line contains TODO
+ airflow/providers/google/cloud/operators/dataproc.py:1464:11: FIX002 Line contains TODO
- airflow/providers/google/cloud/operators/dataproc.py:1464:11: T002 Line contains TODO
+ airflow/providers/google/cloud/operators/dataproc.py:1563:11: FIX002 Line contains TODO
- airflow/providers/google/cloud/operators/dataproc.py:1563:11: T002 Line contains TODO
+ airflow/providers/google/cloud/operators/dataproc.py:491:11: FIX002 Line contains TODO
- airflow/providers/google/cloud/operators/dataproc.py:491:11: T002 Line contains TODO
+ airflow/providers/google/cloud/operators/dataproc.py:736:11: FIX002 Line contains TODO
- airflow/providers/google/cloud/operators/dataproc.py:736:11: T002 Line contains TODO
+ airflow/providers/google/cloud/operators/gcs.py:765:15: FIX002 Line contains TODO
- airflow/providers/google/cloud/operators/gcs.py:765:15: T002 Line contains TODO
+ airflow/providers/google/cloud/operators/gcs.py:806:15: FIX002 Line contains TODO
- airflow/providers/google/cloud/operators/gcs.py:806:15: T002 Line contains TODO
+ airflow/providers/google/cloud/sensors/bigquery.py:78:15: FIX002 Line contains TODO
- airflow/providers/google/cloud/sensors/bigquery.py:78:15: T002 Line contains TODO
+ airflow/providers/google/cloud/sensors/tasks.py:80:11: FIX002 Line contains TODO
- airflow/providers/google/cloud/sensors/tasks.py:80:11: T002 Line contains TODO
+ airflow/providers/google/cloud/transfers/gcs_to_local.py:89:11: FIX002 Line contains TODO
- airflow/providers/google/cloud/transfers/gcs_to_local.py:89:11: T002 Line contains TODO
+ airflow/providers/jenkins/operators/jenkins_job_trigger.py:154:11: FIX002 Line contains TODO
- airflow/providers/jenkins/operators/jenkins_job_trigger.py:154:11: T002 Line contains TODO
+ airflow/providers/microsoft/azure/hooks/adx.py:120:46: FIX002 Line contains TODO
- airflow/providers/microsoft/azure/hooks/adx.py:120:46: T002 Line contains TODO
+ airflow/providers/microsoft/azure/hooks/cosmos.py:74:52: FIX002 Line contains TODO
- airflow/providers/microsoft/azure/hooks/cosmos.py:74:52: T002 Line contains TODO
+ airflow/providers/microsoft/azure/log/wasb_task_handler.py:136:11: FIX002 Line contains TODO
- airflow/providers/microsoft/azure/log/wasb_task_handler.py:136:11: T002 Line contains TODO
+ airflow/providers/microsoft/winrm/hooks/winrm.py:27:3: FIX002 Line contains TODO
- airflow/providers/microsoft/winrm/hooks/winrm.py:27:3: T002 Line contains TODO
+ airflow/providers/openlineage/extractors/manager.py:150:11: FIX002 Line contains TODO
- airflow/providers/openlineage/extractors/manager.py:150:11: T002 Line contains TODO
+ airflow/providers/openlineage/plugins/listener.py:162:11: FIX002 Line contains TODO
- airflow/providers/openlineage/plugins/listener.py:162:11: T002 Line contains TODO
+ airflow/providers/openlineage/utils/utils.py:348:19: FIX002 Line contains TODO
- airflow/providers/openlineage/utils/utils.py:348:19: T002 Line contains TODO
+ airflow/providers/openlineage/utils/utils.py:40:3: FIX002 Line contains TODO
- airflow/providers/openlineage/utils/utils.py:40:3: T002 Line contains TODO
+ airflow/providers/qubole/operators/qubole_check.py:126:3: FIX002 Line contains TODO
- airflow/providers/qubole/operators/qubole_check.py:126:3: T002 Line contains TODO
+ airflow/providers/sftp/hooks/sftp.py:115:15: FIX002 Line contains TODO
- airflow/providers/sftp/hooks/sftp.py:115:15: T002 Line contains TODO
+ airflow/providers/sftp/hooks/sftp.py:81:11: FIX002 Line contains TODO
- airflow/providers/sftp/hooks/sftp.py:81:11: T002 Line contains TODO
+ airflow/providers/sftp/operators/sftp.py:129:11: FIX002 Line contains TODO
- airflow/providers/sftp/operators/sftp.py:129:11: T002 Line contains TODO
+ airflow/providers/snowflake/hooks/snowflake.py:45:3: FIX002 Line contains TODO
- airflow/providers/snowflake/hooks/snowflake.py:45:3: T002 Line contains TODO
+ airflow/secrets/base_secrets.py:93:11: FIX002 Line contains TODO
- airflow/secrets/base_secrets.py:93:11: T002 Line contains TODO
+ airflow/serialization/serde.py:165:11: FIX001 Line contains FIXME
- airflow/serialization/serde.py:165:11: T001 Line contains FIXME
+ airflow/serialization/serialized_objects.py:1049:42: FIX002 Line contains TODO
- airflow/serialization/serialized_objects.py:1049:42: T002 Line contains TODO
+ airflow/serialization/serialized_objects.py:449:15: FIX001 Line contains FIXME
- airflow/serialization/serialized_objects.py:449:15: T001 Line contains FIXME
+ airflow/serialization/serialized_objects.py:463:15: FIX001 Line contains FIXME
- airflow/serialization/serialized_objects.py:463:15: T001 Line contains FIXME
+ airflow/serialization/serialized_objects.py:647:15: FIX002 Line contains TODO
- airflow/serialization/serialized_objects.py:647:15: T002 Line contains TODO
+ airflow/serialization/serialized_objects.py:911:15: FIX002 Line contains TODO
- airflow/serialization/serialized_objects.py:911:15: T002 Line contains TODO
+ airflow/serialization/serialized_objects.py:969:15: FIX002 Line contains TODO
- airflow/serialization/serialized_objects.py:969:15: T002 Line contains TODO
+ airflow/task/task_runner/cgroup_task_runner.py:171:11: FIX002 Line contains TODO
- airflow/task/task_runner/cgroup_task_runner.py:171:11: T002 Line contains TODO
+ airflow/ti_deps/dependencies_deps.py:68:3: FIX002 Line contains TODO
- airflow/ti_deps/dependencies_deps.py:68:3: T002 Line contains TODO
+ airflow/ti_deps/deps/trigger_rule_dep.py:243:15: FIX002 Line contains TODO
- airflow/ti_deps/deps/trigger_rule_dep.py:243:15: T002 Line contains TODO
+ airflow/utils/db.py:1783:15: FIX002 Line contains TODO
- airflow/utils/db.py:1783:15: T002 Line contains TODO
+ airflow/utils/db.py:1796:15: FIX002 Line contains TODO
- airflow/utils/db.py:1796:15: T002 Line contains TODO
+ airflow/utils/db.py:42:3: FIX002 Line contains TODO
- airflow/utils/db.py:42:3: T002 Line contains TODO
+ airflow/www/api/experimental/endpoints.py:224:3: FIX002 Line contains TODO
- airflow/www/api/experimental/endpoints.py:224:3: T002 Line contains TODO
+ airflow/www/fab_security/manager.py:1486:27: FIX002 Line contains TODO
- airflow/www/fab_security/manager.py:1486:27: T002 Line contains TODO
+ airflow/www/views.py:1341:11: FIX002 Line contains TODO
- airflow/www/views.py:1341:11: T002 Line contains TODO
+ airflow/www/views.py:1753:19: FIX004 Line contains HACK
- airflow/www/views.py:1753:19: T004 Line contains HACK
+ airflow/www/views.py:2368:19: FIX002 Line contains TODO
- airflow/www/views.py:2368:19: T002 Line contains TODO
+ airflow/www/views.py:4160:11: FIX002 Line contains TODO
- airflow/www/views.py:4160:11: T002 Line contains TODO
+ airflow/www/views.py:446:19: FIX002 Line contains TODO
- airflow/www/views.py:446:19: T002 Line contains TODO
+ airflow/www/views.py:5292:21: FIX002 Line contains TODO
- airflow/www/views.py:5292:21: T002 Line contains TODO
+ airflow/www/views.py:5654:22: FIX002 Line contains TODO
- airflow/www/views.py:5654:22: T002 Line contains TODO
+ dev/stats/get_important_pr_candidates.py:351:25: FIX002 Line contains TODO
- dev/stats/get_important_pr_candidates.py:351:25: T002 Line contains TODO
+ docs/conf.py:101:3: FIX004 Line contains HACK
- docs/conf.py:101:3: T004 Line contains HACK
+ docs/conf.py:525:11: FIX002 Line contains TODO
- docs/conf.py:525:11: T002 Line contains TODO
+ kubernetes_tests/test_kubernetes_pod_operator.py:712:63: FIX002 Line contains TODO
- kubernetes_tests/test_kubernetes_pod_operator.py:712:63: T002 Line contains TODO
+ kubernetes_tests/test_kubernetes_pod_operator.py:896:11: FIX002 Line contains TODO
- kubernetes_tests/test_kubernetes_pod_operator.py:896:11: T002 Line contains TODO
+ scripts/in_container/verify_providers.py:859:11: FIX002 Line contains TODO
- scripts/in_container/verify_providers.py:859:11: T002 Line contains TODO
+ setup.py:341:7: FIX002 Line contains TODO
- setup.py:341:7: T002 Line contains TODO
+ setup.py:416:7: FIX002 Line contains TODO
- setup.py:416:7: T002 Line contains TODO
+ tests/always/test_project_structure.py:60:11: FIX002 Line contains TODO
- tests/always/test_project_structure.py:60:11: T002 Line contains TODO
+ tests/api_experimental/common/test_mark_tasks.py:410:7: FIX002 Line contains TODO
- tests/api_experimental/common/test_mark_tasks.py:410:7: T002 Line contains TODO
+ tests/cli/commands/test_dag_command.py:49:3: FIX002 Line contains TODO
- tests/cli/commands/test_dag_command.py:49:3: T002 Line contains TODO
+ tests/cli/commands/test_task_command.py:74:3: FIX002 Line contains TODO
- tests/cli/commands/test_task_command.py:74:3: T002 Line contains TODO
+ tests/core/test_stats.py:212:11: FIX002 Line contains TODO
- tests/core/test_stats.py:212:11: T002 Line contains TODO
+ tests/jobs/test_scheduler_job.py:1059:7: FIX002 Line contains TODO
- tests/jobs/test_scheduler_job.py:1059:7: T002 Line contains TODO
+ tests/jobs/test_scheduler_job.py:2279:15: FIX002 Line contains TODO
- tests/jobs/test_scheduler_job.py:2279:15: T002 Line contains TODO
+ tests/jobs/test_scheduler_job.py:2340:11: FIX002 Line contains TODO
- tests/jobs/test_scheduler_job.py:2340:11: T002 Line contains TODO
+ tests/jobs/test_scheduler_job.py:4661:15: FIX002 Line contains TODO
- tests/jobs/test_scheduler_job.py:4661:15: T002 Line contains TODO
+ tests/jobs/test_triggerer_job_logging.py:430:7: FIX002 Line contains TODO
- tests/jobs/test_triggerer_job_logging.py:430:7: T002 Line contains TODO
+ tests/kubernetes/test_kubernetes_helper_functions.py:31:3: FIX002 Line contains TODO
- tests/kubernetes/test_kubernetes_helper_functions.py:31:3: T002 Line contains TODO
+ tests/providers/cncf/kubernetes/operators/test_pod.py:251:11: FIX002 Line contains TODO
- tests/providers/cncf/kubernetes/operators/test_pod.py:251:11: T002 Line contains TODO
+ tests/providers/elasticsearch/log/elasticmock/fake_elasticsearch.py:369:11: FIX002 Line contains TODO
- tests/providers/elasticsearch/log/elasticmock/fake_elasticsearch.py:369:11: T002 Line contains TODO
+ tests/providers/google/cloud/operators/test_cloud_memorystore.py:61:53: FIX002 Line contains TODO
- tests/providers/google/cloud/operators/test_cloud_memorystore.py:61:53: T002 Line contains TODO
+ tests/providers/microsoft/azure/hooks/test_azure_batch.py:149:11: FIX002 Line contains TODO
- tests/providers/microsoft/azure/hooks/test_azure_batch.py:149:11: T002 Line contains TODO
+ tests/providers/microsoft/azure/hooks/test_azure_batch.py:172:11: FIX002 Line contains TODO
- tests/providers/microsoft/azure/hooks/test_azure_batch.py:172:11: T002 Line contains TODO
+ tests/serialization/test_dag_serialization.py:309:52: FIX002 Line contains TODO
- tests/serialization/test_dag_serialization.py:309:52: T002 Line contains TODO
+ tests/serialization/test_dag_serialization.py:310:54: FIX002 Line contains TODO
- tests/serialization/test_dag_serialization.py:310:54: T002 Line contains TODO
+ tests/system/providers/snowflake/example_s3_to_snowflake.py:30:3: FIX002 Line contains TODO
- tests/system/providers/snowflake/example_s3_to_snowflake.py:30:3: T002 Line contains TODO
+ tests/utils/test_config.py:27:3: FIX002 Line contains TODO
- tests/utils/test_config.py:27:3: T002 Line contains TODO
+ tests/www/test_security.py:876:7: FIX002 Line contains TODO
- tests/www/test_security.py:876:7: T002 Line contains TODO
```

</p>
</details>
<details><summary>bokeh (+193, -193)</summary>
<p>

```diff
+ examples/integration/layout/plot_fixed_frame_size.py:38:3: FIX002 Line contains TODO
- examples/integration/layout/plot_fixed_frame_size.py:38:3: T002 Line contains TODO
+ examples/models/legends.py:53:11: FIX002 Line contains TODO
- examples/models/legends.py:53:11: T002 Line contains TODO
+ examples/server/app/simple_hdf5/main.py:50:3: FIX002 Line contains TODO
- examples/server/app/simple_hdf5/main.py:50:3: T002 Line contains TODO
+ release/credentials.py:77:7: FIX002 Line contains TODO
- release/credentials.py:77:7: T002 Line contains TODO
+ release/credentials.py:84:7: FIX002 Line contains TODO
- release/credentials.py:84:7: T002 Line contains TODO
+ src/bokeh/application/application.py:187:15: FIX002 Line contains TODO
- src/bokeh/application/application.py:187:15: T002 Line contains TODO
+ src/bokeh/application/handlers/code.py:168:11: FIX002 Line contains TODO
- src/bokeh/application/handlers/code.py:168:11: T002 Line contains TODO
+ src/bokeh/application/handlers/code_runner.py:225:11: FIX003 Line contains XXX
- src/bokeh/application/handlers/code_runner.py:225:11: T003 Line contains XXX
+ src/bokeh/application/handlers/directory.py:313:15: FIX002 Line contains TODO
- src/bokeh/application/handlers/directory.py:313:15: T002 Line contains TODO
+ src/bokeh/application/handlers/server_lifecycle.py:127:15: FIX002 Line contains TODO
- src/bokeh/application/handlers/server_lifecycle.py:127:15: T002 Line contains TODO
+ src/bokeh/application/handlers/server_request_handler.py:120:15: FIX002 Line contains TODO
- src/bokeh/application/handlers/server_request_handler.py:120:15: T002 Line contains TODO
+ src/bokeh/client/connection.py:349:19: FIX003 Line contains XXX
- src/bokeh/client/connection.py:349:19: T003 Line contains XXX
+ src/bokeh/client/session.py:513:11: FIX002 Line contains TODO
- src/bokeh/client/session.py:513:11: T002 Line contains TODO
+ src/bokeh/core/has_props.py:250:41: FIX002 Line contains TODO
- src/bokeh/core/has_props.py:250:41: T002 Line contains TODO
+ src/bokeh/core/has_props.py:292:15: FIX002 Line contains TODO
- src/bokeh/core/has_props.py:292:15: T002 Line contains TODO
+ src/bokeh/core/has_props.py:443:99: FIX002 Line contains TODO
- src/bokeh/core/has_props.py:443:99: T002 Line contains TODO
+ src/bokeh/core/has_props.py:633:15: FIX002 Line contains TODO
- src/bokeh/core/has_props.py:633:15: T002 Line contains TODO
+ src/bokeh/core/has_props.py:656:19: FIX002 Line contains TODO
- src/bokeh/core/has_props.py:656:19: T002 Line contains TODO
+ src/bokeh/core/has_props.py:749:17: FIX002 Line contains TODO
- src/bokeh/core/has_props.py:749:17: T002 Line contains TODO
+ src/bokeh/core/has_props.py:776:7: FIX002 Line contains TODO
- src/bokeh/core/has_props.py:776:7: T002 Line contains TODO
+ src/bokeh/core/has_props.py:789:7: FIX002 Line contains TODO
- src/bokeh/core/has_props.py:789:7: T002 Line contains TODO
+ src/bokeh/core/has_props.py:792:24: FIX002 Line contains TODO
- src/bokeh/core/has_props.py:792:24: T002 Line contains TODO
+ src/bokeh/core/json_encoder.py:186:43: FIX002 Line contains TODO
- src/bokeh/core/json_encoder.py:186:43: T002 Line contains TODO
+ src/bokeh/core/property/any.py:82:7: FIX002 Line contains TODO
- src/bokeh/core/property/any.py:82:7: T002 Line contains TODO
+ src/bokeh/core/property/container.py:124:11: FIX002 Line contains TODO
- src/bokeh/core/property/container.py:124:11: T002 Line contains TODO
+ src/bokeh/core/property/container.py:151:11: FIX002 Line contains TODO
- src/bokeh/core/property/container.py:151:11: T002 Line contains TODO
+ src/bokeh/core/property/dataspec.py:471:37: FIX003 Line contains XXX
- src/bokeh/core/property/dataspec.py:471:37: T003 Line contains XXX
+ src/bokeh/core/property/pd.py:30:7: FIX003 Line contains XXX
- src/bokeh/core/property/pd.py:30:7: T003 Line contains XXX
+ src/bokeh/core/property/struct.py:70:87: FIX003 Line contains XXX
- src/bokeh/core/property/struct.py:70:87: T003 Line contains XXX
+ src/bokeh/core/property/wrappers.py:466:11: FIX002 Line contains TODO
- src/bokeh/core/property/wrappers.py:466:11: T002 Line contains TODO
+ src/bokeh/core/property_mixins.py:374:41: FIX003 Line contains XXX
- src/bokeh/core/property_mixins.py:374:41: T003 Line contains XXX
+ src/bokeh/core/serialization.py:578:62: FIX002 Line contains TODO
- src/bokeh/core/serialization.py:578:62: T002 Line contains TODO
+ src/bokeh/core/serialization.py:717:33: FIX003 Line contains XXX
- src/bokeh/core/serialization.py:717:33: T003 Line contains XXX
+ src/bokeh/core/types.py:58:3: FIX002 Line contains TODO
- src/bokeh/core/types.py:58:3: T002 Line contains TODO
+ src/bokeh/document/callbacks.py:259:11: FIX002 Line contains TODO
- src/bokeh/document/callbacks.py:259:11: T002 Line contains TODO
+ src/bokeh/document/document.py:390:15: FIX002 Line contains TODO
- src/bokeh/document/document.py:390:15: T002 Line contains TODO
+ src/bokeh/document/document.py:418:11: FIX002 Line contains TODO
- src/bokeh/document/document.py:418:11: T002 Line contains TODO
+ src/bokeh/document/document.py:433:11: FIX002 Line contains TODO
- src/bokeh/document/document.py:433:11: T002 Line contains TODO
+ src/bokeh/document/events.py:124:47: FIX002 Line contains TODO
- src/bokeh/document/events.py:124:47: T002 Line contains TODO
+ src/bokeh/document/json.py:53:26: FIX002 Line contains TODO
- src/bokeh/document/json.py:53:26: T002 Line contains TODO
+ src/bokeh/document/locking.py:111:32: FIX002 Line contains TODO
- src/bokeh/document/locking.py:111:32: T002 Line contains TODO
+ src/bokeh/document/models.py:241:7: FIX003 Line contains XXX
- src/bokeh/document/models.py:241:7: T003 Line contains XXX
+ src/bokeh/embed/bundle.py:170:11: FIX003 Line contains XXX
- src/bokeh/embed/bundle.py:170:11: T003 Line contains XXX
+ src/bokeh/embed/elements.py:137:7: FIX003 Line contains XXX
- src/bokeh/embed/elements.py:137:7: T003 Line contains XXX
+ src/bokeh/embed/elements.py:168:11: FIX003 Line contains XXX
- src/bokeh/embed/elements.py:168:11: T003 Line contains XXX
+ src/bokeh/embed/standalone.py:108:7: FIX002 Line contains TODO
- src/bokeh/embed/standalone.py:108:7: T002 Line contains TODO
+ src/bokeh/embed/standalone.py:137:79: FIX003 Line contains XXX
- src/bokeh/embed/standalone.py:137:79: T003 Line contains XXX
+ src/bokeh/embed/standalone.py:144:89: FIX003 Line contains XXX
- src/bokeh/embed/standalone.py:144:89: T003 Line contains XXX
+ src/bokeh/embed/standalone.py:151:90: FIX003 Line contains XXX
- src/bokeh/embed/standalone.py:151:90: T003 Line contains XXX
+ src/bokeh/embed/standalone.py:248:7: FIX003 Line contains XXX
- src/bokeh/embed/standalone.py:248:7: T003 Line contains XXX
+ src/bokeh/embed/standalone.py:255:61: FIX003 Line contains XXX
- src/bokeh/embed/standalone.py:255:61: T003 Line contains XXX
+ src/bokeh/embed/util.py:401:7: FIX002 Line contains TODO
- src/bokeh/embed/util.py:401:7: T002 Line contains TODO
+ src/bokeh/io/export.py:123:38: FIX003 Line contains XXX
- src/bokeh/io/export.py:123:38: T003 Line contains XXX
+ src/bokeh/io/export.py:434:17: FIX003 Line contains XXX
- src/bokeh/io/export.py:434:17: T003 Line contains XXX
+ src/bokeh/io/export.py:438:3: FIX002 Line contains TODO
- src/bokeh/io/export.py:438:3: T002 Line contains TODO
+ src/bokeh/io/export.py:495:11: FIX003 Line contains XXX
- src/bokeh/io/export.py:495:11: T003 Line contains XXX
+ src/bokeh/io/notebook.py:330:93: FIX003 Line contains XXX
- src/bokeh/io/notebook.py:330:93: T003 Line contains XXX
+ src/bokeh/io/showing.py:148:48: FIX002 Line contains TODO
- src/bokeh/io/showing.py:148:48: T002 Line contains TODO
+ src/bokeh/io/util.py:110:7: FIX002 Line contains TODO
- src/bokeh/io/util.py:110:7: T002 Line contains TODO
+ src/bokeh/io/util.py:129:7: FIX003 Line contains XXX
- src/bokeh/io/util.py:129:7: T003 Line contains XXX
+ src/bokeh/layouts.py:194:40: FIX002 Line contains TODO
- src/bokeh/layouts.py:194:40: T002 Line contains TODO
+ src/bokeh/layouts.py:303:3: FIX003 Line contains XXX
- src/bokeh/layouts.py:303:3: T003 Line contains XXX
+ src/bokeh/model/util.py:40:20: FIX003 Line contains XXX
- src/bokeh/model/util.py:40:20: T003 Line contains XXX
+ src/bokeh/models/annotations/arrows.py:71:7: FIX002 Line contains TODO
- src/bokeh/models/annotations/arrows.py:71:7: T002 Line contains TODO
+ src/bokeh/models/annotations/html/toolbars.py:38:39: FIX002 Line contains TODO
- src/bokeh/models/annotations/html/toolbars.py:38:39: T002 Line contains TODO
+ src/bokeh/models/callbacks.py:224:3: FIX002 Line contains TODO
- src/bokeh/models/callbacks.py:224:3: T002 Line contains TODO
+ src/bokeh/models/callbacks.py:225:3: FIX002 Line contains TODO
- src/bokeh/models/callbacks.py:225:3: T002 Line contains TODO
+ src/bokeh/models/graphs.py:85:7: FIX002 Line contains TODO
- src/bokeh/models/graphs.py:85:7: T002 Line contains TODO
+ src/bokeh/models/map_plots.py:172:7: FIX002 Line contains TODO
- src/bokeh/models/map_plots.py:172:7: T002 Line contains TODO
+ src/bokeh/models/renderers/graph_renderer.py:44:3: FIX002 Line contains TODO
- src/bokeh/models/renderers/graph_renderer.py:44:3: T002 Line contains TODO
+ src/bokeh/models/sources.py:224:11: FIX002 Line contains TODO
- src/bokeh/models/sources.py:224:11: T002 Line contains TODO
+ src/bokeh/models/tickers.py:322:7: FIX002 Line contains TODO
- src/bokeh/models/tickers.py:322:7: T002 Line contains TODO
+ src/bokeh/models/tools.py:884:7: FIX002 Line contains TODO
- src/bokeh/models/tools.py:884:7: T002 Line contains TODO
+ src/bokeh/models/ui/dialogs.py:48:22: FIX002 Line contains TODO
- src/bokeh/models/ui/dialogs.py:48:22: T002 Line contains TODO
+ src/bokeh/plotting/_docstring.py:107:7: FIX003 Line contains XXX
- src/bokeh/plotting/_docstring.py:107:7: T003 Line contains XXX
+ src/bokeh/plotting/_renderer.py:283:28: FIX002 Line contains TODO
- src/bokeh/plotting/_renderer.py:283:28: T002 Line contains TODO
+ src/bokeh/plotting/_renderer.py:287:28: FIX002 Line contains TODO
- src/bokeh/plotting/_renderer.py:287:28: T002 Line contains TODO
+ src/bokeh/plotting/_tools.py:71:3: FIX002 Line contains TODO
- src/bokeh/plotting/_tools.py:71:3: T002 Line contains TODO
+ src/bokeh/resources.py:247:3: FIX003 Line contains XXX
- src/bokeh/resources.py:247:3: T003 Line contains XXX
+ src/bokeh/resources.py:342:40: FIX002 Line contains TODO
- src/bokeh/resources.py:342:40: T002 Line contains TODO
+ src/bokeh/resources.py:633:36: FIX002 Line contains TODO
- src/bokeh/resources.py:633:36: T002 Line contains TODO
+ src/bokeh/server/session.py:246:11: FIX002 Line contains TODO
- src/bokeh/server/session.py:246:11: T002 Line contains TODO
+ src/bokeh/server/tornado.py:397:51: FIX002 Line contains TODO
- src/bokeh/server/tornado.py:397:51: T002 Line contains TODO
+ src/bokeh/server/tornado.py:431:38: FIX002 Line contains TODO
- src/bokeh/server/tornado.py:431:38: T002 Line contains TODO
+ src/bokeh/server/tornado.py:432:86: FIX002 Line contains TODO
- src/bokeh/server/tornado.py:432:86: T002 Line contains TODO
+ src/bokeh/server/tornado.py:449:82: FIX002 Line contains TODO
- src/bokeh/server/tornado.py:449:82: T002 Line contains TODO
+ src/bokeh/server/tornado.py:452:78: FIX002 Line contains TODO
- src/bokeh/server/tornado.py:452:78: T002 Line contains TODO
+ src/bokeh/server/tornado.py:458:67: FIX002 Line contains TODO
- src/bokeh/server/tornado.py:458:67: T002 Line contains TODO
+ src/bokeh/server/tornado.py:653:11: FIX002 Line contains TODO
- src/bokeh/server/tornado.py:653:11: T002 Line contains TODO
+ src/bokeh/server/views/ws.py:325:71: FIX002 Line contains TODO
- src/bokeh/server/views/ws.py:325:71: T002 Line contains TODO
+ src/bokeh/settings.py:518:36: FIX002 Line contains TODO
- src/bokeh/settings.py:518:36: T002 Line contains TODO
+ src/bokeh/sphinxext/util.py:47:3: FIX002 Line contains TODO
- src/bokeh/sphinxext/util.py:47:3: T002 Line contains TODO
+ src/bokeh/util/callback_manager.py:61:3: FIX002 Line contains TODO
- src/bokeh/util/callback_manager.py:61:3: T002 Line contains TODO
+ src/bokeh/util/compiler.py:338:3: FIX003 Line contains XXX
- src/bokeh/util/compiler.py:338:3: T003 Line contains XXX
+ src/bokeh/util/compiler.py:512:26: FIX002 Line contains TODO
- src/bokeh/util/compiler.py:512:26: T002 Line contains TODO
+ src/bokeh/util/datatypes.py:73:49: FIX003 Line contains XXX
- src/bokeh/util/datatypes.py:73:49: T003 Line contains XXX
+ src/bokeh/util/logconfig.py:58:3: FIX002 Line contains TODO
- src/bokeh/util/logconfig.py:58:3: T002 Line contains TODO
+ src/bokeh/util/serialization.py:202:7: FIX003 Line contains XXX
- src/bokeh/util/serialization.py:202:7: T003 Line contains XXX
+ src/bokeh/util/serialization.py:239:7: FIX003 Line contains XXX
- src/bokeh/util/serialization.py:239:7: T003 Line contains XXX
+ src/bokeh/util/serialization.py:339:7: FIX003 Line contains XXX
- src/bokeh/util/serialization.py:339:7: T003 Line contains XXX
+ src/bokeh/util/token.py:295:7: FIX002 Line contains TODO
- src/bokeh/util/token.py:295:7: T002 Line contains TODO
+ src/typings/IPython/core/history.pyi:5:89: FIX003 Line contains XXX
- src/typings/IPython/core/history.pyi:5:89: T003 Line contains XXX
+ src/typings/bs4.pyi:6:3: FIX003 Line contains XXX
- src/typings/bs4.pyi:6:3: T003 Line contains XXX
+ tests/integration/models/test_plot.py:44:3: FIX002 Line contains TODO
- tests/integration/models/test_plot.py:44:3: T002 Line contains TODO
+ tests/integration/tools/test_range_tool.py:36:3: FIX002 Line contains TODO
- tests/integration/tools/test_range_tool.py:36:3: T002 Line contains TODO
+ tests/integration/tools/test_range_tool.py:37:3: FIX002 Line contains TODO
- tests/integration/tools/test_range_tool.py:37:3: T002 Line contains TODO
+ tests/integration/tools/test_reset_tool.py:104:11: FIX003 Line contains XXX
- tests/integration/tools/test_reset_tool.py:104:11: T003 Line contains XXX
+ tests/integration/tools/test_reset_tool.py:125:61: FIX003 Line contains XXX
- tests/integration/tools/test_reset_tool.py:125:61: T003 Line contains XXX
+ tests/integration/tools/test_tap_tool.py:34:3: FIX002 Line contains TODO
- tests/integration/tools/test_tap_tool.py:34:3: T002 Line contains TODO
+ tests/integration/widgets/tables/test_cell_editors.py:101:13: FIX002 Line contains TODO
- tests/integration/widgets/tables/test_cell_editors.py:101:13: T002 Line contains TODO
+ tests/integration/widgets/tables/test_cell_editors.py:286:7: FIX003 Line contains XXX
- tests/integration/widgets/tables/test_cell_editors.py:286:7: T003 Line contains XXX
+ tests/integration/widgets/tables/test_cell_editors.py:351:3: FIX003 Line contains XXX
- tests/integration/widgets/tables/test_cell_editors.py:351:3: T003 Line contains XXX
+ tests/integration/widgets/tables/test_cell_editors.py:59:3: FIX003 Line contains XXX
- tests/integration/widgets/tables/test_cell_editors.py:59:3: T003 Line contains XXX
+ tests/integration/widgets/tables/test_cell_editors.py:79:13: FIX002 Line contains TODO
- tests/integration/widgets/tables/test_cell_editors.py:79:13: T002 Line contains TODO
+ tests/integration/widgets/tables/test_copy_paste.py:143:11: FIX003 Line contains XXX
- tests/integration/widgets/tables/test_copy_paste.py:143:11: T003 Line contains XXX
+ tests/integration/widgets/test_color_picker.py:107:11: FIX003 Line contains XXX
- tests/integration/widgets/test_color_picker.py:107:11: T003 Line contains XXX
+ tests/integration/widgets/test_daterange_slider.py:160:13: FIX003 Line contains XXX
- tests/integration/widgets/test_daterange_slider.py:160:13: T003 Line contains XXX
+ tests/integration/widgets/test_daterange_slider.py:169:13: FIX003 Line contains XXX
- tests/integration/widgets/test_daterange_slider.py:169:13: T003 Line contains XXX
+ tests/integration/widgets/test_daterange_slider.py:192:11: FIX003 Line contains XXX
- tests/integration/widgets/test_daterange_slider.py:192:11: T003 Line contains XXX
+ tests/integration/widgets/test_dateslider.py:154:11: FIX003 Line contains XXX
- tests/integration/widgets/test_dateslider.py:154:11: T003 Line contains XXX
+ tests/integration/widgets/test_dateslider.py:163:11: FIX003 Line contains XXX
- tests/integration/widgets/test_dateslider.py:163:11: T003 Line contains XXX
+ tests/integration/widgets/test_dateslider.py:197:11: FIX003 Line contains XXX
- tests/integration/widgets/test_dateslider.py:197:11: T003 Line contains XXX
+ tests/integration/widgets/test_dateslider.py:220:11: FIX003 Line contains XXX
- tests/integration/widgets/test_dateslider.py:220:11: T003 Line contains XXX
+ tests/integration/widgets/test_dateslider.py:243:11: FIX003 Line contains XXX
- tests/integration/widgets/test_dateslider.py:243:11: T003 Line contains XXX
+ tests/integration/widgets/test_dropdown.py:42:3: FIX003 Line contains XXX
- tests/integration/widgets/test_dropdown.py:42:3: T003 Line contains XXX
+ tests/integration/widgets/test_range_slider.py:211:11: FIX003 Line contains XXX
- tests/integration/widgets/test_range_slider.py:211:11: T003 Line contains XXX
+ tests/integration/widgets/test_range_slider.py:220:11: FIX003 Line contains XXX
- tests/integration/widgets/test_range_slider.py:220:11: T003 Line contains XXX
+ tests/integration/widgets/test_range_slider.py:243:11: FIX003 Line contains XXX
- tests/integration/widgets/test_range_slider.py:243:11: T003 Line contains XXX
+ tests/integration/widgets/test_slider.py:177:11: FIX003 Line contains XXX
- tests/integration/widgets/test_slider.py:177:11: T003 Line contains XXX
+ tests/integration/widgets/test_slider.py:186:11: FIX003 Line contains XXX
- tests/integration/widgets/test_slider.py:186:11: T003 Line contains XXX
+ tests/integration/widgets/test_slider.py:220:11: FIX003 Line contains XXX
- tests/integration/widgets/test_slider.py:220:11: T003 Line contains XXX
+ tests/integration/widgets/test_slider.py:243:11: FIX003 Line contains XXX
- tests/integration/widgets/test_slider.py:243:11: T003 Line contains XXX
+ tests/integration/widgets/test_slider.py:266:11: FIX003 Line contains XXX
- tests/integration/widgets/test_slider.py:266:11: T003 Line contains XXX
+ tests/integration/widgets/test_spinner.py:228:11: FIX003 Line contains XXX
- tests/integration/widgets/test_spinner.py:228:11: T003 Line contains XXX
+ tests/integration/widgets/test_toggle.py:105:7: FIX003 Line contains XXX
- tests/integration/widgets/test_toggle.py:105:7: T003 Line contains XXX
+ tests/support/plugins/bokeh_server.py:103:66: FIX003 Line contains XXX
- tests/support/plugins/bokeh_server.py:103:66: T003 Line contains XXX
+ tests/support/plugins/bokeh_server.py:86:48: FIX003 Line contains XXX
- tests/support/plugins/bokeh_server.py:86:48: T003 Line contains XXX
+ tests/support/plugins/file_server.py:64:42: FIX002 Line contains TODO
- tests/support/plugins/file_server.py:64:42: T002 Line contains TODO
+ tests/support/plugins/ipython.py:45:39: FIX003 Line contains XXX
- tests/support/plugins/ipython.py:45:39: T003 Line contains XXX
+ tests/support/plugins/networkx.py:45:34: FIX003 Line contains XXX
- tests/support/plugins/networkx.py:45:34: T003 Line contains XXX
+ tests/support/plugins/project.py:162:15: FIX003 Line contains XXX
- tests/support/plugins/project.py:162:15: T003 Line contains XXX
+ tests/support/plugins/selenium.py:130:15: FIX003 Line contains XXX
- tests/support/plugins/selenium.py:130:15: T003 Line contains XXX
+ tests/support/util/compare.py:55:37: FIX003 Line contains XXX
- tests/support/util/compare.py:55:37: T003 Line contains XXX
+ tests/support/util/selenium.py:320:30: FIX003 Line contains XXX
- tests/support/util/selenium.py:320:30: T003 Line contains XXX
+ tests/unit/bokeh/core/property/test_aliases.py:78:3: FIX002 Line contains TODO
- tests/unit/bokeh/core/property/test_aliases.py:78:3: T002 Line contains TODO
+ tests/unit/bokeh/core/property/test_bases.py:156:11: FIX003 Line contains XXX
- tests/unit/bokeh/core/property/test_bases.py:156:11: T003 Line contains XXX
+ tests/unit/bokeh/core/property/test_bases.py:188:11: FIX003 Line contains XXX
- tests/unit/bokeh/core/property/test_bases.py:188:11: T003 Line contains XXX
+ tests/unit/bokeh/core/property/test_bases.py:205:11: FIX003 Line contains XXX
- tests/unit/bokeh/core/property/test_bases.py:205:11: T003 Line contains XXX
+ tests/unit/bokeh/core/property/test_container.py:62:3: FIX002 Line contains TODO
- tests/unit/bokeh/core/property/test_container.py:62:3: T002 Line contains TODO
+ tests/unit/bokeh/core/property/test_datetime.py:170:3: FIX002 Line contains TODO
- tests/unit/bokeh/core/property/test_datetime.py:170:3: T002 Line contains TODO
+ tests/unit/bokeh/core/property/test_either.py:66:11: FIX002 Line contains TODO
- tests/unit/bokeh/core/property/test_either.py:66:11: T002 Line contains TODO
+ tests/unit/bokeh/core/property/test_numeric.py:122:11: FIX002 Line contains TODO
- tests/unit/bokeh/core/property/test_numeric.py:122:11: T002 Line contains TODO
+ tests/unit/bokeh/core/property/test_numeric.py:160:11: FIX002 Line contains TODO
- tests/unit/bokeh/core/property/test_numeric.py:160:11: T002 Line contains TODO
+ tests/unit/bokeh/core/property/test_numeric.py:199:11: FIX002 Line contains TODO
- tests/unit/bokeh/core/property/test_numeric.py:199:11: T002 Line contains TODO
+ tests/unit/bokeh/core/property/test_numeric.py:237:11: FIX002 Line contains TODO
- tests/unit/bokeh/core/property/test_numeric.py:237:11: T002 Line contains TODO
+ tests/unit/bokeh/core/property/test_numeric.py:286:11: FIX002 Line contains TODO
- tests/unit/bokeh/core/property/test_numeric.py:286:11: T002 Line contains TODO
+ tests/unit/bokeh/core/property/test_numeric.py:53:11: FIX002 Line contains TODO
- tests/unit/bokeh/core/property/test_numeric.py:53:11: T002 Line contains TODO
+ tests/unit/bokeh/core/property/test_numeric.py:94:11: FIX002 Line contains TODO
- tests/unit/bokeh/core/property/test_numeric.py:94:11: T002 Line contains TODO
+ tests/unit/bokeh/core/property/test_primitive.py:155:11: FIX002 Line contains TODO
- tests/unit/bokeh/core/property/test_primitive.py:155:11: T002 Line contains TODO
+ tests/unit/bokeh/core/property/test_primitive.py:214:11: FIX002 Line contains TODO
- tests/unit/bokeh/core/property/test_primitive.py:214:11: T002 Line contains TODO
+ tests/unit/bokeh/core/property/test_primitive.py:344:11: FIX002 Line contains TODO
- tests/unit/bokeh/core/property/test_primitive.py:344:11: T002 Line contains TODO
+ tests/unit/bokeh/core/property/test_validation__property.py:120:7: FIX002 Line contains TODO
- tests/unit/bokeh/core/property/test_validation__property.py:120:7: T002 Line contains TODO
+ tests/unit/bokeh/core/property/test_validation__property.py:264:7: FIX002 Line contains TODO
- tests/unit/bokeh/core/property/test_validation__property.py:264:7: T002 Line contains TODO
+ tests/unit/bokeh/core/test_properties.py:152:3: FIX002 Line contains TODO
- tests/unit/bokeh/core/test_properties.py:152:3: T002 Line contains TODO
+ tests/unit/bokeh/core/test_properties.py:285:62: FIX003 Line contains XXX
- tests/unit/bokeh/core/test_properties.py:285:62: T003 Line contains XXX
+ tests/unit/bokeh/core/test_properties.py:299:62: FIX003 Line contains XXX
- tests/unit/bokeh/core/test_properties.py:299:62: T003 Line contains XXX
+ tests/unit/bokeh/document/test_callbacks__document.py:179:52: FIX003 Line contains XXX
- tests/unit/bokeh/document/test_callbacks__document.py:179:52: T003 Line contains XXX
+ tests/unit/bokeh/document/test_callbacks__document.py:378:7: FIX002 Line contains TODO
- tests/unit/bokeh/document/test_callbacks__document.py:378:7: T002 Line contains TODO
+ tests/unit/bokeh/document/test_callbacks__document.py:379:7: FIX002 Line contains TODO
- tests/unit/bokeh/document/test_callbacks__document.py:379:7: T002 Line contains TODO
+ tests/unit/bokeh/document/test_document.py:1072:7: FIX002 Line contains TODO
- tests/unit/bokeh/document/test_document.py:1072:7: T002 Line contains TODO
+ tests/unit/bokeh/document/test_document.py:1074:7: FIX002 Line contains TODO
- tests/unit/bokeh/document/test_document.py:1074:7: T002 Line contains TODO
+ tests/unit/bokeh/document/test_document.py:794:11: FIX002 Line contains TODO
- tests/unit/bokeh/document/test_document.py:794:11: T002 Line contains TODO
+ tests/unit/bokeh/document/test_events__document.py:159:7: FIX002 Line contains TODO
- tests/unit/bokeh/document/test_events__document.py:159:7: T002 Line contains TODO
+ tests/unit/bokeh/embed/test_standalone.py:282:11: FIX003 Line contains XXX
- tests/unit/bokeh/embed/test_standalone.py:282:11: T003 Line contains XXX
+ tests/unit/bokeh/io/test_saving.py:63:41: FIX002 Line contains TODO
- tests/unit/bokeh/io/test_saving.py:63:41: T002 Line contains TODO
+ tests/unit/bokeh/model/test_util_model.py:127:7: FIX002 Line contains TODO
- tests/unit/bokeh/model/test_util_model.py:127:7: T002 Line contains TODO
+ tests/unit/bokeh/model/test_util_model.py:61:3: FIX002 Line contains TODO
- tests/unit/bokeh/model/test_util_model.py:61:3: T002 Line contains TODO
+ tests/unit/bokeh/models/test_sources.py:748:11: FIX002 Line contains TODO
- tests/unit/bokeh/models/test_sources.py:748:11: T002 Line contains TODO
+ tests/unit/bokeh/models/test_tools.py:7:3: FIX002 Line contains TODO
- tests/unit/bokeh/models/test_tools.py:7:3: T002 Line contains TODO
+ tests/unit/bokeh/plotting/test__decorators.py:43:3: FIX002 Line contains TODO
- tests/unit/bokeh/plotting/test__decorators.py:43:3: T002 Line contains TODO
+ tests/unit/bokeh/plotting/test__renderer.py:115:5: FIX002 Line contains TODO
- tests/unit/bokeh/plotting/test__renderer.py:115:5: T002 Line contains TODO
+ tests/unit/bokeh/plotting/test_figure.py:536:7: FIX003 Line contains XXX
- tests/unit/bokeh/plotting/test_figure.py:536:7: T003 Line contains XXX
+ tests/unit/bokeh/plotting/test_figure.py:61:59: FIX002 Line contains TODO
- tests/unit/bokeh/plotting/test_figure.py:61:59: T002 Line contains TODO
+ tests/unit/bokeh/server/views/test_multi_root_static_handler.py:45:50: FIX002 Line contains TODO
- tests/unit/bokeh/server/views/test_multi_root_static_handler.py:45:50: T002 Line contains TODO
+ tests/unit/bokeh/server/views/test_ws.py:47:7: FIX002 Line contains TODO
- tests/unit/bokeh/server/views/test_ws.py:47:7: T002 Line contains TODO
+ tests/unit/bokeh/server/views/test_ws.py:52:11: FIX002 Line contains TODO
- tests/unit/bokeh/server/views/test_ws.py:52:11: T002 Line contains TODO
+ tests/unit/bokeh/server/views/test_ws.py:53:11: FIX002 Line contains TODO
- tests/unit/bokeh/server/views/test_ws.py:53:11: T002 Line contains TODO
+ tests/unit/bokeh/server/views/test_ws.py:54:11: FIX003 Line contains XXX
- tests/unit/bokeh/server/views/test_ws.py:54:11: T003 Line contains XXX
+ tests/unit/bokeh/test___init__.py:101:7: FIX002 Line contains TODO
- tests/unit/bokeh/test___init__.py:101:7: T002 Line contains TODO
+ tests/unit/bokeh/test_client_server.py:670:58: FIX003 Line contains XXX
- tests/unit/bokeh/test_client_server.py:670:58: T003 Line contains XXX
+ tests/unit/bokeh/test_objects.py:238:11: FIX003 Line contains XXX
- tests/unit/bokeh/test_objects.py:238:11: T003 Line contains XXX
+ tests/unit/bokeh/test_resources.py:81:7: FIX002 Line contains TODO
- tests/unit/bokeh/test_resources.py:81:7: T002 Line contains TODO
+ tests/unit/bokeh/test_tile_providers.py:79:3: FIX003 Line contains XXX
- tests/unit/bokeh/test_tile_providers.py:79:3: T003 Line contains XXX
```

</p>
</details>
<details><summary>zulip (+204, -204)</summary>
<p>

```diff
+ analytics/lib/counts.py:111:7: FIX002 Line contains TODO
- analytics/lib/counts.py:111:7: T002 Line contains TODO
+ analytics/lib/counts.py:255:11: FIX002 Line contains TODO
- analytics/lib/counts.py:255:11: T002 Line contains TODO
+ analytics/management/commands/populate_analytics_db.py:71:11: FIX002 Line contains TODO
- analytics/management/commands/populate_analytics_db.py:71:11: T002 Line contains TODO
+ analytics/views/stats.py:108:11: FIX002 Line contains TODO
- analytics/views/stats.py:108:11: T002 Line contains TODO
+ analytics/views/stats.py:413:11: FIX002 Line contains TODO
- analytics/views/stats.py:413:11: T002 Line contains TODO
+ analytics/views/stats.py:538:11: FIX004 Line contains HACK
- analytics/views/stats.py:538:11: T004 Line contains HACK
+ corporate/lib/stripe.py:1069:11: FIX002 Line contains TODO
- corporate/lib/stripe.py:1069:11: T002 Line contains TODO
+ corporate/lib/stripe.py:1074:11: FIX002 Line contains TODO
- corporate/lib/stripe.py:1074:11: T002 Line contains TODO
+ corporate/lib/stripe.py:286:19: FIX002 Line contains TODO
- corporate/lib/stripe.py:286:19: T002 Line contains TODO
+ corporate/lib/stripe.py:603:7: FIX002 Line contains TODO
- corporate/lib/stripe.py:603:7: T002 Line contains TODO
+ corporate/lib/stripe.py:707:7: FIX002 Line contains TODO
- corporate/lib/stripe.py:707:7: T002 Line contains TODO
+ corporate/models.py:258:7: FIX002 Line contains TODO
- corporate/models.py:258:7: T002 Line contains TODO
+ corporate/tests/test_stripe.py:1706:11: FIX002 Line contains TODO
- corporate/tests/test_stripe.py:1706:11: T002 Line contains TODO
+ corporate/tests/test_stripe.py:2482:11: FIX002 Line contains TODO
- corporate/tests/test_stripe.py:2482:11: T002 Line contains TODO
+ corporate/tests/test_stripe.py:4299:11: FIX002 Line contains TODO
- corporate/tests/test_stripe.py:4299:11: T002 Line contains TODO
+ corporate/tests/test_stripe.py:4599:11: FIX002 Line contains TODO
- corporate/tests/test_stripe.py:4599:11: T002 Line contains TODO
+ corporate/tests/test_stripe.py:742:11: FIX002 Line contains TODO
- corporate/tests/test_stripe.py:742:11: T002 Line contains TODO
+ scripts/lib/check_rabbitmq_queue.py:65:11: FIX002 Line contains TODO
- scripts/lib/check_rabbitmq_queue.py:65:11: T002 Line contains TODO
+ scripts/lib/clean_node_cache.py:3:3: FIX002 Line contains TODO
- scripts/lib/clean_node_cache.py:3:3: T002 Line contains TODO
+ scripts/lib/setup_venv.py:199:15: FIX002 Line contains TODO
- scripts/lib/setup_venv.py:199:15: T002 Line contains TODO
+ scripts/lib/sharding.py:29:3: FIX002 Line contains TODO
- scripts/lib/sharding.py:29:3: T002 Line contains TODO
+ tools/documentation_crawler/documentation_crawler/spiders/common/spiders.py:109:15: FIX002 Line contains TODO
- tools/documentation_crawler/documentation_crawler/spiders/common/spiders.py:109:15: T002 Line contains TODO
+ tools/documentation_crawler/documentation_crawler/spiders/common/spiders.py:237:15: FIX004 Line contains HACK
- tools/documentation_crawler/documentation_crawler/spiders/common/spiders.py:237:15: T004 Line contains HACK
+ tools/lib/capitalization.py:226:15: FIX004 Line contains HACK
- tools/lib/capitalization.py:226:15: T004 Line contains HACK
+ tools/lib/provision.py:280:7: FIX004 Line contains HACK
- tools/lib/provision.py:280:7: T004 Line contains HACK
+ tools/lib/provision.py:290:15: FIX002 Line contains TODO
- tools/lib/provision.py:290:15: T002 Line contains TODO
+ tools/lib/template_parser.py:457:15: FIX002 Line contains TODO
- tools/lib/template_parser.py:457:15: T002 Line contains TODO
+ zerver/actions/create_realm.py:193:11: FIX004 Line contains HACK
- zerver/actions/create_realm.py:193:11: T004 Line contains HACK
+ zerver/actions/invites.py:455:7: FIX002 Line contains TODO
- zerver/actions/invites.py:455:7: T002 Line contains TODO
+ zerver/actions/message_edit.py:1010:15: FIX002 Line contains TODO
- zerver/actions/message_edit.py:1010:15: T002 Line contains TODO
+ zerver/actions/message_edit.py:731:15: FIX002 Line contains TODO
- zerver/actions/message_edit.py:731:15: T002 Line contains TODO
+ zerver/actions/message_send.py:307:11: FIX002 Line contains TODO
- zerver/actions/message_send.py:307:11: T002 Line contains TODO
+ zerver/actions/reactions.py:28:11: FIX002 Line contains TODO
- zerver/actions/reactions.py:28:11: T002 Line contains TODO
+ zerver/actions/user_settings.py:416:7: FIX002 Line contains TODO
- zerver/actions/user_settings.py:416:7: T002 Line contains TODO
+ zerver/actions/user_settings.py:543:15: FIX004 Line contains HACK
- zerver/actions/user_settings.py:543:15: T004 Line contains HACK
+ zerver/actions/users.py:388:7: FIX002 Line contains TODO
- zerver/actions/users.py:388:7: T002 Line contains TODO
+ zerver/data_import/gitter.py:132:7: FIX002 Line contains TODO
- zerver/data_import/gitter.py:132:7: T002 Line contains TODO
+ zerver/data_import/gitter.py:206:7: FIX002 Line contains TODO
- zerver/data_import/gitter.py:206:7: T002 Line contains TODO
+ zerver/data_import/rocketchat.py:1254:7: FIX002 Line contains TODO
- zerver/data_import/rocketchat.py:1254:7: T002 Line contains TODO
+ zerver/data_import/rocketchat.py:91:11: FIX002 Line contains TODO
- zerver/data_import/rocketchat.py:91:11: T002 Line contains TODO
+ zerver/data_import/slack.py:571:15: FIX002 Line contains TODO
- zerver/data_import/slack.py:571:15: T002 Line contains TODO
+ zerver/forms.py:377:15: FIX002 Line contains TODO
- zerver/forms.py:377:15: T002 Line contains TODO
+ zerver/lib/alert_words.py:76:7: FIX002 Line contains TODO
- zerver/lib/alert_words.py:76:7: T002 Line contains TODO
+ zerver/lib/ccache.py:194:7: FIX002 Line contains TODO
- zerver/lib/ccache.py:194:7: T002 Line contains TODO
+ zerver/lib/compatibility.py:150:7: FIX002 Line contains TODO
- zerver/lib/compatibility.py:150:7: T002 Line contains TODO
+ zerver/lib/email_mirror.py:485:11: FIX002 Line contains TODO
- zerver/lib/email_mirror.py:485:11: T002 Line contains TODO
+ zerver/lib/email_notifications.py:610:11: FIX002 Line contains TODO
- zerver/lib/email_notifications.py:610:11: T002 Line contains TODO
+ zerver/lib/email_notifications.py:833:7: FIX002 Line contains TODO
- zerver/lib/email_notifications.py:833:7: T002 Line contains TODO
+ zerver/lib/email_validation.py:10:3: FIX002 Line contains TODO
- zerver/lib/email_validation.py:10:3: T002 Line contains TODO
+ zerver/lib/event_schema.py:1041:3: FIX002 Line contains TODO
- zerver/lib/event_schema.py:1041:3: T002 Line contains TODO
+ zerver/lib/event_schema.py:1359:3: FIX002 Line contains TODO
- zerver/lib/event_schema.py:1359:3: T002 Line contains TODO
+ zerver/lib/events.py:1004:23: FIX002 Line contains TODO
- zerver/lib/events.py:1004:23: T002 Line contains TODO
+ zerver/lib/events.py:1489:11: FIX002 Line contains TODO
- zerver/lib/events.py:1489:11: T002 Line contains TODO
+ zerver/lib/events.py:334:11: FIX002 Line contains TODO
- zerver/lib/events.py:334:11: T002 Line contains TODO
+ zerver/lib/events.py:466:11: FIX002 Line contains TODO
- zerver/lib/events.py:466:11: T002 Line contains TODO
+ zerver/lib/events.py:571:15: FIX002 Line contains TODO
- zerver/lib/events.py:571:15: T002 Line contains TODO
+ zerver/lib/events.py:668:15: FIX002 Line contains TODO
- zerver/lib/events.py:668:15: T002 Line contains TODO
+ zerver/lib/events.py:877:23: FIX002 Line contains TODO
- zerver/lib/events.py:877:23: T002 Line contains TODO
+ zerver/lib/export.py:1255:7: FIX002 Line contains TODO
- zerver/lib/export.py:1255:7: T002 Line contains TODO
+ zerver/lib/export.py:2127:7: FIX002 Line contains TODO
- zerver/lib/export.py:2127:7: T002 Line contains TODO
+ zerver/lib/export.py:282:3: FIX002 Line contains TODO
- zerver/lib/export.py:282:3: T002 Line contains TODO
+ zerver/lib/import_realm.py:1090:11: FIX002 Line contains TODO
- zerver/lib/import_realm.py:1090:11: T002 Line contains TODO
+ zerver/lib/import_realm.py:1385:7: FIX002 Line contains TODO
- zerver/lib/import_realm.py:1385:7: T002 Line contains TODO
+ zerver/lib/import_realm.py:1613:7: FIX002 Line contains TODO
- zerver/lib/import_realm.py:1613:7: T002 Line contains TODO
+ zerver/lib/import_realm.py:682:7: FIX002 Line contains TODO
- zerver/lib/import_realm.py:682:7: T002 Line contains TODO
+ zerver/lib/import_realm.py:802:19: FIX002 Line contains TODO
- zerver/lib/import_realm.py:802:19: T002 Line contains TODO
+ zerver/lib/integrations.py:100:11: FIX002 Line contains TODO
- zerver/lib/integrations.py:100:11: T002 Line contains TODO
+ zerver/lib/integrations.py:163:19: FIX002 Line contains TODO
- zerver/lib/integrations.py:163:19: T002 Line contains TODO
+ zerver/lib/integrations.py:692:21: FIX001 Line contains FIXME
- zerver/lib/integrations.py:692:21: T001 Line contains FIXME
+ zerver/lib/markdown/__init__.py:1591:11: FIX004 Line contains HACK
- zerver/lib/markdown/__init__.py:1591:11: T004 Line contains HACK
+ zerver/lib/markdown/__init__.py:1878:11: FIX002 Line contains TODO
- zerver/lib/markdown/__init__.py:1878:11: T002 Line contains TODO
+ zerver/lib/markdown/__init__.py:713:11: FIX002 Line contains TODO
- zerver/lib/markdown/__init__.py:713:11: T002 Line contains TODO
+ zerver/lib/markdown/__init__.py:746:15: FIX002 Line contains TODO
- zerver/lib/markdown/__init__.py:746:15: T002 Line contains TODO
+ zerver/lib/markdown/api_arguments_table_generator.py:133:15: FIX002 Line contains TODO
- zerver/lib/markdown/api_arguments_table_generator.py:133:15: T002 Line contains TODO
+ zerver/lib/markdown/api_arguments_table_generator.py:166:15: FIX002 Line contains TODO
- zerver/lib/markdown/api_arguments_table_generator.py:166:15: T002 Line contains TODO
+ zerver/lib/markdown/api_return_values_table_generator.py:70:15: FIX004 Line contains HACK
- zerver/lib/markdown/api_return_values_table_generator.py:70:15: T004 Line contains HACK
+ zerver/lib/message.py:1146:11: FIX002 Line contains TODO
- zerver/lib/message.py:1146:11: T002 Line contains TODO
+ zerver/lib/message.py:1157:35: FIX002 Line contains TODO
- zerver/lib/message.py:1157:35: T002 Line contains TODO
+ zerver/lib/message.py:570:15: FIX002 Line contains TODO
- zerver/lib/message.py:570:15: T002 Line contains TODO
+ zerver/lib/message.py:714:15: FIX002 Line contains TODO
- zerver/lib/message.py:714:15: T002 Line contains TODO
+ zerver/lib/narrow.py:278:15: FIX002 Line contains TODO
- zerver/lib/narrow.py:278:15: T002 Line contains TODO
+ zerver/lib/presence.py:147:11: FIX002 Line contains TODO
- zerver/lib/presence.py:147:11: T002 Line contains TODO
+ zerver/lib/presence.py:69:7: FIX002 Line contains TODO
- zerver/lib/presence.py:69:7: T002 Line contains TODO
+ zerver/lib/presence.py:88:7: FIX002 Line contains TODO
- zerver/lib/presence.py:88:7: T002 Line contains TODO
+ zerver/lib/push_notifications.py:1049:11: FIX002 Line contains TODO
- zerver/lib/push_notifications.py:1049:11: T002 Line contains TODO
+ zerver/lib/push_notifications.py:155:7: FIX002 Line contains TODO
- zerver/lib/push_notifications.py:155:7: T002 Line contains TODO
+ zerver/lib/push_notifications.py:227:11: FIX002 Line contains TODO
- zerver/lib/push_notifications.py:227:11: T002 Line contains TODO
+ zerver/lib/push_notifications.py:580:11: FIX002 Line contains TODO
- zerver/lib/push_notifications.py:580:11: T002 Line contains TODO
+ zerver/lib/request.py:381:19: FIX002 Line contains TODO
- zerver/lib/request.py:381:19: T002 Line contains TODO
+ zerver/lib/request.py:465:15: FIX002 Line contains TODO
- zerver/lib/request.py:465:15: T002 Line contains TODO
+ zerver/lib/rest.py:190:11: FIX002 Line contains TODO
- zerver/lib/rest.py:190:11: T002 Line contains TODO
+ zerver/lib/retention.py:365:32: FIX002 Line contains TODO
- zerver/lib/retention.py:365:32: T002 Line contains TODO
+ zerver/lib/scim.py:109:15: FIX002 Line contains TODO
- zerver/lib/scim.py:109:15: T002 Line contains TODO
+ zerver/lib/scim.py:211:19: FIX002 Line contains TODO
- zerver/lib/scim.py:211:19: T002 Line contains TODO
+ zerver/lib/scim.py:278:11: FIX002 Line contains TODO
- zerver/lib/scim.py:278:11: T002 Line contains TODO
+ zerver/lib/sessions.py:66:54: FIX002 Line contains TODO
- zerver/lib/sessions.py:66:54: T002 Line contains TODO
+ zerver/lib/storage.py:30:15: FIX004 Line contains HACK
- zerver/lib/storage.py:30:15: T004 Line contains HACK
+ zerver/lib/templates.py:135:11: FIX002 Line contains TODO
- zerver/lib/templates.py:135:11: T002 Line contains TODO
+ zerver/lib/test_classes.py:880:11: FIX002 Line contains TODO
- zerver/lib/test_classes.py:880:11: T002 Line contains TODO
+ zerver/lib/test_fixtures.py:183:15: FIX002 Line contains TODO
- zerver/lib/test_fixtures.py:183:15: T002 Line contains TODO
+ zerver/lib/test_helpers.py:388:74: FIX002 Line contains TODO
- zerver/lib/test_helpers.py:388:74: T002 Line contains TODO
+ zerver/lib/test_helpers.py:396:7: FIX002 Line contains TODO
- zerver/lib/test_helpers.py:396:7: T002 Line contains TODO
+ zerver/lib/test_helpers.py:536:28: FIX002 Line contains TODO
- zerver/lib/test_helpers.py:536:28: T002 Line contains TODO
+ zerver/lib/transfer.py:19:7: FIX002 Line contains TODO
- zerver/lib/transfer.py:19:7: T002 Line contains TODO
+ zerver/lib/url_preview/preview.py:33:3: FIX001 Line contains FIXME
- zerver/lib/url_preview/preview.py:33:3: T001 Line contains FIXME
+ zerver/lib/users.py:122:15: FIX002 Line contains TODO
- zerver/lib/users.py:122:15: T002 Line contains TODO
+ zerver/lib/users.py:216:7: FIX002 Line contains TODO
- zerver/lib/users.py:216:7: T002 Line contains TODO
+ zerver/lib/users.py:610:11: FIX002 Line contains TODO
- zerver/lib/users.py:610:11: T002 Line contains TODO
+ zerver/lib/webhooks/common.py:196:11: FIX002 Line contains TODO
- zerver/lib/webhooks/common.py:196:11: T002 Line contains TODO
+ zerver/management/commands/convert_gitter_data.py:46:15: FIX002 Line contains TODO
- zerver/management/commands/convert_gitter_data.py:46:15: T002 Line contains TODO
+ zerver/management/commands/delete_realm.py:59:11: FIX002 Line contains TODO
- zerver/management/commands/delete_realm.py:59:11: T002 Line contains TODO
+ zerver/management/commands/list_realms.py:52:15: FIX004 Line contains HACK
- zerver/management/commands/list_realms.py:52:15: T004 Line contains HACK
+ zerver/middleware.py:398:19: FIX002 Line contains TODO
- zerver/middleware.py:398:19: T002 Line contains TODO
+ zerver/migrations/0206_stream_rendered_description.py:9:7: FIX001 Line contains FIXME
- zerver/migrations/0206_stream_rendered_description.py:9:7: T001 Line contains FIXME
+ zerver/migrations/0239_usermessage_copy_id_to_bigint_id.py:38:8: FIX002 Line contains TODO
- zerver/migrations/0239_usermessage_copy_id_to_bigint_id.py:38:8: T002 Line contains TODO
+ zerver/models.py:2488:7: FIX002 Line contains TODO
- zerver/models.py:2488:7: T002 Line contains TODO
+ zerver/models.py:2570:7: FIX002 Line contains TODO
- zerver/models.py:2570:7: T002 Line contains TODO
+ zerver/models.py:4040:7: FIX002 Line contains TODO
- zerver/models.py:4040:7: T002 Line contains TODO
+ zerver/models.py:4662:11: FIX002 Line contains TODO
- zerver/models.py:4662:11: T002 Line contains TODO
+ zerver/openapi/markdown_extension.py:214:11: FIX002 Line contains TODO
- zerver/openapi/markdown_extension.py:214:11: T002 Line contains TODO
+ zerver/openapi/markdown_extension.py:234:11: FIX004 Line contains HACK
- zerver/openapi/markdown_extension.py:234:11: T004 Line contains HACK
+ zerver/openapi/openapi.py:408:11: FIX002 Line contains TODO
- zerver/openapi/openapi.py:408:11: T002 Line contains TODO
+ zerver/openapi/openapi.py:413:11: FIX002 Line contains TODO
- zerver/openapi/openapi.py:413:11: T002 Line contains TODO
+ zerver/openapi/openapi.py:496:7: FIX002 Line contains TODO
- zerver/openapi/openapi.py:496:7: T002 Line contains TODO
+ zerver/openapi/python_examples.py:433:7: FIX002 Line contains TODO
- zerver/openapi/python_examples.py:433:7: T002 Line contains TODO
+ zerver/tests/test_auth_backends.py:3822:15: FIX002 Line contains TODO
- zerver/tests/test_auth_backends.py:3822:15: T002 Line contains TODO
+ zerver/tests/test_bots.py:1499:11: FIX002 Line contains TODO
- zerver/tests/test_bots.py:1499:11: T002 Line contains TODO
+ zerver/tests/test_decorators.py:119:11: FIX002 Line contains TODO
- zerver/tests/test_decorators.py:119:11: T002 Line contains TODO
+ zerver/tests/test_decorators.py:126:11: FIX002 Line contains TODO
- zerver/tests/test_decorators.py:126:11: T002 Line contains TODO
+ zerver/tests/test_decorators.py:133:11: FIX002 Line contains TODO
- zerver/tests/test_decorators.py:133:11: T002 Line contains TODO
+ zerver/tests/test_docs.py:304:11: FIX002 Line contains TODO
- zerver/tests/test_docs.py:304:11: T002 Line contains TODO
+ zerver/tests/test_docs.py:445:11: FIX002 Line contains TODO
- zerver/tests/test_docs.py:445:11: T002 Line contains TODO
+ zerver/tests/test_home.py:1186:11: FIX002 Line contains TODO
- zerver/tests/test_home.py:1186:11: T002 Line contains TODO
+ zerver/tests/test_home.py:1200:7: FIX002 Line contains TODO
- zerver/tests/test_home.py:1200:7: T002 Line contains TODO
+ zerver/tests/test_home.py:1212:11: FIX002 Line contains TODO
- zerver/tests/test_home.py:1212:11: T002 Line contains TODO
+ zerver/tests/test_home.py:272:11: FIX002 Line contains TODO
- zerver/tests/test_home.py:272:11: T002 Line contains TODO
+ zerver/tests/test_i18n.py:45:11: FIX002 Line contains TODO
- zerver/tests/test_i18n.py:45:11: T002 Line contains TODO
+ zerver/tests/test_i18n.py:72:11: FIX002 Line contains TODO
- zerver/tests/test_i18n.py:72:11: T002 Line contains TODO
+ zerver/tests/test_invite.py:1014:11: FIX002 Line contains TODO
- zerver/tests/test_invite.py:1014:11: T002 Line contains TODO
+ zerver/tests/test_link_embed.py:909:15: FIX001 Line contains FIXME
- zerver/tests/test_link_embed.py:909:15: T001 Line contains FIXME
+ zerver/tests/test_mattermost_importer.py:232:11: FIX002 Line contains TODO
- zerver/tests/test_mattermost_importer.py:232:11: T002 Line contains TODO
+ zerver/tests/test_mattermost_importer.py:430:11: FIX002 Line contains TODO
- zerver/tests/test_mattermost_importer.py:430:11: T002 Line contains TODO
+ zerver/tests/test_message_flags.py:1042:11: FIX002 Line contains TODO
- zerver/tests/test_message_flags.py:1042:11: T002 Line contains TODO
+ zerver/tests/test_message_send.py:2206:15: FIX004 Line contains HACK
- zerver/tests/test_message_send.py:2206:15: T004 Line contains HACK
+ zerver/tests/test_message_topics.py:42:15: FIX002 Line contains TODO
- zerver/tests/test_message_topics.py:42:15: T002 Line contains TODO
+ zerver/tests/test_openapi.py:207:14: FIX002 Line contains TODO
- zerver/tests/test_openapi.py:207:14: T002 Line contains TODO
+ zerver/tests/test_openapi.py:442:19: FIX002 Line contains TODO
- zerver/tests/test_openapi.py:442:19: T002 Line contains TODO
+ zerver/tests/test_openapi.py:528:23: FIX002 Line contains TODO
- zerver/tests/test_openapi.py:528:23: T002 Line contains TODO
+ zerver/tests/test_openapi.py:551:27: FIX004 Line contains HACK
- zerver/tests/test_openapi.py:551:27: T004 Line contains HACK
+ zerver/tests/test_openapi.py:59:11: FIX004 Line contains HACK
- zerver/tests/test_openapi.py:59:11: T004 Line contains HACK
+ zerver/tests/test_queue_worker.py:319:11: FIX004 Line contains HACK
- zerver/tests/test_queue_worker.py:319:11: T004 Line contains HACK
+ zerver/tests/test_rocketchat_importer.py:855:11: FIX002 Line contains TODO
- zerver/tests/test_rocketchat_importer.py:855:11: T002 Line contains TODO
+ zerver/tests/test_settings.py:28:7: FIX002 Line contains TODO
- zerver/tests/test_settings.py:28:7: T002 Line contains TODO
+ zerver/tests/test_slack_importer.py:1147:15: FIX004 Line contains HACK
- zerver/tests/test_slack_importer.py:1147:15: T004 Line contains HACK
+ zerver/tests/test_subs.py:3104:34: FIX002 Line contains TODO
- zerver/tests/test_subs.py:3104:34: T002 Line contains TODO
+ zerver/tests/test_tornado.py:88:11: FIX002 Line contains TODO
- zerver/tests/test_tornado.py:88:11: T002 Line contains TODO
+ zerver/tests/test_urls.py:40:11: FIX001 Line contains FIXME
- zerver/tests/test_urls.py:40:11: T001 Line contains FIXME
+ zerver/tornado/event_queue.py:1141:7: FIX002 Line contains TODO
- zerver/tornado/event_queue.py:1141:7: T002 Line contains TODO
+ zerver/tornado/event_queue.py:1153:7: FIX002 Line contains TODO
- zerver/tornado/event_queue.py:1153:7: T002 Line contains TODO
+ zerver/tornado/event_queue.py:116:45: FIX002 Line contains TODO
- zerver/tornado/event_queue.py:116:45: T002 Line contains TODO
+ zerver/tornado/event_queue.py:1261:11: FIX002 Line contains TODO
- zerver/tornado/event_queue.py:1261:11: T002 Line contains TODO
+ zerver/tornado/event_queue.py:1326:11: FIX002 Line contains TODO
- zerver/tornado/event_queue.py:1326:11: T002 Line contains TODO
+ zerver/tornado/event_queue.py:1345:15: FIX002 Line contains TODO
- zerver/tornado/event_queue.py:1345:15: T002 Line contains TODO
+ zerver/tornado/event_queue.py:1360:15: FIX002 Line contains TODO
- zerver/tornado/event_queue.py:1360:15: T002 Line contains TODO
+ zerver/tornado/event_queue.py:245:7: FIX002 Line contains TODO
- zerver/tornado/event_queue.py:245:7: T002 Line contains TODO
+ zerver/tornado/handlers.py:207:11: FIX002 Line contains TODO
- zerver/tornado/handlers.py:207:11: T002 Line contains TODO
+ zerver/tornado/views.py:62:11: FIX002 Line contains TODO
- zerver/tornado/views.py:62:11: T002 Line contains TODO
+ zerver/views/auth.py:1244:11: FIX002 Line contains TODO
- zerver/views/auth.py:1244:11: T002 Line contains TODO
+ zerver/views/auth.py:213:11: FIX002 Line contains TODO
- zerver/views/auth.py:213:11: T002 Line contains TODO
+ zerver/views/auth.py:685:7: FIX002 Line contains TODO
- zerver/views/auth.py:685:7: T002 Line contains TODO
+ zerver/views/custom_profile_fields.py:227:11: FIX004 Line contains HACK
- zerver/views/custom_profile_fields.py:227:11: T004 Line contains HACK
+ zerver/views/custom_profile_fields.py:230:11: FIX002 Line contains TODO
- zerver/views/custom_profile_fields.py:230:11: T002 Line contains TODO
+ zerver/views/documentation.py:232:11: FIX004 Line contains HACK
- zerver/views/documentation.py:232:11: T004 Line contains HACK
+ zerver/views/documentation.py:297:7: FIX001 Line contains FIXME
- zerver/views/documentation.py:297:7: T001 Line contains FIXME
+ zerver/views/home.py:116:15: FIX002 Line contains TODO
- zerver/views/home.py:116:15: T002 Line contains TODO
+ zerver/views/home.py:136:11: FIX002 Line contains TODO
- zerver/views/home.py:136:11: T002 Line contains TODO
+ zerver/views/message_fetch.py:192:11: FIX002 Line contains TODO
- zerver/views/message_fetch.py:192:11: T002 Line contains TODO
+ zerver/views/message_send.py:149:11: FIX002 Line contains TODO
- zerver/views/message_send.py:149:11: T002 Line contains TODO
+ zerver/views/presence.py:76:7: FIX002 Line contains TODO
- zerver/views/presence.py:76:7: T002 Line contains TODO
+ zerver/views/realm.py:269:7: FIX002 Line contains TODO
- zerver/views/realm.py:269:7: T002 Line contains TODO
+ zerver/views/realm_icon.py:64:7: FIX004 Line contains HACK
- zerver/views/realm_icon.py:64:7: T004 Line contains HACK
+ zerver/views/realm_logo.py:67:7: FIX004 Line contains HACK
- zerver/views/realm_logo.py:67:7: T004 Line contains HACK
+ zerver/views/registration.py:316:23: FIX002 Line contains TODO
- zerver/views/registration.py:316:23: T002 Line contains TODO
+ zerver/views/registration.py:525:23: FIX002 Line contains TODO
- zerver/views/registration.py:525:23: T002 Line contains TODO
+ zerver/views/registration.py:554:15: FIX002 Line contains TODO
- zerver/views/registration.py:554:15: T002 Line contains TODO
+ zerver/views/scheduled_messages.py:123:11: FIX002 Line contains TODO
- zerver/views/scheduled_messages.py:123:11: T002 Line contains TODO
+ zerver/views/scheduled_messages.py:75:11: FIX002 Line contains TODO
- zerver/views/scheduled_messages.py:75:11: T002 Line contains TODO
+ zerver/views/streams.py:930:11: FIX002 Line contains TODO
- zerver/views/streams.py:930:11: T002 Line contains TODO
+ zerver/views/typing.py:35:11: FIX002 Line contains TODO
- zerver/views/typing.py:35:11: T002 Line contains TODO
+ zerver/views/user_settings.py:81:11: FIX002 Line contains TODO
- zerver/views/user_settings.py:81:11: T002 Line contains TODO
+ zerver/views/users.py:320:7: FIX004 Line contains HACK
- zerver/views/users.py:320:7: T004 Line contains HACK
+ zerver/views/zephyr.py:23:3: FIX004 Line contains HACK
- zerver/views/zephyr.py:23:3: T004 Line contains HACK
+ zerver/views/zephyr.py:65:7: FIX002 Line contains TODO
- zerver/views/zephyr.py:65:7: T002 Line contains TODO
+ zerver/worker/queue_processors.py:536:11: FIX002 Line contains TODO
- zerver/worker/queue_processors.py:536:11: T002 Line contains TODO
+ zerver/worker/queue_processors.py:961:11: FIX002 Line contains TODO
- zerver/worker/queue_processors.py:961:11: T002 Line contains TODO
+ zilencer/management/commands/populate_db.py:107:26: FIX004 Line contains HACK
- zilencer/management/commands/populate_db.py:107:26: T004 Line contains HACK
+ zilencer/management/commands/populate_db.py:94:7: FIX004 Line contains HACK
- zilencer/management/commands/populate_db.py:94:7: T004 Line contains HACK
+ zilencer/views.py:105:11: FIX002 Line contains TODO
- zilencer/views.py:105:11: T002 Line contains TODO
+ zproject/backends.py:1043:15: FIX002 Line contains TODO
- zproject/backends.py:1043:15: T002 Line contains TODO
+ zproject/backends.py:1546:15: FIX002 Line contains TODO
- zproject/backends.py:1546:15: T002 Line contains TODO
+ zproject/backends.py:2846:7: FIX004 Line contains HACK
- zproject/backends.py:2846:7: T004 Line contains HACK
+ zproject/default_settings.py:140:3: FIX002 Line contains TODO
- zproject/default_settings.py:140:3: T002 Line contains TODO
+ zproject/default_settings.py:164:3: FIX002 Line contains TODO
- zproject/default_settings.py:164:3: T002 Line contains TODO
+ zproject/default_settings.py:482:3: FIX002 Line contains TODO
- zproject/default_settings.py:482:3: T002 Line contains TODO
```

</p>
</details>
Rules changed: 8

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| FIX002 | 438 | 438 | 0 |
| T002 | 438 | 0 | 438 |
| FIX003 | 78 | 78 | 0 |
| T003 | 78 | 0 | 78 |
| FIX004 | 35 | 35 | 0 |
| T004 | 35 | 0 | 35 |
| FIX001 | 10 | 10 | 0 |
| T001 | 10 | 0 | 10 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.5±0.02ms     7.4 MB/sec    1.04      5.7±0.03ms     7.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1077.4±2.28µs    15.5 MB/sec    1.11   1193.9±2.59µs    13.9 MB/sec
formatter/numpy/globals.py                 1.00    120.3±0.33µs    24.5 MB/sec    1.16    139.4±1.25µs    21.2 MB/sec
formatter/pydantic/types.py                1.00      2.4±0.01ms    10.6 MB/sec    1.06      2.5±0.01ms    10.0 MB/sec
linter/all-rules/large/dataset.py          1.00     14.4±0.15ms     2.8 MB/sec    1.06     15.3±0.17ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.02ms     4.9 MB/sec    1.04      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    418.0±1.96µs     7.1 MB/sec    1.03    430.0±1.01µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.06ms     4.3 MB/sec    1.07      6.3±0.06ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.03ms     6.0 MB/sec    1.16      7.9±0.05ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1469.0±5.48µs    11.3 MB/sec    1.11   1635.4±5.48µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.9±0.43µs    18.0 MB/sec    1.08    177.0±0.33µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.13      3.5±0.02ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.9±0.04ms     5.9 MB/sec    1.03      7.2±0.03ms     5.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1323.4±11.79µs    12.6 MB/sec    1.07  1413.9±20.70µs    11.8 MB/sec
formatter/numpy/globals.py                 1.00    149.5±1.60µs    19.7 MB/sec    1.10    164.1±3.52µs    18.0 MB/sec
formatter/pydantic/types.py                1.00      3.0±0.02ms     8.5 MB/sec    1.05      3.1±0.05ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.00     16.6±0.03ms     2.5 MB/sec    1.00     16.6±0.05ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.02ms     3.9 MB/sec    1.00      4.3±0.03ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01   432.2±21.58µs     6.8 MB/sec    1.00    429.0±6.06µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.02ms     3.6 MB/sec    1.00      7.1±0.02ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.03ms     4.9 MB/sec    1.00      8.4±0.01ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1737.0±7.17µs     9.6 MB/sec    1.00  1743.3±11.63µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    187.4±4.37µs    15.7 MB/sec    1.00    186.8±1.29µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.7 MB/sec    1.00      3.8±0.02ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `breaking` added by @MichaReiser on 2023-06-07 12:47_

---

_@MichaReiser approved on 2023-06-07 12:48_

---

_@konstin approved on 2023-06-07 15:28_

---

_Merged by @charliermarsh on 2023-06-07 21:14_

---

_Closed by @charliermarsh on 2023-06-07 21:14_

---

_Branch deleted on 2023-06-07 21:14_

---
