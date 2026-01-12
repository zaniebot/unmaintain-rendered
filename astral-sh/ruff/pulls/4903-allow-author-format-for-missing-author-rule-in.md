```yaml
number: 4903
title: "Allow `@Author` format for \"Missing Author\" rule in `flake8-todos`"
type: pull_request
state: merged
author: mayrholu
labels: []
assignees: []
merged: true
base: main
head: patch-1
created_at: 2023-06-06T13:54:59Z
updated_at: 2023-06-22T21:12:03Z
url: https://github.com/astral-sh/ruff/pull/4903
synced_at: 2026-01-12T15:55:17Z
```

# Allow `@Author` format for "Missing Author" rule in `flake8-todos`

---

_@mayrholu_

## Summary

The TD-002 rule "Missing Author" was updated to allow another format using "@". This reflects the current 0.3.0 version of flake8-todos.




---

_Closed by @mayrholu on 2023-06-06 14:15_

---

_Branch deleted on 2023-06-06 14:15_

---

_Branch restored on 2023-06-06 14:15_

---

_Reopened by @mayrholu on 2023-06-06 14:16_

---

_Comment by @github-actions[bot] on 2023-06-06 14:27_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+446, -446, 0 error(s))

<details><summary>airflow (+146, -146)</summary>
<p>

