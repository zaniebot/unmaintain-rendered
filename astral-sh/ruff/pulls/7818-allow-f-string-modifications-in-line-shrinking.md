```yaml
number: 7818
title: Allow f-string modifications in line-shrinking cases
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/UP032
created_at: 2023-10-04T18:32:42Z
updated_at: 2023-10-04T19:32:37Z
url: https://github.com/astral-sh/ruff/pull/7818
synced_at: 2026-01-12T15:55:24Z
```

# Allow f-string modifications in line-shrinking cases

---

_@charliermarsh_

## Summary

This PR resolves an issue raised in https://github.com/astral-sh/ruff/discussions/7810, whereby we don't fix an f-string that exceeds the line length _even if_ the resultant code is _shorter_ than the current code.

As part of this change, I've also refactored and extracted some common logic we use around "ensuring a fix isn't breaking the line length rules".

## Test Plan

`cargo test`


---

_Merged by @charliermarsh on 2023-10-04 19:24_

---

_Closed by @charliermarsh on 2023-10-04 19:24_

---

_Branch deleted on 2023-10-04 19:24_

---

_Comment by @github-actions[bot] on 2023-10-04 19:26_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+68, -64, 0 error(s))

<details><summary>airflow (+34, -34)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/cli/cli_config.py#L100'>airflow/cli/cli_config.py:100:9:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/cli/cli_config.py#L100'>airflow/cli/cli_config.py:100:9:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/configuration.py#L1106'>airflow/configuration.py:1106:9:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/configuration.py#L1106'>airflow/configuration.py:1106:9:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/configuration.py#L870'>airflow/configuration.py:870:9:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/configuration.py#L870'>airflow/configuration.py:870:9:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/configuration.py#L895'>airflow/configuration.py:895:9:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/configuration.py#L895'>airflow/configuration.py:895:9:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/models/dag.py#L3143'>airflow/models/dag.py:3143:13:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/models/dag.py#L3143'>airflow/models/dag.py:3143:13:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/providers/amazon/aws/sensors/sqs.py#L112'>airflow/providers/amazon/aws/sensors/sqs.py:112:9:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/providers/amazon/aws/sensors/sqs.py#L112'>airflow/providers/amazon/aws/sensors/sqs.py:112:9:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/providers/google/cloud/sensors/dataform.py#L96'>airflow/providers/google/cloud/sensors/dataform.py:96:9:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/providers/google/cloud/sensors/dataform.py#L96'>airflow/providers/google/cloud/sensors/dataform.py:96:9:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/providers/google/cloud/sensors/datafusion.py#L119'>airflow/providers/google/cloud/sensors/datafusion.py:119:9:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/providers/google/cloud/sensors/datafusion.py#L119'>airflow/providers/google/cloud/sensors/datafusion.py:119:9:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/providers/ssh/hooks/ssh.py#L273'>airflow/providers/ssh/hooks/ssh.py:273:13:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/providers/ssh/hooks/ssh.py#L273'>airflow/providers/ssh/hooks/ssh.py:273:13:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/providers_manager.py#L493'>airflow/providers_manager.py:493:13:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/providers_manager.py#L493'>airflow/providers_manager.py:493:13:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/ti_deps/deps/prev_dagrun_dep.py#L160'>airflow/ti_deps/deps/prev_dagrun_dep.py:160:13:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/ti_deps/deps/prev_dagrun_dep.py#L160'>airflow/ti_deps/deps/prev_dagrun_dep.py:160:13:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/ti_deps/deps/trigger_rule_dep.py#L351'>airflow/ti_deps/deps/trigger_rule_dep.py:351:21:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/ti_deps/deps/trigger_rule_dep.py#L351'>airflow/ti_deps/deps/trigger_rule_dep.py:351:21:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/utils/setup_teardown.py#L170'>airflow/utils/setup_teardown.py:170:9:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/airflow/utils/setup_teardown.py#L170'>airflow/utils/setup_teardown.py:170:9:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/docs/exts/extra_files_with_substitutions.py#L31'>docs/exts/extra_files_with_substitutions.py:31:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/docs/exts/extra_files_with_substitutions.py#L31'>docs/exts/extra_files_with_substitutions.py:31:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/scripts/ci/pre_commit/common_precommit_utils.py#L32'>scripts/ci/pre_commit/common_precommit_utils.py:32:9:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/scripts/ci/pre_commit/common_precommit_utils.py#L32'>scripts/ci/pre_commit/common_precommit_utils.py:32:9:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/scripts/ci/pre_commit/pre_commit_check_cncf_k8s_used_for_k8s_executor_only.py#L63'>scripts/ci/pre_commit/pre_commit_check_cncf_k8s_used_for_k8s_executor_only.py:63:13:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/scripts/ci/pre_commit/pre_commit_check_cncf_k8s_used_for_k8s_executor_only.py#L63'>scripts/ci/pre_commit/pre_commit_check_cncf_k8s_used_for_k8s_executor_only.py:63:13:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/always/test_providers_manager.py#L189'>tests/always/test_providers_manager.py:189:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/always/test_providers_manager.py#L189'>tests/always/test_providers_manager.py:189:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/always/test_providers_manager.py#L203'>tests/always/test_providers_manager.py:203:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/always/test_providers_manager.py#L203'>tests/always/test_providers_manager.py:203:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/always/test_secrets_local_filesystem.py#L93'>tests/always/test_secrets_local_filesystem.py:93:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/always/test_secrets_local_filesystem.py#L93'>tests/always/test_secrets_local_filesystem.py:93:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/cli/commands/test_webserver_command.py#L307'>tests/cli/commands/test_webserver_command.py:307:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/cli/commands/test_webserver_command.py#L307'>tests/cli/commands/test_webserver_command.py:307:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/dag_processing/test_processor.py#L69'>tests/dag_processing/test_processor.py:69:5:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/dag_processing/test_processor.py#L69'>tests/dag_processing/test_processor.py:69:5:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/jobs/test_scheduler_job.py#L99'>tests/jobs/test_scheduler_job.py:99:5:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/jobs/test_scheduler_job.py#L99'>tests/jobs/test_scheduler_job.py:99:5:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/models/test_dagbag.py#L1020'>tests/models/test_dagbag.py:1020:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/models/test_dagbag.py#L1020'>tests/models/test_dagbag.py:1020:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/models/test_dagbag.py#L961'>tests/models/test_dagbag.py:961:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/models/test_dagbag.py#L961'>tests/models/test_dagbag.py:961:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/models/test_dagbag.py#L972'>tests/models/test_dagbag.py:972:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/models/test_dagbag.py#L972'>tests/models/test_dagbag.py:972:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/models/test_variable.py#L155'>tests/models/test_variable.py:155:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/models/test_variable.py#L155'>tests/models/test_variable.py:155:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/providers/amazon/aws/hooks/test_batch_client.py#L167'>tests/providers/amazon/aws/hooks/test_batch_client.py:167:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/providers/amazon/aws/hooks/test_batch_client.py#L167'>tests/providers/amazon/aws/hooks/test_batch_client.py:167:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/providers/apache/spark/hooks/test_spark_sql.py#L94'>tests/providers/apache/spark/hooks/test_spark_sql.py:94:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/providers/apache/spark/hooks/test_spark_sql.py#L94'>tests/providers/apache/spark/hooks/test_spark_sql.py:94:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/providers/elasticsearch/log/elasticmock/fake_elasticsearch.py#L271'>tests/providers/elasticsearch/log/elasticmock/fake_elasticsearch.py:271:13:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/providers/elasticsearch/log/elasticmock/fake_elasticsearch.py#L271'>tests/providers/elasticsearch/log/elasticmock/fake_elasticsearch.py:271:13:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/providers/imap/hooks/test_imap.py#L179'>tests/providers/imap/hooks/test_imap.py:179:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/providers/imap/hooks/test_imap.py#L179'>tests/providers/imap/hooks/test_imap.py:179:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/security/test_kerberos.py#L265'>tests/security/test_kerberos.py:265:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/security/test_kerberos.py#L265'>tests/security/test_kerberos.py:265:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/sensors/test_external_task_sensor.py#L1045'>tests/sensors/test_external_task_sensor.py:1045:5:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/sensors/test_external_task_sensor.py#L1045'>tests/sensors/test_external_task_sensor.py:1045:5:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/utils/test_db.py#L141'>tests/utils/test_db.py:141:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/utils/test_db.py#L141'>tests/utils/test_db.py:141:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/utils/test_db.py#L153'>tests/utils/test_db.py:153:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/49bb45ca8662fff9136fc218d9e242bdce1a0afe/tests/utils/test_db.py#L153'>tests/utils/test_db.py:153:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
</pre>

