```yaml
number: 3550
title: "[`flake8-bugbear`] Add `no-explicit-stacklevel` (`B028`)"
type: pull_request
state: merged
author: johnor
labels:
  - rule
assignees: []
merged: true
base: main
head: bugbear-b028
created_at: 2023-03-15T20:21:53Z
updated_at: 2023-03-17T19:44:07Z
url: https://github.com/astral-sh/ruff/pull/3550
synced_at: 2026-01-12T15:55:13Z
```

# [`flake8-bugbear`] Add `no-explicit-stacklevel` (`B028`)

---

_@johnor_

Part of #2954.

Tests are from bugbear: https://github.com/PyCQA/flake8-bugbear/blob/main/tests/b028.py

---

_Comment by @github-actions[bot] on 2023-03-15 20:35_

ℹ️ ecosystem check **detected changes**. (+299, -0, 0 error(s))

<details><summary>bokeh (+2, -0)</summary>
<p>

```diff
+ examples/server/app/image_blur.py:15:5: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ tests/support/plugins/selenium.py:127:21: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
```

</p>
</details>
<details><summary>airflow (+297, -0)</summary>
<p>

```diff
+ airflow/cli/commands/connection_command.py:154:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/cli/commands/connection_command.py:213:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/cli/commands/connection_command.py:214:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/cli/commands/dag_command.py:59:5: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/configuration.py:1516:17: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/configuration.py:1518:17: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/configuration.py:1527:17: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/configuration.py:385:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/configuration.py:405:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/configuration.py:463:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/decorators/task_group.py:80:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/jobs/triggerer_job.py:125:17: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/jobs/triggerer_job.py:146:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/kubernetes/pod_generator.py:141:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/kubernetes/pod_generator.py:154:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/kubernetes/pod_generator.py:189:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/kubernetes/pod_generator.py:370:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/kubernetes/pod_generator.py:566:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/logging_config.py:94:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/models/connection.py:175:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/models/connection.py:334:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/models/connection.py:42:5: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/models/mappedoperator.py:162:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/models/param.py:73:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/models/param.py:88:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/models/param.py:94:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/models/xcom.py:799:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/amazon/aws/operators/emr.py:206:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/amazon/aws/operators/emr.py:213:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/amazon/aws/operators/emr.py:322:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/amazon/aws/operators/emr.py:329:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/amazon/aws/operators/emr.py:618:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/amazon/aws/operators/emr.py:627:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/apache/beam/operators/beam.py:174:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/cncf/kubernetes/hooks/kubernetes.py:344:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/cncf/kubernetes/utils/pod_manager.py:238:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/cncf/kubernetes/utils/pod_manager.py:310:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/dbt/cloud/operators/dbt.py:170:17: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/elasticsearch/log/es_task_handler.py:106:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/automl.py:72:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/bigquery.py:101:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/bigquery.py:1164:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/bigquery.py:1213:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/bigquery.py:130:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/bigquery.py:1406:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/bigquery.py:1568:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/bigquery.py:1662:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/bigquery.py:1849:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/bigquery.py:1931:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/bigquery.py:2054:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/bigquery.py:609:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/bigquery.py:789:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/bigquery.py:940:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/bigquery.py:985:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/bigquery_dts.py:66:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/bigtable.py:50:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/cloud_build.py:65:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/cloud_memorystore.py:81:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/cloud_sql.py:101:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/cloud_storage_transfer_service.py:134:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/cloud_storage_transfer_service.py:236:17: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/cloud_storage_transfer_service.py:367:17: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/compute.py:63:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/compute_ssh.py:127:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/datacatalog.py:67:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/dataflow.py:533:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/datafusion.py:74:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/dataplex.py:64:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/dataproc.py:216:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/datastore.py:48:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/dlp.py:96:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/functions.py:53:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/gcs.py:159:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/gdm.py:42:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/kms.py:68:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/kubernetes_engine.py:70:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/kubernetes_engine.py:90:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/kubernetes_engine.py:99:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/life_sciences.py:67:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/natural_language.py:67:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/os_login.py:51:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/pubsub.py:74:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/secret_manager.py:58:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/spanner.py:51:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/speech_to_text.py:58:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/stackdriver.py:47:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/tasks.py:68:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/text_to_speech.py:66:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/translate.py:45:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/vertex_ai/auto_ml.py:81:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/vertex_ai/batch_prediction_job.py:54:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py:58:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/vertex_ai/hyperparameter_tuning_job.py:55:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/video_intelligence.py:62:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/hooks/vision.py:134:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/bigquery.py:1331:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/bigquery.py:1345:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/bigquery.py:1608:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/bigquery.py:1758:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/bigquery.py:1853:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/bigquery.py:1859:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/bigquery.py:1947:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/bigquery.py:2023:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/bigquery.py:2101:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/bigquery.py:2186:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/bigquery.py:2279:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/bigquery.py:2361:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/bigquery.py:2439:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/bigquery.py:2549:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/bigquery.py:2675:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/bigquery.py:835:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/cloud_build.py:200:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/cloud_composer.py:158:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/cloud_composer.py:298:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/cloud_composer.py:396:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/cloud_composer.py:481:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/cloud_composer.py:576:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/cloud_composer.py:696:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py:1047:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py:873:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataflow.py:1017:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataflow.py:1207:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataflow.py:1347:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataflow.py:159:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataflow.py:376:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataflow.py:646:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataflow.py:842:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataform.py:1044:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataform.py:1132:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataform.py:1217:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataform.py:171:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataform.py:253:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataform.py:349:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataform.py:425:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataform.py:505:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataform.py:596:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataform.py:680:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataform.py:775:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataform.py:865:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataform.py:954:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataform.py:96:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/datafusion.py:1015:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/datafusion.py:103:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/datafusion.py:185:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/datafusion.py:264:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/datafusion.py:378:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/datafusion.py:463:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/datafusion.py:558:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/datafusion.py:655:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/datafusion.py:748:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/datafusion.py:861:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataplex.py:118:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataplex.py:233:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataplex.py:339:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataplex.py:427:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataplex.py:529:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataplex.py:643:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/dataproc.py:992:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/datastore.py:105:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/datastore.py:218:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/datastore.py:306:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/datastore.py:373:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/datastore.py:440:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/datastore.py:507:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/datastore.py:572:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/datastore.py:635:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/datastore.py:695:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/gcs.py:138:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/gcs.py:236:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/gcs.py:316:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/gcs.py:999:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/mlengine.py:1066:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/mlengine.py:1226:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/mlengine.py:1484:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/mlengine.py:214:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/mlengine.py:363:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/mlengine.py:433:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/mlengine.py:508:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/mlengine.py:587:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/mlengine.py:698:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/mlengine.py:797:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/mlengine.py:889:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/mlengine.py:977:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/pubsub.py:150:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/pubsub.py:360:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/pubsub.py:495:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/pubsub.py:603:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/pubsub.py:713:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/pubsub.py:800:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/stackdriver.py:121:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/stackdriver.py:220:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/stackdriver.py:312:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/stackdriver.py:410:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/stackdriver.py:499:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/stackdriver.py:606:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/stackdriver.py:709:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/stackdriver.py:803:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/stackdriver.py:899:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/stackdriver.py:990:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/auto_ml.py:576:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/auto_ml.py:647:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/auto_ml.py:82:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/batch_prediction_job.py:218:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/batch_prediction_job.py:325:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/batch_prediction_job.py:404:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/batch_prediction_job.py:504:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/custom_job.py:1272:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/custom_job.py:1396:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/custom_job.py:152:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/dataset.py:174:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/dataset.py:254:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/dataset.py:335:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/dataset.py:415:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/dataset.py:505:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/dataset.py:588:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/dataset.py:89:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/endpoint_service.py:106:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/endpoint_service.py:189:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/endpoint_service.py:288:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/endpoint_service.py:373:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/endpoint_service.py:482:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/endpoint_service.py:572:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/endpoint_service.py:657:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/hyperparameter_tuning_job.py:222:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/hyperparameter_tuning_job.py:336:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/hyperparameter_tuning_job.py:408:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/hyperparameter_tuning_job.py:498:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/model_service.py:178:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/model_service.py:284:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/model_service.py:362:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/operators/vertex_ai/model_service.py:96:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/sensors/bigquery.py:160:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/sensors/bigquery.py:87:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/sensors/cloud_composer.py:73:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/sensors/dataflow.py:181:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/sensors/dataflow.py:270:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/sensors/dataflow.py:359:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/sensors/dataflow.py:90:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/sensors/dataform.py:87:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/sensors/datafusion.py:89:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/sensors/dataplex.py:96:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/sensors/gcs.py:214:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/sensors/gcs.py:277:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/sensors/gcs.py:380:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/sensors/gcs.py:86:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/sensors/pubsub.py:112:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/transfers/adls_to_gcs.py:128:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/transfers/azure_fileshare_to_gcs.py:97:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/transfers/bigquery_to_bigquery.py:112:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/transfers/bigquery_to_gcs.py:135:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/transfers/bigquery_to_mssql.py:111:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/transfers/bigquery_to_mysql.py:115:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/transfers/calendar_to_gcs.py:137:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/transfers/cassandra_to_gcs.py:125:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/transfers/gcs_to_bigquery.py:276:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/transfers/gcs_to_gcs.py:212:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/transfers/gcs_to_local.py:113:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/transfers/gcs_to_sftp.py:133:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/transfers/gdrive_to_gcs.py:89:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/transfers/gdrive_to_local.py:82:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/transfers/local_to_gcs.py:90:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/transfers/s3_to_gcs.py:129:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/transfers/sftp_to_gcs.py:108:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/transfers/sheets_to_gcs.py:88:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/transfers/sql_to_gcs.py:147:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/triggers/bigquery_dts.py:69:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/triggers/cloud_build.py:67:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/triggers/cloud_composer.py:50:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/cloud/triggers/datafusion.py:73:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/common/hooks/discovery_api.py:61:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/google/firebase/hooks/firestore.py:66:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/microsoft/azure/hooks/adx.py:132:17: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/microsoft/azure/hooks/fileshare.py:119:17: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/microsoft/azure/hooks/fileshare.py:142:17: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/microsoft/azure/transfers/azure_blob_to_gcs.py:89:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/microsoft/azure/utils.py:64:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/presto/transfers/gcs_to_presto.py:91:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/slack/utils/__init__.py:48:17: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/snowflake/hooks/snowflake.py:195:17: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/ssh/hooks/ssh.py:367:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/ssh/hooks/ssh.py:432:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/ssh/operators/ssh.py:139:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/tableau/hooks/tableau.py:123:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/trino/transfers/gcs_to_trino.py:94:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/yandex/hooks/yandex.py:100:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers/yandex/operators/yandexcloud_dataproc.py:259:17: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/providers_manager.py:674:17: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/serialization/serialized_objects.py:979:17: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/settings.py:442:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/utils/context.py:205:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/utils/context.py:313:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/www/app.py:102:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ airflow/www/extensions/init_views.py:283:5: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ tests/charts/test_basic_helm_chart.py:370:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ tests/jobs/test_local_task_job.py:901:13: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ tests/providers/amazon/conftest.py:36:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
+ tests/providers/amazon/conftest.py:42:9: B028 No explicit stacklevel keyword argument found. The warn method from the warnings module uses a stacklevel of 1 by default. This will only show a stack trace for the line on which the warn method is called.
```