```diff
- airflow/api_connexion/schemas/dag_schema.py:98:55: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/api_connexion/schemas/dag_schema.py:98:55: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/cli/cli_config.py:277:6: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/cli/cli_config.py:277:6: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/cli/commands/task_command.py:183:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/cli/commands/task_command.py:183:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/cli/commands/task_command.py:224:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/cli/commands/task_command.py:224:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/cli/commands/task_command.py:456:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/cli/commands/task_command.py:456:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/cli/commands/task_command.py:663:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/cli/commands/task_command.py:663:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/compat/functools.pyi:20:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/compat/functools.pyi:20:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/dag_processing/manager.py:237:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/dag_processing/manager.py:237:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/decorators/base.py:216:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/decorators/base.py:216:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/decorators/task_group.py:117:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/decorators/task_group.py:117:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/decorators/task_group.py:124:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/decorators/task_group.py:124:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/executors/executor_loader.py:205:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/executors/executor_loader.py:205:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/jobs/backfill_job_runner.py:264:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/jobs/backfill_job_runner.py:264:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/jobs/backfill_job_runner.py:943:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/jobs/backfill_job_runner.py:943:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/jobs/job.py:191:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/jobs/job.py:191:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/jobs/local_task_job_runner.py:79:41: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/jobs/local_task_job_runner.py:79:41: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/jobs/scheduler_job_runner.py:1199:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/jobs/scheduler_job_runner.py:1199:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/jobs/scheduler_job_runner.py:1339:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/jobs/scheduler_job_runner.py:1339:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/jobs/scheduler_job_runner.py:1454:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/jobs/scheduler_job_runner.py:1454:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/jobs/scheduler_job_runner.py:168:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/jobs/scheduler_job_runner.py:168:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/jobs/scheduler_job_runner.py:189:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/jobs/scheduler_job_runner.py:189:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/jobs/scheduler_job_runner.py:197:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/jobs/scheduler_job_runner.py:197:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/jobs/scheduler_job_runner.py:207:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/jobs/scheduler_job_runner.py:207:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/jobs/scheduler_job_runner.py:405:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/jobs/scheduler_job_runner.py:405:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/jobs/scheduler_job_runner.py:603:23: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/jobs/scheduler_job_runner.py:603:23: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/metrics/otel_logger.py:400:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/metrics/otel_logger.py:400:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/models/abstractoperator.py:532:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/models/abstractoperator.py:532:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/models/baseoperator.py:1236:40: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/models/baseoperator.py:1236:40: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/models/baseoperator.py:1608:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/models/baseoperator.py:1608:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/models/baseoperator.py:919:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/models/baseoperator.py:919:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/models/dag.py:1166:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/models/dag.py:1166:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/models/dag.py:128:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/models/dag.py:128:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/models/dag.py:1490:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/models/dag.py:1490:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/models/dag.py:457:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/models/dag.py:457:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/models/dagbag.py:287:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/models/dagbag.py:287:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/models/dagbag.py:396:23: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/models/dagbag.py:396:23: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/models/dagrun.py:1202:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/models/dagrun.py:1202:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/models/dagrun.py:318:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/models/dagrun.py:318:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/models/dagrun.py:932:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/models/dagrun.py:932:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/models/expandinput.py:137:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/models/expandinput.py:137:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/models/taskinstance.py:147:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/models/taskinstance.py:147:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/models/taskinstance.py:2408:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/models/taskinstance.py:2408:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/amazon/aws/hooks/redshift_cluster.py:85:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/amazon/aws/hooks/redshift_cluster.py:85:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/amazon/aws/operators/emr.py:241:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/amazon/aws/operators/emr.py:241:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/amazon/aws/operators/emr.py:357:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/amazon/aws/operators/emr.py:357:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/amazon/aws/operators/emr.py:659:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/amazon/aws/operators/emr.py:659:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/amazon/aws/utils/connection_wrapper.py:226:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/amazon/aws/utils/connection_wrapper.py:226:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/cncf/kubernetes/decorators/kubernetes.py:80:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/cncf/kubernetes/decorators/kubernetes.py:80:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/cncf/kubernetes/operators/pod.py:308:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/cncf/kubernetes/operators/pod.py:308:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/cncf/kubernetes/operators/pod.py:315:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/cncf/kubernetes/operators/pod.py:315:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/cncf/kubernetes/operators/pod.py:387:50: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/cncf/kubernetes/operators/pod.py:387:50: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/databricks/hooks/databricks_base.py:494:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/databricks/hooks/databricks_base.py:494:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/databricks/operators/databricks_sql.py:349:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/databricks/operators/databricks_sql.py:349:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/dbt/cloud/sensors/dbt.py:58:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/dbt/cloud/sensors/dbt.py:58:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/docker/decorators/docker.py:139:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/docker/decorators/docker.py:139:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/elasticsearch/log/es_task_handler.py:368:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/elasticsearch/log/es_task_handler.py:368:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/github/hooks/github.py:60:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/github/hooks/github.py:60:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/hooks/cloud_storage_transfer_service.py:223:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/hooks/cloud_storage_transfer_service.py:223:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/hooks/cloud_storage_transfer_service.py:347:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/hooks/cloud_storage_transfer_service.py:347:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/hooks/compute_ssh.py:35:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/hooks/compute_ssh.py:35:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/hooks/datafusion.py:437:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/hooks/datafusion.py:437:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/hooks/gcs.py:1152:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/hooks/gcs.py:1152:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/hooks/gcs.py:328:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/hooks/gcs.py:328:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/hooks/kubernetes_engine.py:107:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/hooks/kubernetes_engine.py:107:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/hooks/kubernetes_engine.py:98:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/hooks/kubernetes_engine.py:98:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/hooks/pubsub.py:148:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/hooks/pubsub.py:148:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/log/stackdriver_task_handler.py:173:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/log/stackdriver_task_handler.py:173:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/log/stackdriver_task_handler.py:38:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/log/stackdriver_task_handler.py:38:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py:547:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py:547:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/operators/dataflow.py:1137:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/operators/dataflow.py:1137:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/operators/dataflow.py:350:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/operators/dataflow.py:350:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/operators/dataproc.py:1163:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/operators/dataproc.py:1163:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/operators/dataproc.py:1237:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/operators/dataproc.py:1237:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/operators/dataproc.py:1312:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/operators/dataproc.py:1312:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/operators/dataproc.py:1389:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/operators/dataproc.py:1389:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/operators/dataproc.py:1462:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/operators/dataproc.py:1462:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/operators/dataproc.py:1559:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/operators/dataproc.py:1559:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/operators/dataproc.py:493:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/operators/dataproc.py:493:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/operators/dataproc.py:740:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/operators/dataproc.py:740:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/operators/gcs.py:765:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/operators/gcs.py:765:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/operators/gcs.py:806:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/operators/gcs.py:806:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/sensors/bigquery.py:78:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/sensors/bigquery.py:78:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/sensors/tasks.py:80:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/sensors/tasks.py:80:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/google/cloud/transfers/gcs_to_local.py:89:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/google/cloud/transfers/gcs_to_local.py:89:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/jenkins/operators/jenkins_job_trigger.py:154:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/jenkins/operators/jenkins_job_trigger.py:154:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/microsoft/azure/hooks/adx.py:120:46: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/microsoft/azure/hooks/adx.py:120:46: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/microsoft/azure/hooks/cosmos.py:74:52: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/microsoft/azure/hooks/cosmos.py:74:52: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/microsoft/azure/log/wasb_task_handler.py:136:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/microsoft/azure/log/wasb_task_handler.py:136:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/microsoft/winrm/hooks/winrm.py:27:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/microsoft/winrm/hooks/winrm.py:27:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/openlineage/extractors/manager.py:150:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/openlineage/extractors/manager.py:150:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/openlineage/plugins/listener.py:162:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/openlineage/plugins/listener.py:162:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/openlineage/utils/utils.py:348:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/openlineage/utils/utils.py:348:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/openlineage/utils/utils.py:40:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/openlineage/utils/utils.py:40:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/sftp/hooks/sftp.py:115:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/sftp/hooks/sftp.py:115:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/sftp/hooks/sftp.py:81:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/sftp/hooks/sftp.py:81:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/sftp/operators/sftp.py:129:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/sftp/operators/sftp.py:129:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/providers/snowflake/hooks/snowflake.py:45:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/providers/snowflake/hooks/snowflake.py:45:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/secrets/base_secrets.py:93:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/secrets/base_secrets.py:93:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/serialization/serde.py:165:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/serialization/serde.py:165:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/serialization/serialized_objects.py:1049:42: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/serialization/serialized_objects.py:1049:42: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/serialization/serialized_objects.py:449:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/serialization/serialized_objects.py:449:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/serialization/serialized_objects.py:463:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/serialization/serialized_objects.py:463:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/serialization/serialized_objects.py:647:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/serialization/serialized_objects.py:647:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/serialization/serialized_objects.py:911:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/serialization/serialized_objects.py:911:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/serialization/serialized_objects.py:969:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/serialization/serialized_objects.py:969:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/ti_deps/deps/trigger_rule_dep.py:243:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/ti_deps/deps/trigger_rule_dep.py:243:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/utils/db.py:1784:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/utils/db.py:1784:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/utils/db.py:1797:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/utils/db.py:1797:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/utils/db.py:42:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/utils/db.py:42:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/www/api/experimental/endpoints.py:224:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/www/api/experimental/endpoints.py:224:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/www/fab_security/manager.py:1486:27: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/www/fab_security/manager.py:1486:27: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/www/views.py:1341:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/www/views.py:1341:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/www/views.py:2367:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/www/views.py:2367:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/www/views.py:4162:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/www/views.py:4162:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/www/views.py:446:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/www/views.py:446:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/www/views.py:5292:21: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/www/views.py:5292:21: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- airflow/www/views.py:5654:22: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ airflow/www/views.py:5654:22: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- dev/stats/get_important_pr_candidates.py:351:25: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ dev/stats/get_important_pr_candidates.py:351:25: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- docs/conf.py:525:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ docs/conf.py:525:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- kubernetes_tests/test_kubernetes_pod_operator.py:712:63: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ kubernetes_tests/test_kubernetes_pod_operator.py:712:63: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- kubernetes_tests/test_kubernetes_pod_operator.py:896:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ kubernetes_tests/test_kubernetes_pod_operator.py:896:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- scripts/in_container/verify_providers.py:859:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ scripts/in_container/verify_providers.py:859:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- setup.py:345:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ setup.py:345:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- setup.py:420:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ setup.py:420:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/always/test_project_structure.py:60:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/always/test_project_structure.py:60:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/api_experimental/common/test_mark_tasks.py:410:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/api_experimental/common/test_mark_tasks.py:410:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/cli/commands/test_dag_command.py:49:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/cli/commands/test_dag_command.py:49:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/cli/commands/test_task_command.py:74:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/cli/commands/test_task_command.py:74:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/core/test_stats.py:212:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/core/test_stats.py:212:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/jobs/test_scheduler_job.py:1059:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/jobs/test_scheduler_job.py:1059:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/jobs/test_scheduler_job.py:2279:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/jobs/test_scheduler_job.py:2279:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/jobs/test_scheduler_job.py:2340:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/jobs/test_scheduler_job.py:2340:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/jobs/test_scheduler_job.py:4661:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/jobs/test_scheduler_job.py:4661:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/jobs/test_triggerer_job_logging.py:430:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/jobs/test_triggerer_job_logging.py:430:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/kubernetes/test_kubernetes_helper_functions.py:31:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/kubernetes/test_kubernetes_helper_functions.py:31:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/models/test_dag.py:3645:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/models/test_dag.py:3645:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/models/test_dag.py:3776:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/models/test_dag.py:3776:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/providers/cncf/kubernetes/operators/test_pod.py:251:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/providers/cncf/kubernetes/operators/test_pod.py:251:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/providers/elasticsearch/log/elasticmock/fake_elasticsearch.py:369:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/providers/elasticsearch/log/elasticmock/fake_elasticsearch.py:369:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/providers/google/cloud/operators/test_cloud_memorystore.py:61:53: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/providers/google/cloud/operators/test_cloud_memorystore.py:61:53: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/providers/microsoft/azure/hooks/test_azure_batch.py:149:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/providers/microsoft/azure/hooks/test_azure_batch.py:149:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/providers/microsoft/azure/hooks/test_azure_batch.py:172:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/providers/microsoft/azure/hooks/test_azure_batch.py:172:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/serialization/test_dag_serialization.py:309:52: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/serialization/test_dag_serialization.py:309:52: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/serialization/test_dag_serialization.py:310:54: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/serialization/test_dag_serialization.py:310:54: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/system/providers/snowflake/example_s3_to_snowflake.py:30:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/system/providers/snowflake/example_s3_to_snowflake.py:30:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/www/test_security.py:876:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/www/test_security.py:876:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
```