</p>
</details>
<details><summary>bokeh (+1, -1)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/02ec7ba6c14b840ebe9b73643dc422e33106b0b2/src/bokeh/core/has_props.py#L669'>src/bokeh/core/has_props.py:669:17:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/bokeh/bokeh/blob/02ec7ba6c14b840ebe9b73643dc422e33106b0b2/src/bokeh/core/has_props.py#L669'>src/bokeh/core/has_props.py:669:17:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
</pre>

</p>
</details>
<details><summary>content (+18, -18)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/BmcITSM/Integrations/BmcITSM/BmcITSM.py#L320'>Packs/BmcITSM/Integrations/BmcITSM/BmcITSM.py:320:9:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/BmcITSM/Integrations/BmcITSM/BmcITSM.py#L320'>Packs/BmcITSM/Integrations/BmcITSM/BmcITSM.py:320:9:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/Endace/Integrations/Endace/Endace.py#L581'>Packs/Endace/Integrations/Endace/Endace.py:581:53:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/Endace/Integrations/Endace/Endace.py#L581'>Packs/Endace/Integrations/Endace/Endace.py:581:53:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/FeedTAXII/Integrations/FeedTAXII/FeedTAXII.py#L825'>Packs/FeedTAXII/Integrations/FeedTAXII/FeedTAXII.py:825:33:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/FeedTAXII/Integrations/FeedTAXII/FeedTAXII.py#L825'>Packs/FeedTAXII/Integrations/FeedTAXII/FeedTAXII.py:825:33:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/FeedTAXII/Integrations/FeedTAXII/FeedTAXII.py#L832'>Packs/FeedTAXII/Integrations/FeedTAXII/FeedTAXII.py:832:33:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/FeedTAXII/Integrations/FeedTAXII/FeedTAXII.py#L832'>Packs/FeedTAXII/Integrations/FeedTAXII/FeedTAXII.py:832:33:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/FeedTAXII/Integrations/FeedTAXII/FeedTAXII.py#L861'>Packs/FeedTAXII/Integrations/FeedTAXII/FeedTAXII.py:861:13:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/FeedTAXII/Integrations/FeedTAXII/FeedTAXII.py#L861'>Packs/FeedTAXII/Integrations/FeedTAXII/FeedTAXII.py:861:13:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/FiltersAndTransformers/Scripts/MapPattern/MapPattern.py#L380'>Packs/FiltersAndTransformers/Scripts/MapPattern/MapPattern.py:380:13:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/FiltersAndTransformers/Scripts/MapPattern/MapPattern.py#L380'>Packs/FiltersAndTransformers/Scripts/MapPattern/MapPattern.py:380:13:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/FiltersAndTransformers/Scripts/ParseHTMLTables/ParseHTMLTables.py#L110'>Packs/FiltersAndTransformers/Scripts/ParseHTMLTables/ParseHTMLTables.py:110:9:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/FiltersAndTransformers/Scripts/ParseHTMLTables/ParseHTMLTables.py#L110'>Packs/FiltersAndTransformers/Scripts/ParseHTMLTables/ParseHTMLTables.py:110:9:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/GmailSingleUser/Integrations/GmailSingleUser/GmailSingleUser.py#L191'>Packs/GmailSingleUser/Integrations/GmailSingleUser/GmailSingleUser.py:191:9:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/GmailSingleUser/Integrations/GmailSingleUser/GmailSingleUser.py#L191'>Packs/GmailSingleUser/Integrations/GmailSingleUser/GmailSingleUser.py:191:9:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/PassiveTotal/Integrations/PassiveTotal_v2/PassiveTotal_v2_test.py#L884'>Packs/PassiveTotal/Integrations/PassiveTotal_v2/PassiveTotal_v2_test.py:884:5:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/PassiveTotal/Integrations/PassiveTotal_v2/PassiveTotal_v2_test.py#L884'>Packs/PassiveTotal/Integrations/PassiveTotal_v2/PassiveTotal_v2_test.py:884:5:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/Polygon/Integrations/Polygon/Polygon.py#L100'>Packs/Polygon/Integrations/Polygon/Polygon.py:100:17:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/Polygon/Integrations/Polygon/Polygon.py#L100'>Packs/Polygon/Integrations/Polygon/Polygon.py:100:17:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/Polygon/Integrations/Polygon/Polygon.py#L101'>Packs/Polygon/Integrations/Polygon/Polygon.py:101:21:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/Polygon/Integrations/Polygon/Polygon.py#L101'>Packs/Polygon/Integrations/Polygon/Polygon.py:101:21:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/SecneurXThreatFeeds/Integrations/SecneurXThreatFeeds/SecneurXThreatFeeds.py#L172'>Packs/SecneurXThreatFeeds/Integrations/SecneurXThreatFeeds/SecneurXThreatFeeds.py:172:25:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/SecneurXThreatFeeds/Integrations/SecneurXThreatFeeds/SecneurXThreatFeeds.py#L172'>Packs/SecneurXThreatFeeds/Integrations/SecneurXThreatFeeds/SecneurXThreatFeeds.py:172:25:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py#L696'>Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py:696:17:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py#L696'>Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py:696:17:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/ThreatConnect/Integrations/ThreatConnect_v2/ThreatConnect_v2.py#L228'>Packs/ThreatConnect/Integrations/ThreatConnect_v2/ThreatConnect_v2.py:228:17:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/ThreatConnect/Integrations/ThreatConnect_v2/ThreatConnect_v2.py#L228'>Packs/ThreatConnect/Integrations/ThreatConnect_v2/ThreatConnect_v2.py:228:17:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/ThreatX/Integrations/ThreatX/ThreatX.py#L441'>Packs/ThreatX/Integrations/ThreatX/ThreatX.py:441:13:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/ThreatX/Integrations/ThreatX/ThreatX.py#L441'>Packs/ThreatX/Integrations/ThreatX/ThreatX.py:441:13:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/TrustwaveSEG/Integrations/TrustwaveSEG/TrustwaveSEG.py#L64'>Packs/TrustwaveSEG/Integrations/TrustwaveSEG/TrustwaveSEG.py:64:9:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/TrustwaveSEG/Integrations/TrustwaveSEG/TrustwaveSEG.py#L64'>Packs/TrustwaveSEG/Integrations/TrustwaveSEG/TrustwaveSEG.py:64:9:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/qualys/Integrations/Qualysv2/Qualysv2.py#L2481'>Packs/qualys/Integrations/Qualysv2/Qualysv2.py:2481:5:</a> SIM102 Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Packs/qualys/Integrations/Qualysv2/Qualysv2.py#L2481'>Packs/qualys/Integrations/Qualysv2/Qualysv2.py:2481:5:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Utils/update_contribution_pack_in_base_branch.py#L71'>Utils/update_contribution_pack_in_base_branch.py:71:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/demisto/content/blob/73231c29a71789d85c0cb9600377eee56010a686/Utils/update_contribution_pack_in_base_branch.py#L71'>Utils/update_contribution_pack_in_base_branch.py:71:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
</pre>

