```yaml
number: 3780
title: Exempt return with side effects for TRY300
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: exempt-return-with-side-effects-for-try300
created_at: 2023-03-28T17:40:38Z
updated_at: 2023-03-29T05:28:32Z
url: https://github.com/astral-sh/ruff/pull/3780
synced_at: 2026-01-12T04:39:45Z
```

# Exempt return with side effects for TRY300

---

_Pull request opened by @JonathanPlasse on 2023-03-28 17:40_

- Close #3778

---

_Comment by @github-actions[bot] on 2023-03-28 17:53_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -101, 0 error(s))

<details><summary>airflow (+0, -53)</summary>
<p>

```diff
- airflow/api_connexion/endpoints/connection_endpoint.py:188:9: TRY300 Consider moving this statement to an `else` block
- airflow/api_connexion/endpoints/dag_run_endpoint.py:351:13: TRY300 Consider moving this statement to an `else` block
- airflow/api_connexion/endpoints/pool_endpoint.py:154:9: TRY300 Consider moving this statement to an `else` block
- airflow/api_connexion/schemas/connection_schema.py:60:13: TRY300 Consider moving this statement to an `else` block
- airflow/api_internal/endpoints/rpc_api_endpoint.py:97:9: TRY300 Consider moving this statement to an `else` block
- airflow/cli/commands/info_command.py:222:13: TRY300 Consider moving this statement to an `else` block
- airflow/configuration.py:125:9: TRY300 Consider moving this statement to an `else` block
- airflow/models/variable.py:77:17: TRY300 Consider moving this statement to an `else` block
- airflow/operators/python.py:665:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/alibaba/cloud/log/oss_task_handler.py:168:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/amazon/aws/hooks/base_aws.py:498:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/amazon/aws/hooks/base_aws.py:790:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/amazon/aws/hooks/batch_client.py:366:17: TRY300 Consider moving this statement to an `else` block
- airflow/providers/amazon/aws/hooks/cloud_formation.py:55:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/amazon/aws/hooks/emr.py:474:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/amazon/aws/hooks/glue.py:173:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/amazon/aws/hooks/glue_catalog.py:162:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/amazon/aws/hooks/quicksight.py:115:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/amazon/aws/hooks/redshift_cluster.py:98:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/amazon/aws/hooks/sagemaker.py:1027:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/amazon/aws/operators/sagemaker.py:990:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/amazon/aws/secrets/secrets_manager.py:330:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/amazon/aws/secrets/systems_manager.py:207:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/apache/cassandra/hooks/cassandra.py:206:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/apache/hdfs/sensors/hdfs.py:128:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/apache/livy/hooks/livy.py:556:21: TRY300 Consider moving this statement to an `else` block
- airflow/providers/databricks/hooks/databricks_base.py:591:17: TRY300 Consider moving this statement to an `else` block
- airflow/providers/dbt/cloud/hooks/dbt.py:238:21: TRY300 Consider moving this statement to an `else` block
- airflow/providers/google/ads/hooks/ads.py:179:17: TRY300 Consider moving this statement to an `else` block
- airflow/providers/google/ads/hooks/ads.py:248:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/google/cloud/operators/functions.py:402:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/google/cloud/operators/vertex_ai/batch_prediction_job.py:431:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/google/cloud/operators/vertex_ai/dataset.py:200:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/google/cloud/operators/vertex_ai/endpoint_service.py:398:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/google/cloud/operators/vertex_ai/hyperparameter_tuning_job.py:363:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/google/cloud/operators/vision.py:926:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/hashicorp/_internal_client/vault_client.py:382:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/hashicorp/_internal_client/vault_client.py:406:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/jenkins/operators/jenkins_job_trigger.py:59:9: TRY300 Consider moving this statement to an `else` block
- airflow/providers/microsoft/azure/hooks/cosmos.py:341:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/microsoft/azure/hooks/data_lake.py:129:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/microsoft/azure/log/wasb_task_handler.py:81:13: TRY300 Consider moving this statement to an `else` block
- airflow/sentry.py:185:25: TRY300 Consider moving this statement to an `else` block
- airflow/stats.py:242:13: TRY300 Consider moving this statement to an `else` block
- airflow/utils/cli.py:110:17: TRY300 Consider moving this statement to an `else` block
- airflow/utils/log/colored_log.py:100:13: TRY300 Consider moving this statement to an `else` block
- airflow/utils/sqlalchemy.py:241:9: TRY300 Consider moving this statement to an `else` block
- airflow/www/extensions/init_appbuilder.py:63:9: TRY300 Consider moving this statement to an `else` block
- airflow/www/views.py:1562:13: TRY300 Consider moving this statement to an `else` block
- dev/breeze/src/airflow_breeze/utils/path_utils.py:106:9: TRY300 Consider moving this statement to an `else` block
- dev/breeze/src/airflow_breeze/utils/run_utils.py:152:13: TRY300 Consider moving this statement to an `else` block
- docs/exts/removemarktransform.py:63:17: TRY300 Consider moving this statement to an `else` block
- scripts/ci/pre_commit/pre_commit_update_breeze_config_hash.py:47:9: TRY300 Consider moving this statement to an `else` block
```

