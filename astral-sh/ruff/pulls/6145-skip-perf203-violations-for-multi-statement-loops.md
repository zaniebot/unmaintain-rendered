```yaml
number: 6145
title: "Skip `PERF203` violations for multi-statement loops"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/perf203
created_at: 2023-07-28T04:46:25Z
updated_at: 2023-07-28T05:11:46Z
url: https://github.com/astral-sh/ruff/pull/6145
synced_at: 2026-01-12T03:23:56Z
```

# Skip `PERF203` violations for multi-statement loops

---

_Pull request opened by @charliermarsh on 2023-07-28 04:46_

Closes https://github.com/astral-sh/ruff/issues/5858.

---

_Label `rule` added by @charliermarsh on 2023-07-28 04:46_

---

_Merged by @charliermarsh on 2023-07-28 04:55_

---

_Closed by @charliermarsh on 2023-07-28 04:55_

---

_Branch deleted on 2023-07-28 04:55_

---

_Comment by @github-actions[bot] on 2023-07-28 04:59_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -171, 0 error(s))

<details><summary>airflow (+0, -114)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/api_connexion/endpoints/user_endpoint.py#L161'>airflow/api_connexion/endpoints/user_endpoint.py:161:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/cli/commands/connection_command.py#L334'>airflow/cli/commands/connection_command.py:334:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/cli/commands/variable_command.py#L125'>airflow/cli/commands/variable_command.py:125:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/configuration.py#L1355'>airflow/configuration.py:1355:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/configuration.py#L1526'>airflow/configuration.py:1526:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/configuration.py#L1582'>airflow/configuration.py:1582:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/configuration.py#L1642'>airflow/configuration.py:1642:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/dag_processing/manager.py#L202'>airflow/dag_processing/manager.py:202:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/dag_processing/manager.py#L268'>airflow/dag_processing/manager.py:268:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/dag_processing/manager.py#L669'>airflow/dag_processing/manager.py:669:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/dag_processing/processor.py#L539'>airflow/dag_processing/processor.py:539:21:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/dag_processing/processor.py#L564'>airflow/dag_processing/processor.py:564:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/dag_processing/processor.py#L716'>airflow/dag_processing/processor.py:716:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/datasets/manager.py#L100'>airflow/datasets/manager.py:100:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/executors/local_executor.py#L187'>airflow/executors/local_executor.py:187:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/executors/sequential_executor.py#L79'>airflow/executors/sequential_executor.py:79:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/jobs/backfill_job_runner.py#L646'>airflow/jobs/backfill_job_runner.py:646:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/jobs/scheduler_job_runner.py#L401'>airflow/jobs/scheduler_job_runner.py:401:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/jobs/triggerer_job_runner.py#L667'>airflow/jobs/triggerer_job_runner.py:667:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/jobs/triggerer_job_runner.py#L674'>airflow/jobs/triggerer_job_runner.py:674:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/models/abstractoperator.py#L700'>airflow/models/abstractoperator.py:700:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/models/abstractoperator.py#L709'>airflow/models/abstractoperator.py:709:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/models/abstractoperator.py#L729'>airflow/models/abstractoperator.py:729:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/models/baseoperator.py#L1335'>airflow/models/baseoperator.py:1335:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/models/baseoperator.py#L1364'>airflow/models/baseoperator.py:1364:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/models/baseoperator.py#L989'>airflow/models/baseoperator.py:989:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/models/dag.py#L1100'>airflow/models/dag.py:1100:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/models/dag.py#L1409'>airflow/models/dag.py:1409:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/models/dag.py#L2746'>airflow/models/dag.py:2746:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/models/dag.py#L753'>airflow/models/dag.py:753:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/models/dagbag.py#L411'>airflow/models/dagbag.py:411:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/models/dagbag.py#L443'>airflow/models/dagbag.py:443:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/models/dagrun.py#L1039'>airflow/models/dagrun.py:1039:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/models/dagrun.py#L1052'>airflow/models/dagrun.py:1052:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/models/dagrun.py#L1161'>airflow/models/dagrun.py:1161:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/models/mappedoperator.py#L90'>airflow/models/mappedoperator.py:90:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/plugins_manager.py#L243'>airflow/plugins_manager.py:243:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/plugins_manager.py#L272'>airflow/plugins_manager.py:272:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/plugins_manager.py#L292'>airflow/plugins_manager.py:292:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/airbyte/hooks/airbyte.py#L71'>airflow/providers/airbyte/hooks/airbyte.py:71:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/amazon/aws/hooks/base_aws.py#L902'>airflow/providers/amazon/aws/hooks/base_aws.py:902:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/amazon/aws/hooks/batch_client.py#L391'>airflow/providers/amazon/aws/hooks/batch_client.py:391:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/amazon/aws/hooks/elasticache_replication_group.py#L215'>airflow/providers/amazon/aws/hooks/elasticache_replication_group.py:215:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/amazon/aws/hooks/sagemaker.py#L263'>airflow/providers/amazon/aws/hooks/sagemaker.py:263:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/amazon/aws/hooks/sagemaker.py#L271'>airflow/providers/amazon/aws/hooks/sagemaker.py:271:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/amazon/aws/hooks/sagemaker.py#L717'>airflow/providers/amazon/aws/hooks/sagemaker.py:717:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/amazon/aws/triggers/batch.py#L176'>airflow/providers/amazon/aws/triggers/batch.py:176:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/amazon/aws/triggers/batch.py#L92'>airflow/providers/amazon/aws/triggers/batch.py:92:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/amazon/aws/triggers/ecs.py#L177'>airflow/providers/amazon/aws/triggers/ecs.py:177:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/amazon/aws/triggers/ecs.py#L205'>airflow/providers/amazon/aws/triggers/ecs.py:205:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/amazon/aws/triggers/emr.py#L87'>airflow/providers/amazon/aws/triggers/emr.py:87:21:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/amazon/aws/triggers/sagemaker.py#L180'>airflow/providers/amazon/aws/triggers/sagemaker.py:180:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/amazon/aws/utils/waiter_with_logging.py#L124'>airflow/providers/amazon/aws/utils/waiter_with_logging.py:124:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/amazon/aws/utils/waiter_with_logging.py#L72'>airflow/providers/amazon/aws/utils/waiter_with_logging.py:72:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/apache/flink/sensors/flink_kubernetes.py#L104'>airflow/providers/apache/flink/sensors/flink_kubernetes.py:104:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/apache/hdfs/hooks/webhdfs.py#L95'>airflow/providers/apache/hdfs/hooks/webhdfs.py:95:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/apache/hive/hooks/hive.py#L324'>airflow/providers/apache/hive/hooks/hive.py:324:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/apache/livy/hooks/livy.py#L558'>airflow/providers/apache/livy/hooks/livy.py:558:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py#L576'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py:576:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/cncf/kubernetes/utils/delete_from.py#L51'>airflow/providers/cncf/kubernetes/utils/delete_from.py:51:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/google/cloud/hooks/compute_ssh.py#L270'>airflow/providers/google/cloud/hooks/compute_ssh.py:270:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/google/cloud/hooks/dataform.py#L95'>airflow/providers/google/cloud/hooks/dataform.py:95:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/google/cloud/hooks/datafusion.py#L120'>airflow/providers/google/cloud/hooks/datafusion.py:120:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/google/cloud/hooks/dataproc.py#L717'>airflow/providers/google/cloud/hooks/dataproc.py:717:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/google/cloud/operators/dataproc.py#L612'>airflow/providers/google/cloud/operators/dataproc.py:612:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/google/cloud/operators/gcs.py#L800'>airflow/providers/google/cloud/operators/gcs.py:800:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/google/cloud/operators/gcs.py#L850'>airflow/providers/google/cloud/operators/gcs.py:850:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/google/cloud/triggers/cloud_storage_transfer_service.py#L99'>airflow/providers/google/cloud/triggers/cloud_storage_transfer_service.py:99:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/google/suite/transfers/local_to_drive.py#L134'>airflow/providers/google/suite/transfers/local_to_drive.py:134:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/http/hooks/http.py#L379'>airflow/providers/http/hooks/http.py:379:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/jenkins/operators/jenkins_job_trigger.py#L164'>airflow/providers/jenkins/operators/jenkins_job_trigger.py:164:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/microsoft/azure/hooks/data_factory.py#L840'>airflow/providers/microsoft/azure/hooks/data_factory.py:840:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/microsoft/azure/log/wasb_task_handler.py#L164'>airflow/providers/microsoft/azure/log/wasb_task_handler.py:164:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/microsoft/azure/operators/container_instances.py#L337'>airflow/providers/microsoft/azure/operators/container_instances.py:337:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/openlineage/utils/sql.py#L129'>airflow/providers/openlineage/utils/sql.py:129:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/sftp/sensors/sftp.py#L91'>airflow/providers/sftp/sensors/sftp.py:91:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/smtp/hooks/smtp.py#L212'>airflow/providers/smtp/hooks/smtp.py:212:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/smtp/hooks/smtp.py#L86'>airflow/providers/smtp/hooks/smtp.py:86:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/snowflake/hooks/snowflake_sql_api.py#L228'>airflow/providers/snowflake/hooks/snowflake_sql_api.py:228:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/providers/snowflake/operators/snowflake.py#L531'>airflow/providers/snowflake/operators/snowflake.py:531:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/sensors/base.py#L239'>airflow/sensors/base.py:239:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/serialization/serialized_objects.py#L1085'>airflow/serialization/serialized_objects.py:1085:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/serialization/serialized_objects.py#L650'>airflow/serialization/serialized_objects.py:650:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/serialization/serialized_objects.py#L799'>airflow/serialization/serialized_objects.py:799:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/utils/db.py#L1011'>airflow/utils/db.py:1011:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/utils/email.py#L272'>airflow/utils/email.py:272:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/www/views.py#L4805'>airflow/www/views.py:4805:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/airflow/www/views.py#L5254'>airflow/www/views.py:5254:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/dev/assign_cherry_picked_prs_with_milestone.py#L304'>dev/assign_cherry_picked_prs_with_milestone.py:304:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L1159'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:1159:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/dev/breeze/src/airflow_breeze/commands/setup_commands.py#L295'>dev/breeze/src/airflow_breeze/commands/setup_commands.py:295:21:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py#L426'>dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py:426:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py#L486'>dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py:486:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/dev/breeze/src/airflow_breeze/utils/publish_docs_helpers.py#L75'>dev/breeze/src/airflow_breeze/utils/publish_docs_helpers.py:75:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/dev/mypy/plugin/decorators.py#L72'>dev/mypy/plugin/decorators.py:72:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/dev/prepare_release_issue.py#L284'>dev/prepare_release_issue.py:284:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/dev/prepare_release_issue.py#L315'>dev/prepare_release_issue.py:315:21:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/dev/provider_packages/prepare_provider_packages.py#L1195'>dev/provider_packages/prepare_provider_packages.py:1195:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/dev/provider_packages/prepare_provider_packages.py#L1236'>dev/provider_packages/prepare_provider_packages.py:1236:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/dev/stats/get_important_pr_candidates.py#L136'>dev/stats/get_important_pr_candidates.py:136:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/docs/exts/provider_yaml_utils.py#L70'>docs/exts/provider_yaml_utils.py:70:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/kubernetes_tests/test_base.py#L177'>kubernetes_tests/test_base.py:177:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/scripts/in_container/run_provider_yaml_files_check.py#L101'>scripts/in_container/run_provider_yaml_files_check.py:101:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/scripts/in_container/run_provider_yaml_files_check.py#L250'>scripts/in_container/run_provider_yaml_files_check.py:250:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/scripts/in_container/run_provider_yaml_files_check.py#L291'>scripts/in_container/run_provider_yaml_files_check.py:291:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/scripts/in_container/verify_providers.py#L207'>scripts/in_container/verify_providers.py:207:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/scripts/tools/list-integrations.py#L56'>scripts/tools/list-integrations.py:56:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/tests/cli/commands/_common_cli_classes.py#L116'>tests/cli/commands/_common_cli_classes.py:116:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/tests/cli/commands/_common_cli_classes.py#L138'>tests/cli/commands/_common_cli_classes.py:138:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/tests/core/test_logging_config.py#L118'>tests/core/test_logging_config.py:118:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/tests/dag_processing/test_job_runner.py#L848'>tests/dag_processing/test_job_runner.py:848:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/tests/operators/test_python.py#L97'>tests/operators/test_python.py:97:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/tests/providers/http/hooks/test_http.py#L87'>tests/providers/http/hooks/test_http.py:87:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/apache/airflow/blob/7ba7fb1173e55c24c94fe01f0742fd00cd9c0d82/tests/serialization/test_serde.py#L308'>tests/serialization/test_serde.py:308:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
</pre>