</p>
</details>
<details><summary>bokeh (+121, -121)</summary>
<p>

```diff
- examples/integration/layout/plot_fixed_frame_size.py:38:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ examples/integration/layout/plot_fixed_frame_size.py:38:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- examples/models/legends.py:53:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ examples/models/legends.py:53:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- examples/server/app/simple_hdf5/main.py:50:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ examples/server/app/simple_hdf5/main.py:50:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- release/credentials.py:77:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ release/credentials.py:77:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- release/credentials.py:84:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ release/credentials.py:84:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/application/handlers/code.py:168:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/application/handlers/code.py:168:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/application/handlers/code_runner.py:225:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/application/handlers/code_runner.py:225:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/application/handlers/directory.py:313:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/application/handlers/directory.py:313:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/application/handlers/server_lifecycle.py:127:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/application/handlers/server_lifecycle.py:127:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/application/handlers/server_request_handler.py:120:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/application/handlers/server_request_handler.py:120:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/client/connection.py:349:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/client/connection.py:349:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/core/has_props.py:250:41: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/has_props.py:250:41: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/core/has_props.py:292:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/has_props.py:292:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/core/has_props.py:443:99: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/has_props.py:443:99: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/core/has_props.py:656:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/has_props.py:656:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/core/has_props.py:749:17: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/has_props.py:749:17: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/core/has_props.py:776:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/has_props.py:776:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/core/has_props.py:789:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/has_props.py:789:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/core/has_props.py:792:24: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/has_props.py:792:24: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/core/json_encoder.py:186:43: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/json_encoder.py:186:43: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/core/property/any.py:82:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/property/any.py:82:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/core/property/container.py:124:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/property/container.py:124:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/core/property/container.py:151:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/property/container.py:151:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/core/property/dataspec.py:471:37: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/property/dataspec.py:471:37: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/core/property/pd.py:30:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/property/pd.py:30:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/core/property/struct.py:70:87: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/property/struct.py:70:87: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/core/property_mixins.py:374:41: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/property_mixins.py:374:41: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/core/serialization.py:578:62: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/serialization.py:578:62: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/core/serialization.py:717:33: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/serialization.py:717:33: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/core/types.py:58:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/core/types.py:58:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/document/document.py:390:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/document/document.py:390:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/document/document.py:433:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/document/document.py:433:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/document/events.py:124:47: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/document/events.py:124:47: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/document/json.py:53:26: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/document/json.py:53:26: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/embed/bundle.py:170:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/embed/bundle.py:170:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/embed/elements.py:137:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/embed/elements.py:137:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/embed/elements.py:168:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/embed/elements.py:168:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/embed/standalone.py:108:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/embed/standalone.py:108:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/embed/standalone.py:137:79: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/embed/standalone.py:137:79: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/embed/standalone.py:144:89: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/embed/standalone.py:144:89: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/embed/standalone.py:151:90: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/embed/standalone.py:151:90: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/embed/standalone.py:248:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/embed/standalone.py:248:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/embed/standalone.py:255:61: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/embed/standalone.py:255:61: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/embed/util.py:401:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/embed/util.py:401:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/io/export.py:123:38: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/io/export.py:123:38: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/io/export.py:442:17: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/io/export.py:442:17: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/io/export.py:446:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/io/export.py:446:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/io/export.py:503:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/io/export.py:503:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/io/notebook.py:330:93: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/io/notebook.py:330:93: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/io/util.py:110:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/io/util.py:110:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/io/util.py:129:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/io/util.py:129:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/layouts.py:194:40: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/layouts.py:194:40: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/layouts.py:303:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/layouts.py:303:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/model/util.py:41:20: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/model/util.py:41:20: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/models/annotations/arrows.py:71:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/models/annotations/arrows.py:71:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/models/annotations/html/toolbars.py:38:39: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/models/annotations/html/toolbars.py:38:39: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/models/callbacks.py:224:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/models/callbacks.py:224:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/models/callbacks.py:225:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/models/callbacks.py:225:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/models/graphs.py:85:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/models/graphs.py:85:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/models/renderers/graph_renderer.py:44:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/models/renderers/graph_renderer.py:44:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/models/tickers.py:322:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/models/tickers.py:322:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/models/tools.py:910:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/models/tools.py:910:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/models/ui/dialogs.py:48:22: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/models/ui/dialogs.py:48:22: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/plotting/_renderer.py:283:28: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/plotting/_renderer.py:283:28: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/plotting/_renderer.py:287:28: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/plotting/_renderer.py:287:28: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/plotting/_tools.py:71:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/plotting/_tools.py:71:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/resources.py:247:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/resources.py:247:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/resources.py:342:40: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/resources.py:342:40: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/resources.py:640:36: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/resources.py:640:36: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/server/tornado.py:397:51: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/server/tornado.py:397:51: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/server/tornado.py:431:38: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/server/tornado.py:431:38: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/server/tornado.py:432:86: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/server/tornado.py:432:86: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/server/tornado.py:449:82: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/server/tornado.py:449:82: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/server/tornado.py:452:78: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/server/tornado.py:452:78: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/server/tornado.py:458:67: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/server/tornado.py:458:67: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/server/tornado.py:653:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/server/tornado.py:653:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/util/compiler.py:338:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/util/compiler.py:338:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/util/compiler.py:512:26: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/util/compiler.py:512:26: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/util/datatypes.py:73:49: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/util/datatypes.py:73:49: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/util/logconfig.py:58:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/util/logconfig.py:58:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/util/serialization.py:339:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/util/serialization.py:339:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/bokeh/util/token.py:295:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/bokeh/util/token.py:295:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/typings/IPython/core/history.pyi:5:89: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/typings/IPython/core/history.pyi:5:89: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- src/typings/bs4.pyi:6:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ src/typings/bs4.pyi:6:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/integration/models/test_plot.py:44:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/integration/models/test_plot.py:44:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/integration/widgets/tables/test_cell_editors.py:101:13: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/integration/widgets/tables/test_cell_editors.py:101:13: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/integration/widgets/tables/test_cell_editors.py:286:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/integration/widgets/tables/test_cell_editors.py:286:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/integration/widgets/tables/test_cell_editors.py:59:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/integration/widgets/tables/test_cell_editors.py:59:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/integration/widgets/tables/test_cell_editors.py:79:13: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/integration/widgets/tables/test_cell_editors.py:79:13: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/support/plugins/bokeh_server.py:103:66: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/support/plugins/bokeh_server.py:103:66: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/support/plugins/bokeh_server.py:86:48: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/support/plugins/bokeh_server.py:86:48: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/support/plugins/file_server.py:64:42: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/support/plugins/file_server.py:64:42: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/support/plugins/ipython.py:45:39: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/support/plugins/ipython.py:45:39: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/support/plugins/networkx.py:45:34: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/support/plugins/networkx.py:45:34: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/support/plugins/selenium.py:130:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/support/plugins/selenium.py:130:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/support/util/compare.py:55:37: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/support/util/compare.py:55:37: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/support/util/selenium.py:320:30: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/support/util/selenium.py:320:30: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/core/property/test_bases.py:156:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/core/property/test_bases.py:156:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/core/property/test_bases.py:188:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/core/property/test_bases.py:188:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/core/property/test_bases.py:205:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/core/property/test_bases.py:205:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/core/test_properties.py:285:62: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/core/test_properties.py:285:62: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/core/test_properties.py:299:62: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/core/test_properties.py:299:62: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/document/test_callbacks__document.py:179:52: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/document/test_callbacks__document.py:179:52: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/document/test_document.py:1072:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/document/test_document.py:1072:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/document/test_document.py:1074:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/document/test_document.py:1074:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/document/test_document.py:794:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/document/test_document.py:794:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/embed/test_standalone.py:288:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/embed/test_standalone.py:288:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/io/test_saving.py:63:41: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/io/test_saving.py:63:41: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/models/test_sources.py:748:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/models/test_sources.py:748:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/plotting/test__decorators.py:43:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/plotting/test__decorators.py:43:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/plotting/test__renderer.py:115:5: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/plotting/test__renderer.py:115:5: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/plotting/test_figure.py:61:59: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/plotting/test_figure.py:61:59: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/server/views/test_multi_root_static_handler.py:45:50: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/server/views/test_multi_root_static_handler.py:45:50: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/server/views/test_ws.py:47:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/server/views/test_ws.py:47:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/server/views/test_ws.py:52:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/server/views/test_ws.py:52:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/server/views/test_ws.py:53:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/server/views/test_ws.py:53:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/server/views/test_ws.py:54:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/server/views/test_ws.py:54:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/test_client_server.py:670:58: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/test_client_server.py:670:58: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/test_objects.py:238:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/test_objects.py:238:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/test_resources.py:81:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/test_resources.py:81:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tests/unit/bokeh/test_tile_providers.py:79:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tests/unit/bokeh/test_tile_providers.py:79:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
```