</p>
</details>
<details><summary>ibis (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/5d7822c7ee80903952c0c258a708bfd95387fcb8/ibis/backends/bigquery/client.py#L204'>ibis/backends/bigquery/client.py:204:13:</a> UP032 [*] Use f-string instead of `format` call
</pre>

</p>
</details>
<details><summary>pymilvus (+3, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/56349083916c3b643c2039a0cde4bad902522b09/examples/example_bulkinsert_json.py#L139'>examples/example_bulkinsert_json.py:139:110:</a> UP032 [*] Use f-string instead of `format` call
+ <a href='https://github.com/milvus-io/pymilvus/blob/56349083916c3b643c2039a0cde4bad902522b09/examples/example_bulkinsert_json.py#L229'>examples/example_bulkinsert_json.py:229:11:</a> UP032 [*] Use f-string instead of `format` call
+ <a href='https://github.com/milvus-io/pymilvus/blob/56349083916c3b643c2039a0cde4bad902522b09/examples/example_bulkinsert_numpy.py#L231'>examples/example_bulkinsert_numpy.py:231:11:</a> UP032 [*] Use f-string instead of `format` call
</pre>

</p>
</details>
<details><summary>zulip (+11, -11)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_auth_backends.py#L5515'>zerver/tests/test_auth_backends.py:5515:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_auth_backends.py#L5515'>zerver/tests/test_auth_backends.py:5515:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_auth_backends.py#L5844'>zerver/tests/test_auth_backends.py:5844:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_auth_backends.py#L5844'>zerver/tests/test_auth_backends.py:5844:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_link_embed.py#L443'>zerver/tests/test_link_embed.py:443:13:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_link_embed.py#L443'>zerver/tests/test_link_embed.py:443:13:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_link_embed.py#L462'>zerver/tests/test_link_embed.py:462:13:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_link_embed.py#L462'>zerver/tests/test_link_embed.py:462:13:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_management_commands.py#L216'>zerver/tests/test_management_commands.py:216:17:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_management_commands.py#L216'>zerver/tests/test_management_commands.py:216:17:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_management_commands.py#L37'>zerver/tests/test_management_commands.py:37:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_management_commands.py#L37'>zerver/tests/test_management_commands.py:37:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_message_fetch.py#L3737'>zerver/tests/test_message_fetch.py:3737:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_message_fetch.py#L3737'>zerver/tests/test_message_fetch.py:3737:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_realm.py#L77'>zerver/tests/test_realm.py:77:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_realm.py#L77'>zerver/tests/test_realm.py:77:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_realm_export.py#L49'>zerver/tests/test_realm_export.py:49:13:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_realm_export.py#L49'>zerver/tests/test_realm_export.py:49:13:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_subs.py#L2559'>zerver/tests/test_subs.py:2559:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/tests/test_subs.py#L2559'>zerver/tests/test_subs.py:2559:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/webhooks/pivotal/tests.py#L205'>zerver/webhooks/pivotal/tests.py:205:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/zulip/zulip/blob/91f81d39623bcfbdab349910ae26920a20eb5463/zerver/webhooks/pivotal/tests.py#L205'>zerver/webhooks/pivotal/tests.py:205:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
</pre>

</p>
</details>
Rules changed: 3

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| SIM102 | 66 | 33 | 33 |
| SIM117 | 62 | 31 | 31 |
| UP032 | 4 | 4 | 0 |



---

_Comment by @charliermarsh on 2023-10-04 19:32_

All the UP032 additions are good. The SIM examples are also working as intended. Previously, we didn't consider the _indent_ when calculating the resulting line length.

---