</p>
</details>
<details><summary>bokeh (+0, -12)</summary>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/examples/topics/geo/choropleth.py#L38'>examples/topics/geo/choropleth.py:38:5:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/release/build.py#L165'>release/build.py:165:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/release/git.py#L56'>release/git.py:56:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/release/git.py#L60'>release/git.py:60:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/src/bokeh/client/connection.py#L344'>src/bokeh/client/connection.py:344:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/src/bokeh/client/connection.py#L357'>src/bokeh/client/connection.py:357:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/src/bokeh/command/subcommands/__init__.py#L69'>src/bokeh/command/subcommands/__init__.py:69:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/src/bokeh/core/has_props.py#L650'>src/bokeh/core/has_props.py:650:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/src/bokeh/sphinxext/bokeh_plot.py#L298'>src/bokeh/sphinxext/bokeh_plot.py:298:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/src/bokeh/util/compiler.py#L395'>src/bokeh/util/compiler.py:395:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/tests/unit/bokeh/command/subcommands/test_serve.py#L525'>tests/unit/bokeh/command/subcommands/test_serve.py:525:21:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/tests/unit/bokeh/models/test_defaults.py#L41'>tests/unit/bokeh/models/test_defaults.py:41:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
</pre>

</p>
</details>
<details><summary>zulip (+0, -45)</summary>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/analytics/management/commands/check_analytics_state.py#L51'>analytics/management/commands/check_analytics_state.py:51:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/scripts/lib/zulip_tools.py#L320'>scripts/lib/zulip_tools.py:320:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/scripts/lib/zulip_tools.py#L54'>scripts/lib/zulip_tools.py:54:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/tools/lib/html_branches.py#L86'>tools/lib/html_branches.py:86:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/tools/lib/template_parser.py#L238'>tools/lib/template_parser.py:238:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/actions/default_streams.py#L51'>zerver/actions/default_streams.py:51:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/actions/typing.py#L54'>zerver/actions/typing.py:54:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/data_import/slack.py#L899'>zerver/data_import/slack.py:899:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/lib/addressee.py#L21'>zerver/lib/addressee.py:21:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/lib/addressee.py#L32'>zerver/lib/addressee.py:32:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/lib/events.py#L1572'>zerver/lib/events.py:1572:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/lib/import_realm.py#L380'>zerver/lib/import_realm.py:380:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/lib/import_realm.py#L698'>zerver/lib/import_realm.py:698:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/lib/management.py#L31'>zerver/lib/management.py:31:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/lib/markdown/__init__.py#L2343'>zerver/lib/markdown/__init__.py:2343:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/lib/markdown/api_arguments_table_generator.py#L80'>zerver/lib/markdown/api_arguments_table_generator.py:80:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/lib/markdown/tabbed_sections.py#L152'>zerver/lib/markdown/tabbed_sections.py:152:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/lib/push_notifications.py#L236'>zerver/lib/push_notifications.py:236:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/lib/subscription_info.py#L322'>zerver/lib/subscription_info.py:322:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/lib/test_runner.py#L153'>zerver/lib/test_runner.py:153:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/lib/users.py#L362'>zerver/lib/users.py:362:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/lib/users.py#L367'>zerver/lib/users.py:367:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/management/commands/bulk_change_user_name.py#L33'>zerver/management/commands/bulk_change_user_name.py:33:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/management/commands/change_password.py#L57'>zerver/management/commands/change_password.py:57:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/management/commands/compilemessages.py#L122'>zerver/management/commands/compilemessages.py:122:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/management/commands/enqueue_file.py#L20'>zerver/management/commands/enqueue_file.py:20:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/management/commands/export_usermessage_batch.py#L32'>zerver/management/commands/export_usermessage_batch.py:32:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/management/commands/export_usermessage_batch.py#L38'>zerver/management/commands/export_usermessage_batch.py:38:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/management/commands/makemessages.py#L276'>zerver/management/commands/makemessages.py:276:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/migrations/0260_missed_message_addresses_from_redis_to_db.py#L70'>zerver/migrations/0260_missed_message_addresses_from_redis_to_db.py:70:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/migrations/0373_fix_deleteduser_dummies.py#L64'>zerver/migrations/0373_fix_deleteduser_dummies.py:64:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/migrations/0401_migrate_old_realm_reactivation_links.py#L50'>zerver/migrations/0401_migrate_old_realm_reactivation_links.py:50:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/models.py#L4728'>zerver/models.py:4728:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/openapi/test_curl_examples.py#L103'>zerver/openapi/test_curl_examples.py:103:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/tests/test_markdown.py#L486'>zerver/tests/test_markdown.py:486:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/tests/test_openapi.py#L568'>zerver/tests/test_openapi.py:568:21:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/views/invite.py#L208'>zerver/views/invite.py:208:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/views/invite.py#L80'>zerver/views/invite.py:80:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/views/streams.py#L1071'>zerver/views/streams.py:1071:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/worker/queue_processors.py#L754'>zerver/worker/queue_processors.py:754:17:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zerver/worker/queue_processors.py#L976'>zerver/worker/queue_processors.py:976:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zilencer/views.py#L391'>zilencer/views.py:391:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zproject/backends.py#L1424'>zproject/backends.py:1424:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zproject/backends.py#L1430'>zproject/backends.py:1430:9:</a> PERF203 `try`-`except` within a loop incurs performance overhead
- <a href='https://github.com/zulip/zulip/blob/875d564f2d8697cdd10a0a5da65b6a78327ad2c3/zproject/backends.py#L901'>zproject/backends.py:901:13:</a> PERF203 `try`-`except` within a loop incurs performance overhead
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PERF203 | 171 | 0 | 171 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.14ms     4.1 MB/sec    1.00      9.9±0.14ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1930.1±21.94µs     8.6 MB/sec    1.00  1938.3±20.23µs     8.6 MB/sec
formatter/numpy/globals.py                 1.02    211.4±2.42µs    14.0 MB/sec    1.00    207.3±6.56µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.04ms     6.1 MB/sec    1.01      4.2±0.03ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.2±0.19ms     3.1 MB/sec    1.01     13.3±0.17ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.05ms     5.0 MB/sec    1.01      3.3±0.03ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    456.2±1.25µs     6.5 MB/sec    1.00    452.1±1.58µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.10ms     4.4 MB/sec    1.03      5.9±0.07ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.10ms     5.8 MB/sec    1.00      7.0±0.12ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1472.0±13.18µs    11.3 MB/sec    1.00  1461.6±28.49µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.2±3.48µs    18.8 MB/sec    1.01    159.2±1.92µs    18.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.07ms     8.3 MB/sec    1.00      3.1±0.05ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.2±0.09ms     4.0 MB/sec    1.00     10.1±0.17ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1965.1±21.56µs     8.5 MB/sec    1.00  1970.9±19.70µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    218.4±6.00µs    13.5 MB/sec    1.02   222.2±16.02µs    13.3 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.05ms     5.9 MB/sec    1.01      4.3±0.06ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.01     13.5±0.12ms     3.0 MB/sec    1.00     13.4±0.11ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.03ms     4.7 MB/sec    1.00      3.5±0.03ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    433.0±4.98µs     6.8 MB/sec    1.00    429.8±5.30µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.08ms     4.2 MB/sec    1.00      6.0±0.07ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.06ms     5.4 MB/sec    1.00      7.5±0.06ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1522.9±16.17µs    10.9 MB/sec    1.00  1509.2±22.36µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    167.6±3.26µs    17.6 MB/sec    1.00    166.1±2.61µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.03ms     7.8 MB/sec    1.00      3.3±0.05ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