</p>
</details>
<details><summary>zulip (+179, -179)</summary>
<p>

```diff
- analytics/lib/counts.py:111:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ analytics/lib/counts.py:111:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- analytics/lib/counts.py:255:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ analytics/lib/counts.py:255:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- analytics/management/commands/populate_analytics_db.py:71:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ analytics/management/commands/populate_analytics_db.py:71:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- analytics/views/stats.py:108:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ analytics/views/stats.py:108:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- analytics/views/stats.py:413:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ analytics/views/stats.py:413:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- corporate/lib/stripe.py:1069:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ corporate/lib/stripe.py:1069:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- corporate/lib/stripe.py:1074:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ corporate/lib/stripe.py:1074:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- corporate/lib/stripe.py:286:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ corporate/lib/stripe.py:286:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- corporate/lib/stripe.py:603:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ corporate/lib/stripe.py:603:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- corporate/lib/stripe.py:707:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ corporate/lib/stripe.py:707:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- corporate/models.py:258:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ corporate/models.py:258:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- corporate/tests/test_stripe.py:1706:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ corporate/tests/test_stripe.py:1706:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- corporate/tests/test_stripe.py:2482:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ corporate/tests/test_stripe.py:2482:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- corporate/tests/test_stripe.py:4299:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ corporate/tests/test_stripe.py:4299:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- corporate/tests/test_stripe.py:4599:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ corporate/tests/test_stripe.py:4599:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- corporate/tests/test_stripe.py:742:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ corporate/tests/test_stripe.py:742:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- scripts/lib/check_rabbitmq_queue.py:65:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ scripts/lib/check_rabbitmq_queue.py:65:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- scripts/lib/clean_node_cache.py:3:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ scripts/lib/clean_node_cache.py:3:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- scripts/lib/setup_venv.py:199:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ scripts/lib/setup_venv.py:199:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- scripts/lib/sharding.py:29:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ scripts/lib/sharding.py:29:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tools/documentation_crawler/documentation_crawler/spiders/common/spiders.py:108:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tools/documentation_crawler/documentation_crawler/spiders/common/spiders.py:108:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tools/lib/provision.py:290:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tools/lib/provision.py:290:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- tools/lib/template_parser.py:457:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ tools/lib/template_parser.py:457:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/actions/invites.py:455:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/actions/invites.py:455:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/actions/message_edit.py:1016:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/actions/message_edit.py:1016:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/actions/message_edit.py:737:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/actions/message_edit.py:737:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/actions/message_send.py:344:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/actions/message_send.py:344:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/actions/reactions.py:27:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/actions/reactions.py:27:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/actions/user_settings.py:404:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/actions/user_settings.py:404:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/actions/users.py:382:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/actions/users.py:382:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/data_import/gitter.py:132:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/data_import/gitter.py:132:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/data_import/gitter.py:206:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/data_import/gitter.py:206:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/data_import/rocketchat.py:1254:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/data_import/rocketchat.py:1254:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/data_import/rocketchat.py:91:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/data_import/rocketchat.py:91:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/data_import/slack.py:571:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/data_import/slack.py:571:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/forms.py:377:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/forms.py:377:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/alert_words.py:76:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/alert_words.py:76:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/ccache.py:194:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/ccache.py:194:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/compatibility.py:150:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/compatibility.py:150:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/email_mirror.py:485:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/email_mirror.py:485:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/email_notifications.py:618:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/email_notifications.py:618:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/email_notifications.py:841:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/email_notifications.py:841:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/email_validation.py:10:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/email_validation.py:10:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/event_schema.py:1041:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/event_schema.py:1041:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/event_schema.py:1359:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/event_schema.py:1359:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/events.py:1004:23: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/events.py:1004:23: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/events.py:1489:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/events.py:1489:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/events.py:334:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/events.py:334:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/events.py:466:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/events.py:466:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/events.py:571:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/events.py:571:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/events.py:668:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/events.py:668:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/events.py:877:23: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/events.py:877:23: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/export.py:1255:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/export.py:1255:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/export.py:2127:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/export.py:2127:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/export.py:282:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/export.py:282:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/import_realm.py:1090:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/import_realm.py:1090:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/import_realm.py:1385:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/import_realm.py:1385:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/import_realm.py:1613:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/import_realm.py:1613:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/import_realm.py:682:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/import_realm.py:682:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/import_realm.py:802:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/import_realm.py:802:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/integrations.py:100:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/integrations.py:100:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/integrations.py:163:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/integrations.py:163:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/integrations.py:681:21: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/integrations.py:681:21: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/markdown/__init__.py:1878:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/markdown/__init__.py:1878:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/markdown/__init__.py:713:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/markdown/__init__.py:713:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/markdown/__init__.py:746:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/markdown/__init__.py:746:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/markdown/api_arguments_table_generator.py:133:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/markdown/api_arguments_table_generator.py:133:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/markdown/api_arguments_table_generator.py:166:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/markdown/api_arguments_table_generator.py:166:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/message.py:1152:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/message.py:1152:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/message.py:1163:35: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/message.py:1163:35: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/message.py:576:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/message.py:576:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/message.py:720:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/message.py:720:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/narrow.py:278:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/narrow.py:278:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/presence.py:147:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/presence.py:147:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/presence.py:69:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/presence.py:69:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/presence.py:88:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/presence.py:88:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/push_notifications.py:1049:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/push_notifications.py:1049:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/push_notifications.py:155:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/push_notifications.py:155:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/push_notifications.py:227:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/push_notifications.py:227:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/push_notifications.py:580:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/push_notifications.py:580:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/request.py:381:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/request.py:381:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/request.py:465:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/request.py:465:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/rest.py:190:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/rest.py:190:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/retention.py:365:32: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/retention.py:365:32: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/scim.py:109:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/scim.py:109:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/scim.py:211:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/scim.py:211:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/scim.py:278:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/scim.py:278:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/sessions.py:66:54: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/sessions.py:66:54: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/templates.py:135:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/templates.py:135:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/test_classes.py:879:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/test_classes.py:879:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/test_fixtures.py:183:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/test_fixtures.py:183:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/test_helpers.py:390:74: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/test_helpers.py:390:74: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/test_helpers.py:398:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/test_helpers.py:398:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/test_helpers.py:538:28: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/test_helpers.py:538:28: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/transfer.py:19:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/transfer.py:19:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/url_preview/preview.py:33:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/url_preview/preview.py:33:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/users.py:122:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/users.py:122:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/users.py:216:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/users.py:216:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/users.py:610:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/users.py:610:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/lib/webhooks/common.py:196:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/lib/webhooks/common.py:196:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/management/commands/convert_gitter_data.py:46:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/management/commands/convert_gitter_data.py:46:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/management/commands/delete_realm.py:59:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/management/commands/delete_realm.py:59:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/middleware.py:382:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/middleware.py:382:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/migrations/0206_stream_rendered_description.py:9:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/migrations/0206_stream_rendered_description.py:9:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/migrations/0239_usermessage_copy_id_to_bigint_id.py:38:8: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/migrations/0239_usermessage_copy_id_to_bigint_id.py:38:8: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/models.py:2500:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/models.py:2500:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/models.py:2582:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/models.py:2582:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/models.py:4052:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/models.py:4052:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/models.py:4679:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/models.py:4679:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/openapi/markdown_extension.py:214:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/openapi/markdown_extension.py:214:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/openapi/openapi.py:408:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/openapi/openapi.py:408:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/openapi/openapi.py:413:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/openapi/openapi.py:413:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/openapi/openapi.py:496:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/openapi/openapi.py:496:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/openapi/python_examples.py:433:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/openapi/python_examples.py:433:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_auth_backends.py:3822:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_auth_backends.py:3822:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_bots.py:1499:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_bots.py:1499:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_decorators.py:119:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_decorators.py:119:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_decorators.py:126:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_decorators.py:126:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_decorators.py:133:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_decorators.py:133:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_docs.py:304:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_docs.py:304:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_docs.py:445:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_docs.py:445:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_home.py:1186:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_home.py:1186:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_home.py:1200:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_home.py:1200:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_home.py:1212:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_home.py:1212:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_home.py:272:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_home.py:272:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_i18n.py:45:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_i18n.py:45:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_i18n.py:72:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_i18n.py:72:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_invite.py:1014:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_invite.py:1014:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_link_embed.py:909:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_link_embed.py:909:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_mattermost_importer.py:232:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_mattermost_importer.py:232:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_mattermost_importer.py:430:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_mattermost_importer.py:430:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_message_flags.py:1042:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_message_flags.py:1042:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_message_topics.py:42:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_message_topics.py:42:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_openapi.py:207:14: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_openapi.py:207:14: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_openapi.py:442:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_openapi.py:442:19: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_openapi.py:528:23: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_openapi.py:528:23: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_rocketchat_importer.py:855:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_rocketchat_importer.py:855:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_settings.py:27:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_settings.py:27:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_subs.py:3104:34: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_subs.py:3104:34: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_tornado.py:88:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_tornado.py:88:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tests/test_urls.py:40:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tests/test_urls.py:40:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tornado/event_queue.py:1162:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tornado/event_queue.py:1162:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tornado/event_queue.py:116:45: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tornado/event_queue.py:116:45: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tornado/event_queue.py:1174:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tornado/event_queue.py:1174:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tornado/event_queue.py:1285:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tornado/event_queue.py:1285:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tornado/event_queue.py:1355:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tornado/event_queue.py:1355:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tornado/event_queue.py:1374:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tornado/event_queue.py:1374:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tornado/event_queue.py:1389:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tornado/event_queue.py:1389:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tornado/event_queue.py:245:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tornado/event_queue.py:245:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tornado/handlers.py:207:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tornado/handlers.py:207:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/tornado/views.py:62:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/tornado/views.py:62:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/auth.py:1244:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/auth.py:1244:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/auth.py:213:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/auth.py:213:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/auth.py:685:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/auth.py:685:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/custom_profile_fields.py:230:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/custom_profile_fields.py:230:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/documentation.py:297:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/documentation.py:297:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/home.py:116:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/home.py:116:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/home.py:136:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/home.py:136:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/message_fetch.py:192:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/message_fetch.py:192:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/message_send.py:149:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/message_send.py:149:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/presence.py:76:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/presence.py:76:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/realm.py:269:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/realm.py:269:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/registration.py:316:23: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/registration.py:316:23: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/registration.py:525:23: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/registration.py:525:23: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/registration.py:554:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/registration.py:554:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/scheduled_messages.py:123:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/scheduled_messages.py:123:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/scheduled_messages.py:75:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/scheduled_messages.py:75:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/streams.py:930:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/streams.py:930:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/typing.py:35:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/typing.py:35:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/user_settings.py:81:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/user_settings.py:81:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/views/zephyr.py:65:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/views/zephyr.py:65:7: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/worker/queue_processors.py:536:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/worker/queue_processors.py:536:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zerver/worker/queue_processors.py:961:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zerver/worker/queue_processors.py:961:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zilencer/views.py:105:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zilencer/views.py:105:11: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zproject/backends.py:1042:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zproject/backends.py:1042:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zproject/backends.py:1545:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zproject/backends.py:1545:15: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zproject/default_settings.py:140:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zproject/default_settings.py:140:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zproject/default_settings.py:164:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zproject/default_settings.py:164:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- zproject/default_settings.py:482:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...`
+ zproject/default_settings.py:482:3: TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| TD002 | 892 | 446 | 446 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.0±0.07ms     5.8 MB/sec    1.00      7.0±0.04ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1465.9±2.53µs    11.4 MB/sec    1.00   1465.8±3.16µs    11.4 MB/sec
formatter/numpy/globals.py                 1.00    160.9±0.32µs    18.3 MB/sec    1.00    160.4±0.99µs    18.4 MB/sec
formatter/pydantic/types.py                1.02      3.5±0.01ms     7.3 MB/sec    1.00      3.4±0.01ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.08     15.0±0.05ms     2.7 MB/sec    1.00     13.9±0.18ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.6±0.01ms     4.6 MB/sec    1.00      3.5±0.02ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.03    374.9±2.99µs     7.9 MB/sec    1.00    364.0±3.29µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.06      6.5±0.10ms     3.9 MB/sec    1.00      6.1±0.04ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.16      8.2±0.04ms     4.9 MB/sec    1.00      7.1±0.08ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.11   1642.8±3.07µs    10.1 MB/sec    1.00   1474.1±4.96µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.09    169.1±0.56µs    17.4 MB/sec    1.00    155.6±0.16µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.13      3.6±0.01ms     7.1 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.3±0.02ms     5.6 MB/sec    1.01      7.4±0.05ms     5.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   1500.1±4.77µs    11.1 MB/sec    1.02   1527.4±9.89µs    10.9 MB/sec
formatter/numpy/globals.py                 1.00    164.2±1.58µs    18.0 MB/sec    1.01    165.5±1.14µs    17.8 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.03ms     7.3 MB/sec    1.02      3.6±0.02ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.1±0.07ms     2.9 MB/sec    1.01     14.1±0.10ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.02ms     4.8 MB/sec    1.00      3.5±0.02ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    390.3±2.67µs     7.6 MB/sec    1.01    396.0±3.23µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.10ms     4.1 MB/sec    1.00      6.1±0.04ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.02      7.5±0.37ms     5.5 MB/sec    1.00      7.3±0.05ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1485.6±20.63µs    11.2 MB/sec    1.01   1494.9±4.82µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.2±0.79µs    18.0 MB/sec    1.01    166.1±1.88µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     7.9 MB/sec    1.00      3.2±0.01ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @MichaReiser on 2023-06-06 15:32_