</p>
</details>
<details><summary>bokeh (+0, -34)</summary>
<p>

```diff
- release/build.py:108:9: TRY300 Consider moving this statement to an `else` block
- release/build.py:128:9: TRY300 Consider moving this statement to an `else` block
- release/build.py:180:9: TRY300 Consider moving this statement to an `else` block
- release/build.py:192:9: TRY300 Consider moving this statement to an `else` block
- release/build.py:199:9: TRY300 Consider moving this statement to an `else` block
- release/build.py:206:9: TRY300 Consider moving this statement to an `else` block
- release/build.py:213:9: TRY300 Consider moving this statement to an `else` block
- release/build.py:220:9: TRY300 Consider moving this statement to an `else` block
- release/build.py:46:9: TRY300 Consider moving this statement to an `else` block
- release/build.py:56:9: TRY300 Consider moving this statement to an `else` block
- release/build.py:64:9: TRY300 Consider moving this statement to an `else` block
- release/build.py:74:9: TRY300 Consider moving this statement to an `else` block
- release/build.py:82:9: TRY300 Consider moving this statement to an `else` block
- release/build.py:90:9: TRY300 Consider moving this statement to an `else` block
- release/build.py:98:9: TRY300 Consider moving this statement to an `else` block
- release/checks.py:46:13: TRY300 Consider moving this statement to an `else` block
- release/credentials.py:57:17: TRY300 Consider moving this statement to an `else` block
- release/deploy.py:32:9: TRY300 Consider moving this statement to an `else` block
- release/deploy.py:44:9: TRY300 Consider moving this statement to an `else` block
- release/deploy.py:67:9: TRY300 Consider moving this statement to an `else` block
- release/deploy.py:77:9: TRY300 Consider moving this statement to an `else` block
- release/deploy.py:86:9: TRY300 Consider moving this statement to an `else` block
- release/git.py:31:9: TRY300 Consider moving this statement to an `else` block
- release/git.py:39:9: TRY300 Consider moving this statement to an `else` block
- release/git.py:47:9: TRY300 Consider moving this statement to an `else` block
- release/git.py:72:9: TRY300 Consider moving this statement to an `else` block
- release/git.py:80:9: TRY300 Consider moving this statement to an `else` block
- release/git.py:90:9: TRY300 Consider moving this statement to an `else` block
- release/git.py:98:9: TRY300 Consider moving this statement to an `else` block
- release/remote.py:41:9: TRY300 Consider moving this statement to an `else` block
- release/remote.py:71:9: TRY300 Consider moving this statement to an `else` block
- release/remote.py:79:9: TRY300 Consider moving this statement to an `else` block
- src/bokeh/core/property/dataspec.py:234:13: TRY300 Consider moving this statement to an `else` block
- src/bokeh/plotting/_plot.py:97:17: TRY300 Consider moving this statement to an `else` block
```