</p>
</details>

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Review requested from @charliermarsh by @charliermarsh on 2023-03-17 03:50_

---

_Label `rule` added by @charliermarsh on 2023-03-17 03:50_

---

_Comment by @charliermarsh on 2023-03-17 03:50_

Sorry for the delay, will merge this tomorrow!

---

_@MichaReiser approved on 2023-03-17 08:04_

---

_Renamed from "Add flake8-bugbear rule B028" to "[`flake8-bugbear`] Add `no-explicit-stacklevel` (`B028`)" by @charliermarsh on 2023-03-17 19:15_

---

_Merged by @charliermarsh on 2023-03-17 19:20_

---

_Closed by @charliermarsh on 2023-03-17 19:20_

---

_Branch deleted on 2023-03-17 19:22_

---

_Comment by @charliermarsh on 2023-03-17 19:24_

Thank you, great PR :)

---

_@charliermarsh reviewed on 2023-03-17 19:24_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/rules/no_explicit_stacklevel.rs`:37 on 2023-03-17 19:24_

I truncated this message a bit and instead moved the additional context into docs.

---

_Comment by @github-actions[bot] on 2023-03-17 19:28_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+300, -0, 0 error(s))

<details><summary>bokeh (+2, -0)</summary>
<p>

```diff
+ examples/server/app/image_blur.py:15:5: B028 No explicit `stacklevel` keyword argument found
+ tests/support/plugins/selenium.py:127:21: B028 No explicit `stacklevel` keyword argument found
```

</p>
</details>
<details><summary>airflow (+298, -0)</summary>
<p>

```diff
+ airflow/cli/commands/connection_command.py:154:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/cli/commands/connection_command.py:213:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/cli/commands/connection_command.py:214:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/cli/commands/dag_command.py:59:5: B028 No explicit `stacklevel` keyword argument found
+ airflow/configuration.py:1516:17: B028 No explicit `stacklevel` keyword argument found
+ airflow/configuration.py:1518:17: B028 No explicit `stacklevel` keyword argument found
+ airflow/configuration.py:1527:17: B028 No explicit `stacklevel` keyword argument found
+ airflow/configuration.py:385:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/configuration.py:405:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/configuration.py:463:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/decorators/task_group.py:80:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/jobs/triggerer_job.py:126:17: B028 No explicit `stacklevel` keyword argument found
+ airflow/jobs/triggerer_job.py:147:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/kubernetes/pod_generator.py:141:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/kubernetes/pod_generator.py:154:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/kubernetes/pod_generator.py:189:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/kubernetes/pod_generator.py:370:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/kubernetes/pod_generator.py:566:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/logging_config.py:94:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/models/connection.py:175:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/models/connection.py:334:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/models/connection.py:42:5: B028 No explicit `stacklevel` keyword argument found
+ airflow/models/mappedoperator.py:162:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/models/param.py:73:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/models/param.py:88:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/models/param.py:94:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/models/xcom.py:799:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/amazon/aws/operators/emr.py:206:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/amazon/aws/operators/emr.py:213:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/amazon/aws/operators/emr.py:322:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/amazon/aws/operators/emr.py:329:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/amazon/aws/operators/emr.py:618:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/amazon/aws/operators/emr.py:627:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/apache/beam/operators/beam.py:174:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/cncf/kubernetes/hooks/kubernetes.py:344:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/cncf/kubernetes/utils/pod_manager.py:238:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/cncf/kubernetes/utils/pod_manager.py:310:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/dbt/cloud/operators/dbt.py:170:17: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/elasticsearch/log/es_task_handler.py:106:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/automl.py:72:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/bigquery.py:101:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/bigquery.py:1164:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/bigquery.py:1213:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/bigquery.py:130:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/bigquery.py:1406:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/bigquery.py:1568:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/bigquery.py:1662:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/bigquery.py:1849:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/bigquery.py:1931:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/bigquery.py:2054:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/bigquery.py:609:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/bigquery.py:789:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/bigquery.py:940:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/bigquery.py:985:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/bigquery_dts.py:66:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/bigtable.py:50:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/cloud_build.py:65:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/cloud_memorystore.py:81:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/cloud_sql.py:101:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/cloud_storage_transfer_service.py:134:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/cloud_storage_transfer_service.py:236:17: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/cloud_storage_transfer_service.py:367:17: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/compute.py:63:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/compute_ssh.py:127:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/datacatalog.py:67:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/dataflow.py:533:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/datafusion.py:74:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/dataplex.py:64:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/dataproc.py:216:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/datastore.py:48:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/dlp.py:96:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/functions.py:53:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/gcs.py:159:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/gdm.py:42:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/kms.py:68:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/kubernetes_engine.py:70:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/kubernetes_engine.py:90:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/kubernetes_engine.py:99:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/life_sciences.py:67:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/natural_language.py:67:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/os_login.py:51:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/pubsub.py:74:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/secret_manager.py:58:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/spanner.py:51:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/speech_to_text.py:58:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/stackdriver.py:47:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/tasks.py:68:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/text_to_speech.py:66:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/translate.py:45:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/vertex_ai/auto_ml.py:81:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/vertex_ai/batch_prediction_job.py:54:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py:58:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/vertex_ai/hyperparameter_tuning_job.py:55:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/video_intelligence.py:62:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/hooks/vision.py:134:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/bigquery.py:1351:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/bigquery.py:1365:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/bigquery.py:1628:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/bigquery.py:1778:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/bigquery.py:1873:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/bigquery.py:1879:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/bigquery.py:1967:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/bigquery.py:2043:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/bigquery.py:2121:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/bigquery.py:2206:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/bigquery.py:2299:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/bigquery.py:2381:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/bigquery.py:2459:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/bigquery.py:2569:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/bigquery.py:2696:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/bigquery.py:853:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/cloud_build.py:200:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/cloud_composer.py:158:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/cloud_composer.py:298:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/cloud_composer.py:396:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/cloud_composer.py:481:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/cloud_composer.py:576:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/cloud_composer.py:696:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py:1047:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py:873:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataflow.py:1017:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataflow.py:1207:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataflow.py:1347:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataflow.py:159:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataflow.py:376:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataflow.py:646:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataflow.py:842:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataform.py:1044:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataform.py:1132:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataform.py:1217:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataform.py:171:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataform.py:253:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataform.py:349:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataform.py:425:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataform.py:505:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataform.py:596:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataform.py:680:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataform.py:775:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataform.py:865:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataform.py:954:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataform.py:96:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/datafusion.py:1015:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/datafusion.py:103:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/datafusion.py:185:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/datafusion.py:264:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/datafusion.py:378:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/datafusion.py:463:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/datafusion.py:558:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/datafusion.py:655:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/datafusion.py:748:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/datafusion.py:861:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataplex.py:118:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataplex.py:233:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataplex.py:339:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataplex.py:427:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataplex.py:529:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataplex.py:643:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/dataproc.py:992:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/datastore.py:105:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/datastore.py:218:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/datastore.py:306:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/datastore.py:373:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/datastore.py:440:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/datastore.py:507:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/datastore.py:572:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/datastore.py:635:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/datastore.py:695:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/gcs.py:138:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/gcs.py:236:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/gcs.py:316:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/gcs.py:999:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/mlengine.py:1066:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/mlengine.py:1226:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/mlengine.py:1484:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/mlengine.py:214:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/mlengine.py:363:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/mlengine.py:433:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/mlengine.py:508:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/mlengine.py:587:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/mlengine.py:698:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/mlengine.py:797:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/mlengine.py:889:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/mlengine.py:977:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/pubsub.py:150:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/pubsub.py:360:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/pubsub.py:495:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/pubsub.py:603:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/pubsub.py:713:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/pubsub.py:800:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/stackdriver.py:121:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/stackdriver.py:220:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/stackdriver.py:312:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/stackdriver.py:410:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/stackdriver.py:499:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/stackdriver.py:606:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/stackdriver.py:709:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/stackdriver.py:803:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/stackdriver.py:899:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/stackdriver.py:990:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/auto_ml.py:576:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/auto_ml.py:647:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/auto_ml.py:82:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/batch_prediction_job.py:218:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/batch_prediction_job.py:325:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/batch_prediction_job.py:404:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/batch_prediction_job.py:504:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/custom_job.py:1272:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/custom_job.py:1396:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/custom_job.py:152:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/dataset.py:174:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/dataset.py:254:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/dataset.py:335:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/dataset.py:415:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/dataset.py:505:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/dataset.py:588:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/dataset.py:89:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/endpoint_service.py:106:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/endpoint_service.py:189:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/endpoint_service.py:288:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/endpoint_service.py:373:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/endpoint_service.py:482:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/endpoint_service.py:572:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/endpoint_service.py:657:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/hyperparameter_tuning_job.py:222:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/hyperparameter_tuning_job.py:336:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/hyperparameter_tuning_job.py:408:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/hyperparameter_tuning_job.py:498:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/model_service.py:178:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/model_service.py:284:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/model_service.py:362:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/operators/vertex_ai/model_service.py:96:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/sensors/bigquery.py:160:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/sensors/bigquery.py:87:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/sensors/cloud_composer.py:73:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/sensors/dataflow.py:181:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/sensors/dataflow.py:270:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/sensors/dataflow.py:359:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/sensors/dataflow.py:90:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/sensors/dataform.py:87:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/sensors/datafusion.py:89:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/sensors/dataplex.py:96:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/sensors/gcs.py:214:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/sensors/gcs.py:277:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/sensors/gcs.py:380:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/sensors/gcs.py:86:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/sensors/pubsub.py:112:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/transfers/adls_to_gcs.py:128:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/transfers/azure_fileshare_to_gcs.py:97:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/transfers/bigquery_to_bigquery.py:112:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/transfers/bigquery_to_gcs.py:135:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/transfers/bigquery_to_mssql.py:111:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/transfers/bigquery_to_mysql.py:115:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/transfers/calendar_to_gcs.py:137:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/transfers/cassandra_to_gcs.py:125:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/transfers/gcs_to_bigquery.py:276:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/transfers/gcs_to_gcs.py:212:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/transfers/gcs_to_local.py:113:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/transfers/gcs_to_sftp.py:133:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/transfers/gdrive_to_gcs.py:89:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/transfers/gdrive_to_local.py:82:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/transfers/local_to_gcs.py:90:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/transfers/s3_to_gcs.py:129:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/transfers/sftp_to_gcs.py:108:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/transfers/sheets_to_gcs.py:88:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/transfers/sql_to_gcs.py:147:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/triggers/bigquery_dts.py:69:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/triggers/cloud_build.py:67:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/triggers/cloud_composer.py:50:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/cloud/triggers/datafusion.py:73:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/common/hooks/discovery_api.py:61:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/google/firebase/hooks/firestore.py:66:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/microsoft/azure/hooks/adx.py:132:17: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/microsoft/azure/hooks/fileshare.py:119:17: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/microsoft/azure/hooks/fileshare.py:142:17: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/microsoft/azure/operators/data_factory.py:212:17: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/microsoft/azure/transfers/azure_blob_to_gcs.py:89:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/microsoft/azure/utils.py:64:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/presto/transfers/gcs_to_presto.py:91:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/slack/utils/__init__.py:48:17: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/snowflake/hooks/snowflake.py:195:17: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/ssh/hooks/ssh.py:367:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/ssh/hooks/ssh.py:432:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/ssh/operators/ssh.py:139:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/tableau/hooks/tableau.py:123:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/trino/transfers/gcs_to_trino.py:94:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/yandex/hooks/yandex.py:100:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers/yandex/operators/yandexcloud_dataproc.py:259:17: B028 No explicit `stacklevel` keyword argument found
+ airflow/providers_manager.py:674:17: B028 No explicit `stacklevel` keyword argument found
+ airflow/serialization/serialized_objects.py:979:17: B028 No explicit `stacklevel` keyword argument found
+ airflow/settings.py:442:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/utils/context.py:205:13: B028 No explicit `stacklevel` keyword argument found
+ airflow/utils/context.py:313:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/www/app.py:111:9: B028 No explicit `stacklevel` keyword argument found
+ airflow/www/extensions/init_views.py:283:5: B028 No explicit `stacklevel` keyword argument found
+ tests/charts/test_basic_helm_chart.py:370:13: B028 No explicit `stacklevel` keyword argument found
+ tests/jobs/test_local_task_job.py:901:13: B028 No explicit `stacklevel` keyword argument found
+ tests/providers/amazon/conftest.py:36:9: B028 No explicit `stacklevel` keyword argument found
+ tests/providers/amazon/conftest.py:42:9: B028 No explicit `stacklevel` keyword argument found
```

</p>
</details>
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.5±0.03ms     2.8 MB/sec    1.00     14.4±0.02ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.01ms     4.3 MB/sec    1.00      3.8±0.00ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    442.2±1.42µs     6.7 MB/sec    1.00    440.0±1.85µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.4±0.01ms     4.0 MB/sec    1.00      6.4±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.01ms     4.9 MB/sec    1.00      8.2±0.01ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1789.4±2.46µs     9.3 MB/sec    1.00   1779.8±2.82µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.7±0.30µs    15.9 MB/sec    1.00    186.6±0.72µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.00ms     6.7 MB/sec    1.00      3.8±0.01ms     6.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.5±0.04ms     2.6 MB/sec    1.00     15.4±0.03ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.02ms     4.0 MB/sec    1.00      4.2±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.02    462.4±4.56µs     6.4 MB/sec    1.00    454.8±3.22µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.02ms     3.7 MB/sec    1.00      6.8±0.03ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.02ms     4.7 MB/sec    1.00      8.7±0.02ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1847.7±9.81µs     9.0 MB/sec    1.00   1846.1±5.78µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    190.5±1.06µs    15.5 MB/sec    1.01    191.9±3.89µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.02ms     6.4 MB/sec    1.01      4.0±0.03ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