@evanrittenhouse do you want to take a look at these changes? You know the implementation best

---

_@evanrittenhouse reviewed on 2023-06-06 15:46_

So the `trimmed` variable, if there's a `(` but no `)` essentially treats the entire rest of the comment as the author. In the case of `# TODO (evan this is some other text`, we can't know if "evan this is some other text" is the author or not. 

We should probably treat `author_end` as the next whitespace character to restrict the author name to a word if we're using the `@` delimiter.

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todos/rules/todos.rs`:362 on 2023-06-06 15:53_

So the `trimmed` variable, if there's a `(` but no `)` essentially treats the entire rest of the comment as the author. In the case of `# TODO (evan this is some other text`, we can't know if "evan this is some other text" is the author or not. 

We should probably treat `author_end` as the next whitespace character to restrict the author name to a word if we're using the `@` delimiter. For example, `# TODO @evan this is some other text` should treat `@evan` as the author

---

_@evanrittenhouse reviewed on 2023-06-06 15:54_

---

_@mayrholu reviewed on 2023-06-06 17:04_

---

_Review comment by @mayrholu on `crates/ruff/src/rules/flake8_todos/rules/todos.rs`:362 on 2023-06-06 17:04_

That does make sense to me. 👍🏻 should I change that (and the line above my addition) or will you do it after the pull-request?