</p>
</details>
<details><summary>zulip (+0, -14)</summary>
<p>

```diff
- corporate/views/portico.py:45:9: TRY300 Consider moving this statement to an `else` block
- zerver/decorator.py:767:17: TRY300 Consider moving this statement to an `else` block
- zerver/lib/logging_util.py:61:13: TRY300 Consider moving this statement to an `else` block
- zerver/lib/management.py:106:13: TRY300 Consider moving this statement to an `else` block
- zerver/lib/sessions.py:28:9: TRY300 Consider moving this statement to an `else` block
- zerver/lib/tex.py:39:9: TRY300 Consider moving this statement to an `else` block
- zerver/lib/webhooks/common.py:229:9: TRY300 Consider moving this statement to an `else` block
- zerver/management/commands/compilemessages.py:68:13: TRY300 Consider moving this statement to an `else` block
- zerver/views/auth.py:862:13: TRY300 Consider moving this statement to an `else` block
- zerver/views/documentation.py:102:17: TRY300 Consider moving this statement to an `else` block
- zerver/views/realm.py:368:9: TRY300 Consider moving this statement to an `else` block
- zerver/views/realm_linkifiers.py:38:9: TRY300 Consider moving this statement to an `else` block
- zerver/views/realm_linkifiers.py:71:9: TRY300 Consider moving this statement to an `else` block
- zproject/backends.py:2273:13: TRY300 Consider moving this statement to an `else` block
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.8±0.09ms     2.4 MB/sec    1.00     16.7±0.09ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.01ms     3.9 MB/sec    1.00      4.2±0.01ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    555.2±1.30µs     5.3 MB/sec    1.01    558.6±3.63µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.2±0.07ms     3.6 MB/sec    1.00      7.1±0.01ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.02ms     4.7 MB/sec    1.00      8.6±0.02ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1943.6±3.42µs     8.6 MB/sec    1.00   1941.0±3.07µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    222.8±9.27µs    13.2 MB/sec    1.02    227.4±9.28µs    13.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.01ms     6.4 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     19.3±0.23ms     2.1 MB/sec    1.00     19.2±0.30ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.0±0.08ms     3.4 MB/sec    1.00      4.9±0.08ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    599.5±9.02µs     4.9 MB/sec    1.00   601.4±14.25µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.2±0.14ms     3.1 MB/sec    1.00      8.2±0.12ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.01     10.0±0.12ms     4.1 MB/sec    1.00      9.9±0.09ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.03ms     7.7 MB/sec    1.00      2.2±0.06ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    237.1±5.59µs    12.4 MB/sec    1.00    236.4±9.88µs    12.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.6±0.07ms     5.6 MB/sec    1.00      4.6±0.10ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @JonathanPlasse on 2023-03-28 18:11_

As said in the issue.
Changing this code
```python
def something_that_may_go_wrong():
    raise Exception

def func():
    try:
        _ = something_that_may_go_wrong()
        return something_that_may_go_wrong()
    except Exception:
        print("Failed")
```
into this
```python
def something_that_may_go_wrong():
    raise Exception

def func():
    try:
        _ = something_that_may_go_wrong()
        result = something_that_may_go_wrong()
    except Exception:
        print("Failed")
    else:
        return result
```
Is not much better and decreases readability.

And this is a common pattern as there are 101 fewer errors in the ecosystem check with this PR.

---

_@charliermarsh approved on 2023-03-28 23:45_

---

_Merged by @charliermarsh on 2023-03-28 23:52_

---

_Closed by @charliermarsh on 2023-03-28 23:52_

---

_Label `bug` added by @charliermarsh on 2023-03-28 23:52_

---

_Label `rule` added by @charliermarsh on 2023-03-28 23:52_

---

_Branch deleted on 2023-03-29 05:28_

---