---

_@evanrittenhouse reviewed on 2023-06-06 17:13_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todos/rules/todos.rs`:362 on 2023-06-06 17:13_

I'd prefer changing it now so that we don't have any false positives!

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todos/rules/todos.rs`:362 on 2023-06-06 19:10_

In this case, we do need to find the whitespace. To generalize across all whitespace, we can do something like `TextSize::try_from(trimmed.find(char::is_whitespace))` to find the offset of the first whitespace in `trimmed`

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todos/rules/todos.rs`:359 on 2023-06-06 19:10_

I think this one is actually fine, we don't need to change this line. The author block should be treated as everything between parentheses - if there's no parenthesis, we can't assume that the author is a single word.

---

_@evanrittenhouse reviewed on 2023-06-06 19:10_

---

_Review comment by @mayrholu on `crates/ruff/src/rules/flake8_todos/rules/todos.rs`:362 on 2023-06-06 19:16_

ok, so `TextSize::try_from(trimmed.find(char::is_whitespace)).unwrap()`? (Sorry, I am not a rust developer)

---

_@mayrholu reviewed on 2023-06-06 19:16_

---

_Comment by @mayrholu on 2023-06-06 20:12_

 @evanrittenhouse Sorry for the many corrections. I think it finally works as expected. 

---

_Comment by @evanrittenhouse on 2023-06-06 21:36_

Really sorry, thinking about this more I think we need to check for a colon or a white space, whichever comes first. Note that this will not allow cases like "@evan rittenhouse". 

For pseudocode I'm thinking something like "find an @ sign. Find the first letter after that sign. Continue iterating until you find either a whitespace or a colon, whichever comes first. `author_end` should be just before that whitespace/colon".

@MichaReiser does that make sense? Not sure how we could use multi-word author names and maintain the optional colon check without iterating through the entire line first, then iterating again for the other static errors. 

Again, sorry for the confusion - bit of a crazy day. Appreciate your contribution

---

_Comment by @evanrittenhouse on 2023-06-07 00:56_

@mayrholu if you want to give me edit privileges on the PR, I can make the change as well. I believe you'll still get credited in GH - I feel bad making you do the extra work, so entirely your call. The change should basically be adding `trimmed.find(|c| c.is_whitespace() || c == ':')` though. I would ask that you please create tests for all permutations of the values though: 
```
# TODO @evan test
# TODO @evan: test
# TODO @ evan test
# TODO @ evan: test

(and any others you can think of!)
```

---

_Comment by @mayrholu on 2023-06-07 05:51_

@evanrittenhouse I think you should already have permissions to edit?! 
I could add your proposed changes myself, though it won't be before next week... Anyway, thanks for your feedback and input on my PR.

---

_Comment by @evanrittenhouse on 2023-06-07 17:13_

I'm not sure I do because I'm not a maintainer 😬 

If you'd like to wait, that's totally fine! 

If not, I can create a new PR to fix it. 

---

_Comment by @mayrholu on 2023-06-08 06:11_

@evanrittenhouse I added you to my forked repo. 

---

_Review requested from @evanrittenhouse by @mayrholu on 2023-06-12 12:09_

---

_Comment by @mayrholu on 2023-06-15 11:47_

Hi @evanrittenhouse, I think now it should work as expected. Could you please have a look at it? Thanks!

---

_Comment by @evanrittenhouse on 2023-06-15 12:47_

That looks good! Thanks :)

---

_Comment by @evanrittenhouse on 2023-06-15 12:48_

I think @charliermarsh has to approve this - I don't have permissions. Charlie, feel free to double check as well

---

_Renamed from "Update flake8-todos "Missing Author" rule TD-002" to "Update flake8-todos "Missing Author" rule TD002" by @charliermarsh on 2023-06-22 20:38_

---

_Renamed from "Update flake8-todos "Missing Author" rule TD002" to "Allow `@Author` format for "Missing Author" rule in `flake8-todos`" by @charliermarsh on 2023-06-22 20:38_

---

_Comment by @charliermarsh on 2023-06-22 20:39_

Thank you.

---

_Merged by @charliermarsh on 2023-06-22 20:53_

---

_Closed by @charliermarsh on 2023-06-22 20:53_

---
