```yaml
number: 4794
title: Exclude function definition from too-many-statements rule
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/statements
created_at: 2023-06-02T03:58:19Z
updated_at: 2023-06-02T04:30:30Z
url: https://github.com/astral-sh/ruff/pull/4794
synced_at: 2026-01-12T03:50:03Z
```

# Exclude function definition from too-many-statements rule

---

_Pull request opened by @charliermarsh on 2023-06-02 03:58_

## Summary

Pylint seems to include the `def` itself when counting the number of statements in a function, which feels off-by-one in practice. This PR removes that extra statement from the count.

Closes #4659.


---

_@charliermarsh reviewed on 2023-06-02 03:58_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/too_many_statements.rs`:179 on 2023-06-02 03:58_

(These are redundant, we already include them in the `assert_equal!` below.)

---

_@charliermarsh reviewed on 2023-06-02 03:58_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/snapshots/ruff__rules__pylint__tests__max_statements.snap`:9 on 2023-06-02 03:58_

E.g., it's strange that this fails with `max_statements=1`.

---

_Label `bug` added by @charliermarsh on 2023-06-02 03:58_

---

_Merged by @charliermarsh on 2023-06-02 04:04_

---

_Closed by @charliermarsh on 2023-06-02 04:04_

---

_Branch deleted on 2023-06-02 04:04_

---

_Comment by @github-actions[bot] on 2023-06-02 04:07_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+219, -234, 0 error(s))

<details><summary>airflow (+78, -85)</summary>
<p>

```diff
+ airflow/api_connexion/endpoints/user_endpoint.py:128:5: PLR0915 Too many statements (51 > 50)
- airflow/api_connexion/endpoints/user_endpoint.py:128:5: PLR0915 Too many statements (52 > 50)
+ airflow/cli/commands/internal_api_command.py:61:5: PLR0915 Too many statements (57 > 50)
- airflow/cli/commands/internal_api_command.py:61:5: PLR0915 Too many statements (58 > 50)
+ airflow/cli/commands/webserver_command.py:323:5: PLR0915 Too many statements (78 > 50)
- airflow/cli/commands/webserver_command.py:323:5: PLR0915 Too many statements (79 > 50)
+ airflow/dag_processing/manager.py:558:9: PLR0915 Too many statements (73 > 50)
- airflow/dag_processing/manager.py:558:9: PLR0915 Too many statements (74 > 50)
+ airflow/dag_processing/processor.py:416:9: PLR0915 Too many statements (81 > 50)
- airflow/dag_processing/processor.py:416:9: PLR0915 Too many statements (82 > 50)
+ airflow/executors/kubernetes_executor.py:674:9: PLR0915 Too many statements (60 > 50)
- airflow/executors/kubernetes_executor.py:674:9: PLR0915 Too many statements (61 > 50)
+ airflow/jobs/backfill_job_runner.py:428:9: PLR0915 Too many statements (127 > 50)
- airflow/jobs/backfill_job_runner.py:428:9: PLR0915 Too many statements (128 > 50)
+ airflow/jobs/backfill_job_runner.py:462:17: PLR0915 Too many statements (73 > 50)
- airflow/jobs/backfill_job_runner.py:462:17: PLR0915 Too many statements (74 > 50)
+ airflow/jobs/backfill_job_runner.py:836:9: PLR0915 Too many statements (55 > 50)
- airflow/jobs/backfill_job_runner.py:836:9: PLR0915 Too many statements (56 > 50)
+ airflow/jobs/scheduler_job_runner.py:284:9: PLR0915 Too many statements (130 > 50)
- airflow/jobs/scheduler_job_runner.py:284:9: PLR0915 Too many statements (131 > 50)
+ airflow/migrations/versions/0027_1_10_0_add_time_zone_awareness.py:282:5: PLR0915 Too many statements (56 > 50)
- airflow/migrations/versions/0027_1_10_0_add_time_zone_awareness.py:282:5: PLR0915 Too many statements (57 > 50)
+ airflow/migrations/versions/0027_1_10_0_add_time_zone_awareness.py:39:5: PLR0915 Too many statements (60 > 50)
- airflow/migrations/versions/0027_1_10_0_add_time_zone_awareness.py:39:5: PLR0915 Too many statements (61 > 50)
+ airflow/migrations/versions/0046_1_10_5_change_datetime_to_datetime2_6_on_mssql_.py:154:5: PLR0915 Too many statements (55 > 50)
- airflow/migrations/versions/0046_1_10_5_change_datetime_to_datetime2_6_on_mssql_.py:154:5: PLR0915 Too many statements (56 > 50)
+ airflow/migrations/versions/0046_1_10_5_change_datetime_to_datetime2_6_on_mssql_.py:41:5: PLR0915 Too many statements (55 > 50)
- airflow/migrations/versions/0046_1_10_5_change_datetime_to_datetime2_6_on_mssql_.py:41:5: PLR0915 Too many statements (56 > 50)
+ airflow/migrations/versions/0093_2_2_0_taskinstance_keyed_to_dagrun.py:290:5: PLR0915 Too many statements (57 > 50)
- airflow/migrations/versions/0093_2_2_0_taskinstance_keyed_to_dagrun.py:290:5: PLR0915 Too many statements (58 > 50)
+ airflow/migrations/versions/0093_2_2_0_taskinstance_keyed_to_dagrun.py:67:5: PLR0915 Too many statements (95 > 50)
- airflow/migrations/versions/0093_2_2_0_taskinstance_keyed_to_dagrun.py:67:5: PLR0915 Too many statements (96 > 50)
+ airflow/models/abstractoperator.py:394:9: PLR0915 Too many statements (51 > 50)
- airflow/models/abstractoperator.py:394:9: PLR0915 Too many statements (52 > 50)
+ airflow/models/baseoperator.py:742:9: PLR0915 Too many statements (98 > 50)
- airflow/models/baseoperator.py:742:9: PLR0915 Too many statements (99 > 50)
+ airflow/models/dag.py:1627:9: PLR0915 Too many statements (89 > 50)
- airflow/models/dag.py:1627:9: PLR0915 Too many statements (90 > 50)
+ airflow/models/dag.py:2297:9: PLR0915 Too many statements (53 > 50)
- airflow/models/dag.py:2297:9: PLR0915 Too many statements (54 > 50)
+ airflow/models/dag.py:2838:9: PLR0915 Too many statements (115 > 50)
- airflow/models/dag.py:2838:9: PLR0915 Too many statements (116 > 50)
+ airflow/models/dag.py:390:9: PLR0915 Too many statements (122 > 50)
- airflow/models/dag.py:390:9: PLR0915 Too many statements (123 > 50)
+ airflow/models/dagrun.py:537:9: PLR0915 Too many statements (68 > 50)
- airflow/models/dagrun.py:537:9: PLR0915 Too many statements (69 > 50)
+ airflow/models/taskinstance.py:1441:9: PLR0915 Too many statements (73 > 50)
- airflow/models/taskinstance.py:1441:9: PLR0915 Too many statements (74 > 50)
+ airflow/models/taskinstance.py:1885:9: PLR0915 Too many statements (55 > 50)
- airflow/models/taskinstance.py:1885:9: PLR0915 Too many statements (56 > 50)
+ airflow/models/taskinstance.py:1994:9: PLR0915 Too many statements (71 > 50)
- airflow/models/taskinstance.py:1994:9: PLR0915 Too many statements (72 > 50)
+ airflow/providers/amazon/aws/operators/redshift_cluster.py:194:9: PLR0915 Too many statements (68 > 50)
- airflow/providers/amazon/aws/operators/redshift_cluster.py:194:9: PLR0915 Too many statements (69 > 50)
+ airflow/providers/amazon/aws/utils/connection_wrapper.py:131:9: PLR0915 Too many statements (61 > 50)
- airflow/providers/amazon/aws/utils/connection_wrapper.py:131:9: PLR0915 Too many statements (62 > 50)
+ airflow/providers/apache/hive/transfers/s3_to_hive.py:137:9: PLR0915 Too many statements (52 > 50)
- airflow/providers/apache/hive/transfers/s3_to_hive.py:137:9: PLR0915 Too many statements (53 > 50)
+ airflow/providers/apache/spark/hooks/spark_submit.py:257:9: PLR0915 Too many statements (63 > 50)
- airflow/providers/apache/spark/hooks/spark_submit.py:257:9: PLR0915 Too many statements (64 > 50)
+ airflow/providers/cncf/kubernetes/operators/pod.py:251:9: PLR0915 Too many statements (61 > 50)
- airflow/providers/cncf/kubernetes/operators/pod.py:251:9: PLR0915 Too many statements (62 > 50)
+ airflow/providers/docker/operators/docker.py:176:9: PLR0915 Too many statements (61 > 50)
- airflow/providers/docker/operators/docker.py:176:9: PLR0915 Too many statements (62 > 50)
- airflow/providers/google/cloud/hooks/bigquery.py:1629:9: PLR0915 Too many statements (51 > 50)
+ airflow/providers/google/cloud/hooks/bigquery.py:2037:9: PLR0915 Too many statements (57 > 50)
- airflow/providers/google/cloud/hooks/bigquery.py:2037:9: PLR0915 Too many statements (58 > 50)
+ airflow/providers/google/cloud/operators/bigquery.py:1529:9: PLR0915 Too many statements (55 > 50)
- airflow/providers/google/cloud/operators/bigquery.py:1529:9: PLR0915 Too many statements (56 > 50)
+ airflow/providers/google/cloud/operators/gcs.py:703:9: PLR0915 Too many statements (63 > 50)
- airflow/providers/google/cloud/operators/gcs.py:703:9: PLR0915 Too many statements (64 > 50)
+ airflow/providers/google/cloud/operators/mlengine.py:1159:9: PLR0915 Too many statements (52 > 50)
- airflow/providers/google/cloud/operators/mlengine.py:1159:9: PLR0915 Too many statements (53 > 50)
+ airflow/providers/google/cloud/transfers/gcs_to_bigquery.py:185:9: PLR0915 Too many statements (51 > 50)
- airflow/providers/google/cloud/transfers/gcs_to_bigquery.py:185:9: PLR0915 Too many statements (52 > 50)
+ airflow/providers/google/cloud/transfers/gcs_to_bigquery.py:315:9: PLR0915 Too many statements (53 > 50)
- airflow/providers/google/cloud/transfers/gcs_to_bigquery.py:315:9: PLR0915 Too many statements (54 > 50)
+ airflow/providers/google/cloud/transfers/sql_to_gcs.py:215:9: PLR0915 Too many statements (69 > 50)
- airflow/providers/google/cloud/transfers/sql_to_gcs.py:215:9: PLR0915 Too many statements (70 > 50)
+ airflow/providers/hashicorp/hooks/vault.py:108:9: PLR0915 Too many statements (51 > 50)
- airflow/providers/hashicorp/hooks/vault.py:108:9: PLR0915 Too many statements (52 > 50)
+ airflow/providers/microsoft/winrm/hooks/winrm.py:119:9: PLR0915 Too many statements (60 > 50)
- airflow/providers/microsoft/winrm/hooks/winrm.py:119:9: PLR0915 Too many statements (61 > 50)
+ airflow/providers/oracle/hooks/oracle.py:125:9: PLR0915 Too many statements (70 > 50)
- airflow/providers/oracle/hooks/oracle.py:125:9: PLR0915 Too many statements (71 > 50)
- airflow/providers/sftp/operators/sftp.py:106:9: PLR0915 Too many statements (51 > 50)
+ airflow/providers/ssh/hooks/ssh.py:105:9: PLR0915 Too many statements (101 > 50)
- airflow/providers/ssh/hooks/ssh.py:105:9: PLR0915 Too many statements (102 > 50)
+ airflow/serialization/serialized_objects.py:875:9: PLR0915 Too many statements (63 > 50)
- airflow/serialization/serialized_objects.py:875:9: PLR0915 Too many statements (64 > 50)
+ airflow/ti_deps/deps/trigger_rule_dep.py:111:9: PLR0915 Too many statements (159 > 50)
- airflow/ti_deps/deps/trigger_rule_dep.py:111:9: PLR0915 Too many statements (160 > 50)
+ airflow/utils/dates.py:41:5: PLR0915 Too many statements (51 > 50)
- airflow/utils/dates.py:41:5: PLR0915 Too many statements (52 > 50)
+ airflow/utils/db.py:115:5: PLR0915 Too many statements (60 > 50)
- airflow/utils/db.py:115:5: PLR0915 Too many statements (61 > 50)
+ airflow/utils/process_utils.py:53:5: PLR0915 Too many statements (53 > 50)
- airflow/utils/process_utils.py:53:5: PLR0915 Too many statements (54 > 50)
+ airflow/www/app.py:78:5: PLR0915 Too many statements (60 > 50)
- airflow/www/app.py:78:5: PLR0915 Too many statements (61 > 50)
- airflow/www/extensions/init_appbuilder.py:173:9: PLR0915 Too many statements (51 > 50)
+ airflow/www/fab_security/manager.py:1058:9: PLR0915 Too many statements (79 > 50)
- airflow/www/fab_security/manager.py:1058:9: PLR0915 Too many statements (80 > 50)
+ airflow/www/fab_security/manager.py:217:9: PLR0915 Too many statements (54 > 50)
- airflow/www/fab_security/manager.py:217:9: PLR0915 Too many statements (55 > 50)
+ airflow/www/views.py:1387:9: PLR0915 Too many statements (56 > 50)
- airflow/www/views.py:1387:9: PLR0915 Too many statements (57 > 50)
+ airflow/www/views.py:1943:9: PLR0915 Too many statements (82 > 50)
- airflow/www/views.py:1943:9: PLR0915 Too many statements (83 > 50)
+ airflow/www/views.py:270:5: PLR0915 Too many statements (64 > 50)
- airflow/www/views.py:270:5: PLR0915 Too many statements (65 > 50)
+ airflow/www/views.py:297:9: PLR0915 Too many statements (60 > 50)
- airflow/www/views.py:297:9: PLR0915 Too many statements (61 > 50)
+ airflow/www/views.py:3150:9: PLR0915 Too many statements (64 > 50)
- airflow/www/views.py:3150:9: PLR0915 Too many statements (65 > 50)
+ airflow/www/views.py:3527:9: PLR0915 Too many statements (57 > 50)
- airflow/www/views.py:3527:9: PLR0915 Too many statements (58 > 50)
+ airflow/www/views.py:665:9: PLR0915 Too many statements (126 > 50)
- airflow/www/views.py:665:9: PLR0915 Too many statements (127 > 50)
+ dev/assign_cherry_picked_prs_with_milestone.py:250:5: PLR0915 Too many statements (97 > 50)
- dev/assign_cherry_picked_prs_with_milestone.py:250:5: PLR0915 Too many statements (98 > 50)
+ dev/breeze/src/airflow_breeze/commands/developer_commands.py:432:5: PLR0915 Too many statements (54 > 50)
- dev/breeze/src/airflow_breeze/commands/developer_commands.py:432:5: PLR0915 Too many statements (55 > 50)
+ dev/breeze/src/airflow_breeze/commands/release_candidate_command.py:309:5: PLR0915 Too many statements (53 > 50)
- dev/breeze/src/airflow_breeze/commands/release_candidate_command.py:309:5: PLR0915 Too many statements (54 > 50)
- dev/breeze/src/airflow_breeze/commands/release_command.py:214:5: PLR0915 Too many statements (51 > 50)
+ dev/breeze/src/airflow_breeze/commands/release_management_commands.py:788:5: PLR0915 Too many statements (57 > 50)
- dev/breeze/src/airflow_breeze/commands/release_management_commands.py:788:5: PLR0915 Too many statements (58 > 50)
+ dev/breeze/src/airflow_breeze/commands/release_management_commands.py:998:5: PLR0915 Too many statements (61 > 50)
- dev/breeze/src/airflow_breeze/commands/release_management_commands.py:998:5: PLR0915 Too many statements (62 > 50)
+ dev/breeze/src/airflow_breeze/commands/setup_commands.py:398:5: PLR0915 Too many statements (59 > 50)
- dev/breeze/src/airflow_breeze/commands/setup_commands.py:398:5: PLR0915 Too many statements (60 > 50)
- dev/breeze/src/airflow_breeze/utils/docker_command_utils.py:586:5: PLR0915 Too many statements (51 > 50)
- dev/breeze/src/airflow_breeze/utils/parallel.py:364:5: PLR0915 Too many statements (51 > 50)
+ dev/perf/scheduler_dag_execution_timing.py:208:5: PLR0915 Too many statements (61 > 50)
- dev/perf/scheduler_dag_execution_timing.py:208:5: PLR0915 Too many statements (62 > 50)
+ dev/provider_packages/prepare_provider_packages.py:945:5: PLR0915 Too many statements (55 > 50)
- dev/provider_packages/prepare_provider_packages.py:945:5: PLR0915 Too many statements (56 > 50)
+ dev/system_tests/update_issue_status.py:132:5: PLR0915 Too many statements (77 > 50)
- dev/system_tests/update_issue_status.py:132:5: PLR0915 Too many statements (78 > 50)
+ docker_tests/test_docker_compose_quick_start.py:113:5: PLR0915 Too many statements (65 > 50)
- docker_tests/test_docker_compose_quick_start.py:113:5: PLR0915 Too many statements (66 > 50)
+ scripts/in_container/verify_providers.py:220:5: PLR0915 Too many statements (60 > 50)
- scripts/in_container/verify_providers.py:220:5: PLR0915 Too many statements (61 > 50)
+ tests/jobs/test_backfill_job.py:1494:9: PLR0915 Too many statements (100 > 50)
- tests/jobs/test_backfill_job.py:1494:9: PLR0915 Too many statements (101 > 50)
+ tests/jobs/test_scheduler_job.py:1060:9: PLR0915 Too many statements (59 > 50)
- tests/jobs/test_scheduler_job.py:1060:9: PLR0915 Too many statements (60 > 50)
+ tests/models/test_dagbag.py:496:9: PLR0915 Too many statements (63 > 50)
- tests/models/test_dagbag.py:496:9: PLR0915 Too many statements (64 > 50)
+ tests/models/test_dagbag.py:635:9: PLR0915 Too many statements (56 > 50)
- tests/models/test_dagbag.py:635:9: PLR0915 Too many statements (57 > 50)
+ tests/models/test_taskinstance.py:738:9: PLR0915 Too many statements (52 > 50)
- tests/models/test_taskinstance.py:738:9: PLR0915 Too many statements (53 > 50)
+ tests/models/test_taskinstance.py:836:9: PLR0915 Too many statements (53 > 50)
- tests/models/test_taskinstance.py:836:9: PLR0915 Too many statements (54 > 50)
+ tests/providers/apache/hive/transfers/test_s3_to_hive.py:42:9: PLR0915 Too many statements (54 > 50)
- tests/providers/apache/hive/transfers/test_s3_to_hive.py:42:9: PLR0915 Too many statements (55 > 50)
+ tests/providers/google/cloud/transfers/test_sql_to_gcs.py:94:9: PLR0915 Too many statements (95 > 50)
- tests/providers/google/cloud/transfers/test_sql_to_gcs.py:94:9: PLR0915 Too many statements (96 > 50)
+ tests/system/providers/amazon/aws/example_sagemaker.py:194:5: PLR0915 Too many statements (59 > 50)
- tests/system/providers/amazon/aws/example_sagemaker.py:194:5: PLR0915 Too many statements (60 > 50)
- tests/www/views/test_views.py:335:5: PLR0915 Too many statements (51 > 50)
```

</p>
</details>
<details><summary>bokeh (+11, -12)</summary>
<p>

```diff
+ src/bokeh/application/handlers/directory.py:110:9: PLR0915 Too many statements (59 > 50)
- src/bokeh/application/handlers/directory.py:110:9: PLR0915 Too many statements (60 > 50)
+ src/bokeh/command/subcommands/serve.py:799:9: PLR0915 Too many statements (107 > 50)
- src/bokeh/command/subcommands/serve.py:799:9: PLR0915 Too many statements (108 > 50)
+ src/bokeh/embed/bundle.py:259:5: PLR0915 Too many statements (61 > 50)
- src/bokeh/embed/bundle.py:259:5: PLR0915 Too many statements (62 > 50)
+ src/bokeh/layouts.py:317:5: PLR0915 Too many statements (69 > 50)
- src/bokeh/layouts.py:317:5: PLR0915 Too many statements (70 > 50)
+ src/bokeh/plotting/_graph.py:54:5: PLR0915 Too many statements (61 > 50)
- src/bokeh/plotting/_graph.py:54:5: PLR0915 Too many statements (62 > 50)
+ src/bokeh/server/tornado.py:252:9: PLR0915 Too many statements (125 > 50)
- src/bokeh/server/tornado.py:252:9: PLR0915 Too many statements (126 > 50)
+ src/bokeh/util/compiler.py:501:5: PLR0915 Too many statements (60 > 50)
- src/bokeh/util/compiler.py:501:5: PLR0915 Too many statements (61 > 50)
+ tests/support/plugins/jupyter_notebook.py:61:5: PLR0915 Too many statements (53 > 50)
- tests/support/plugins/jupyter_notebook.py:61:5: PLR0915 Too many statements (54 > 50)
+ tests/unit/bokeh/core/test_has_props.py:195:5: PLR0915 Too many statements (55 > 50)
- tests/unit/bokeh/core/test_has_props.py:195:5: PLR0915 Too many statements (56 > 50)
+ tests/unit/bokeh/plotting/test_figure.py:388:5: PLR0915 Too many statements (68 > 50)
- tests/unit/bokeh/plotting/test_figure.py:388:5: PLR0915 Too many statements (69 > 50)
- tests/unit/bokeh/plotting/test_figure.py:474:5: PLR0915 Too many statements (51 > 50)
+ tests/unit/bokeh/test_client_server.py:798:9: PLR0915 Too many statements (62 > 50)
- tests/unit/bokeh/test_client_server.py:798:9: PLR0915 Too many statements (63 > 50)
```

</p>
</details>
<details><summary>zulip (+130, -137)</summary>
<p>

```diff
+ analytics/management/commands/populate_analytics_db.py:70:9: PLR0915 Too many statements (103 > 50)
- analytics/management/commands/populate_analytics_db.py:70:9: PLR0915 Too many statements (104 > 50)
+ analytics/tests/test_stats_views.py:392:9: PLR0915 Too many statements (51 > 50)
- analytics/tests/test_stats_views.py:392:9: PLR0915 Too many statements (52 > 50)
+ analytics/tests/test_support_views.py:30:9: PLR0915 Too many statements (125 > 50)
- analytics/tests/test_support_views.py:30:9: PLR0915 Too many statements (126 > 50)
+ analytics/views/installation_activity.py:101:5: PLR0915 Too many statements (57 > 50)
- analytics/views/installation_activity.py:101:5: PLR0915 Too many statements (58 > 50)
- analytics/views/stats.py:232:5: PLR0915 Too many statements (100 > 50)
+ analytics/views/stats.py:232:5: PLR0915 Too many statements (99 > 50)
+ analytics/views/support.py:156:5: PLR0915 Too many statements (138 > 50)
- analytics/views/support.py:156:5: PLR0915 Too many statements (139 > 50)
+ corporate/lib/stripe.py:840:5: PLR0915 Too many statements (53 > 50)
- corporate/lib/stripe.py:840:5: PLR0915 Too many statements (54 > 50)
+ corporate/tests/test_stripe.py:1015:9: PLR0915 Too many statements (79 > 50)
- corporate/tests/test_stripe.py:1015:9: PLR0915 Too many statements (80 > 50)
+ corporate/tests/test_stripe.py:1301:9: PLR0915 Too many statements (59 > 50)
- corporate/tests/test_stripe.py:1301:9: PLR0915 Too many statements (60 > 50)
+ corporate/tests/test_stripe.py:1484:9: PLR0915 Too many statements (55 > 50)
- corporate/tests/test_stripe.py:1484:9: PLR0915 Too many statements (56 > 50)
- corporate/tests/test_stripe.py:1654:9: PLR0915 Too many statements (51 > 50)
+ corporate/tests/test_stripe.py:1975:9: PLR0915 Too many statements (51 > 50)
- corporate/tests/test_stripe.py:1975:9: PLR0915 Too many statements (52 > 50)
+ corporate/tests/test_stripe.py:2717:9: PLR0915 Too many statements (60 > 50)
- corporate/tests/test_stripe.py:2717:9: PLR0915 Too many statements (61 > 50)
+ corporate/tests/test_stripe.py:2824:9: PLR0915 Too many statements (87 > 50)
- corporate/tests/test_stripe.py:2824:9: PLR0915 Too many statements (88 > 50)
+ corporate/tests/test_stripe.py:3012:9: PLR0915 Too many statements (60 > 50)
- corporate/tests/test_stripe.py:3012:9: PLR0915 Too many statements (61 > 50)
+ corporate/tests/test_stripe.py:3657:9: PLR0915 Too many statements (61 > 50)
- corporate/tests/test_stripe.py:3657:9: PLR0915 Too many statements (62 > 50)
+ corporate/tests/test_stripe.py:693:9: PLR0915 Too many statements (56 > 50)
- corporate/tests/test_stripe.py:693:9: PLR0915 Too many statements (57 > 50)
+ scripts/setup/generate_secrets.py:75:5: PLR0915 Too many statements (54 > 50)
- scripts/setup/generate_secrets.py:75:5: PLR0915 Too many statements (55 > 50)
+ tools/lib/provision.py:350:5: PLR0915 Too many statements (60 > 50)
- tools/lib/provision.py:350:5: PLR0915 Too many statements (61 > 50)
+ tools/lib/provision_inner.py:193:5: PLR0915 Too many statements (76 > 50)
- tools/lib/provision_inner.py:193:5: PLR0915 Too many statements (77 > 50)
+ tools/lib/template_parser.py:346:5: PLR0915 Too many statements (63 > 50)
- tools/lib/template_parser.py:346:5: PLR0915 Too many statements (64 > 50)
+ tools/lib/template_parser.py:51:5: PLR0915 Too many statements (145 > 50)
- tools/lib/template_parser.py:51:5: PLR0915 Too many statements (146 > 50)
+ tools/tests/test_template_parser.py:275:9: PLR0915 Too many statements (55 > 50)
- tools/tests/test_template_parser.py:275:9: PLR0915 Too many statements (56 > 50)
+ zerver/actions/create_realm.py:139:5: PLR0915 Too many statements (68 > 50)
- zerver/actions/create_realm.py:139:5: PLR0915 Too many statements (69 > 50)
+ zerver/actions/message_edit.py:1149:5: PLR0915 Too many statements (61 > 50)
- zerver/actions/message_edit.py:1149:5: PLR0915 Too many statements (62 > 50)
+ zerver/actions/message_edit.py:351:5: PLR0915 Too many statements (226 > 50)
- zerver/actions/message_edit.py:351:5: PLR0915 Too many statements (227 > 50)
+ zerver/actions/message_send.py:1270:5: PLR0915 Too many statements (66 > 50)
- zerver/actions/message_send.py:1270:5: PLR0915 Too many statements (67 > 50)
+ zerver/actions/message_send.py:714:5: PLR0915 Too many statements (71 > 50)
- zerver/actions/message_send.py:714:5: PLR0915 Too many statements (72 > 50)
+ zerver/actions/scheduled_messages.py:145:5: PLR0915 Too many statements (51 > 50)
- zerver/actions/scheduled_messages.py:145:5: PLR0915 Too many statements (52 > 50)
+ zerver/actions/user_settings.py:397:5: PLR0915 Too many statements (54 > 50)
- zerver/actions/user_settings.py:397:5: PLR0915 Too many statements (55 > 50)
+ zerver/data_import/mattermost.py:391:5: PLR0915 Too many statements (53 > 50)
- zerver/data_import/mattermost.py:391:5: PLR0915 Too many statements (54 > 50)
+ zerver/data_import/rocketchat.py:1054:5: PLR0915 Too many statements (59 > 50)
- zerver/data_import/rocketchat.py:1054:5: PLR0915 Too many statements (60 > 50)
+ zerver/data_import/rocketchat.py:605:5: PLR0915 Too many statements (93 > 50)
- zerver/data_import/rocketchat.py:605:5: PLR0915 Too many statements (94 > 50)
+ zerver/data_import/rocketchat.py:650:9: PLR0915 Too many statements (75 > 50)
- zerver/data_import/rocketchat.py:650:9: PLR0915 Too many statements (76 > 50)
+ zerver/data_import/slack.py:1338:5: PLR0915 Too many statements (52 > 50)
- zerver/data_import/slack.py:1338:5: PLR0915 Too many statements (53 > 50)
+ zerver/data_import/slack.py:487:5: PLR0915 Too many statements (80 > 50)
- zerver/data_import/slack.py:487:5: PLR0915 Too many statements (81 > 50)
+ zerver/data_import/slack.py:838:5: PLR0915 Too many statements (73 > 50)
- zerver/data_import/slack.py:838:5: PLR0915 Too many statements (74 > 50)
+ zerver/lib/email_notifications.py:194:5: PLR0915 Too many statements (69 > 50)
- zerver/lib/email_notifications.py:194:5: PLR0915 Too many statements (70 > 50)
+ zerver/lib/email_notifications.py:410:5: PLR0915 Too many statements (67 > 50)
- zerver/lib/email_notifications.py:410:5: PLR0915 Too many statements (68 > 50)
+ zerver/lib/events.py:104:5: PLR0915 Too many statements (231 > 50)
- zerver/lib/events.py:104:5: PLR0915 Too many statements (232 > 50)
+ zerver/lib/events.py:688:5: PLR0915 Too many statements (443 > 50)
- zerver/lib/events.py:688:5: PLR0915 Too many statements (444 > 50)
+ zerver/lib/export.py:552:5: PLR0915 Too many statements (65 > 50)
- zerver/lib/export.py:552:5: PLR0915 Too many statements (66 > 50)
+ zerver/lib/import_realm.py:747:5: PLR0915 Too many statements (87 > 50)
- zerver/lib/import_realm.py:747:5: PLR0915 Too many statements (88 > 50)
+ zerver/lib/import_realm.py:926:5: PLR0915 Too many statements (250 > 50)
- zerver/lib/import_realm.py:926:5: PLR0915 Too many statements (251 > 50)
+ zerver/lib/markdown/__init__.py:1148:9: PLR0915 Too many statements (79 > 50)
- zerver/lib/markdown/__init__.py:1148:9: PLR0915 Too many statements (80 > 50)
- zerver/lib/markdown/__init__.py:2446:5: PLR0915 Too many statements (51 > 50)
+ zerver/lib/message.py:1063:5: PLR0915 Too many statements (52 > 50)
- zerver/lib/message.py:1063:5: PLR0915 Too many statements (53 > 50)
+ zerver/lib/request.py:339:5: PLR0915 Too many statements (86 > 50)
- zerver/lib/request.py:339:5: PLR0915 Too many statements (87 > 50)
+ zerver/lib/request.py:370:9: PLR0915 Too many statements (70 > 50)
- zerver/lib/request.py:370:9: PLR0915 Too many statements (71 > 50)
+ zerver/lib/send_email.py:75:5: PLR0915 Too many statements (55 > 50)
- zerver/lib/send_email.py:75:5: PLR0915 Too many statements (56 > 50)
+ zerver/lib/test_classes.py:1587:9: PLR0915 Too many statements (61 > 50)
- zerver/lib/test_classes.py:1587:9: PLR0915 Too many statements (62 > 50)
+ zerver/lib/test_helpers.py:439:5: PLR0915 Too many statements (51 > 50)
- zerver/lib/test_helpers.py:439:5: PLR0915 Too many statements (52 > 50)
- zerver/management/commands/export.py:109:9: PLR0915 Too many statements (51 > 50)
+ zerver/management/commands/export_search.py:97:9: PLR0915 Too many statements (64 > 50)
- zerver/management/commands/export_search.py:97:9: PLR0915 Too many statements (65 > 50)
+ zerver/middleware.py:122:5: PLR0915 Too many statements (69 > 50)
- zerver/middleware.py:122:5: PLR0915 Too many statements (70 > 50)
+ zerver/migrations/0423_fix_email_gateway_attachment_owner.py:10:5: PLR0915 Too many statements (55 > 50)
- zerver/migrations/0423_fix_email_gateway_attachment_owner.py:10:5: PLR0915 Too many statements (56 > 50)
+ zerver/tests/test_auth_backends.py:1384:9: PLR0915 Too many statements (52 > 50)
- zerver/tests/test_auth_backends.py:1384:9: PLR0915 Too many statements (53 > 50)
- zerver/tests/test_auth_backends.py:176:9: PLR0915 Too many statements (51 > 50)
+ zerver/tests/test_custom_profile_data.py:243:9: PLR0915 Too many statements (55 > 50)
- zerver/tests/test_custom_profile_data.py:243:9: PLR0915 Too many statements (56 > 50)
+ zerver/tests/test_decorators.py:1052:9: PLR0915 Too many statements (52 > 50)
- zerver/tests/test_decorators.py:1052:9: PLR0915 Too many statements (53 > 50)
+ zerver/tests/test_decorators.py:301:9: PLR0915 Too many statements (85 > 50)
- zerver/tests/test_decorators.py:301:9: PLR0915 Too many statements (86 > 50)
+ zerver/tests/test_docs.py:449:9: PLR0915 Too many statements (54 > 50)
- zerver/tests/test_docs.py:449:9: PLR0915 Too many statements (55 > 50)
- zerver/tests/test_email_notifications.py:1856:9: PLR0915 Too many statements (51 > 50)
+ zerver/tests/test_events.py:3055:9: PLR0915 Too many statements (71 > 50)
- zerver/tests/test_events.py:3055:9: PLR0915 Too many statements (72 > 50)
+ zerver/tests/test_home.py:831:9: PLR0915 Too many statements (79 > 50)
- zerver/tests/test_home.py:831:9: PLR0915 Too many statements (80 > 50)
+ zerver/tests/test_import_export.py:1022:9: PLR0915 Too many statements (116 > 50)
- zerver/tests/test_import_export.py:1022:9: PLR0915 Too many statements (117 > 50)
+ zerver/tests/test_import_export.py:1642:9: PLR0915 Too many statements (107 > 50)
- zerver/tests/test_import_export.py:1642:9: PLR0915 Too many statements (108 > 50)
+ zerver/tests/test_import_export.py:506:9: PLR0915 Too many statements (79 > 50)
- zerver/tests/test_import_export.py:506:9: PLR0915 Too many statements (80 > 50)
+ zerver/tests/test_import_export.py:706:9: PLR0915 Too many statements (106 > 50)
- zerver/tests/test_import_export.py:706:9: PLR0915 Too many statements (107 > 50)
+ zerver/tests/test_invite.py:155:9: PLR0915 Too many statements (58 > 50)
- zerver/tests/test_invite.py:155:9: PLR0915 Too many statements (59 > 50)
+ zerver/tests/test_invite.py:733:9: PLR0915 Too many statements (54 > 50)
- zerver/tests/test_invite.py:733:9: PLR0915 Too many statements (55 > 50)
+ zerver/tests/test_logging_handlers.py:145:9: PLR0915 Too many statements (80 > 50)
- zerver/tests/test_logging_handlers.py:145:9: PLR0915 Too many statements (81 > 50)
+ zerver/tests/test_markdown.py:1097:9: PLR0915 Too many statements (60 > 50)
- zerver/tests/test_markdown.py:1097:9: PLR0915 Too many statements (61 > 50)
+ zerver/tests/test_mattermost_importer.py:172:9: PLR0915 Too many statements (51 > 50)
- zerver/tests/test_mattermost_importer.py:172:9: PLR0915 Too many statements (52 > 50)
+ zerver/tests/test_mattermost_importer.py:621:9: PLR0915 Too many statements (51 > 50)
- zerver/tests/test_mattermost_importer.py:621:9: PLR0915 Too many statements (52 > 50)
+ zerver/tests/test_mattermost_importer.py:724:9: PLR0915 Too many statements (74 > 50)
- zerver/tests/test_mattermost_importer.py:724:9: PLR0915 Too many statements (75 > 50)
+ zerver/tests/test_message_edit.py:1072:9: PLR0915 Too many statements (76 > 50)
- zerver/tests/test_message_edit.py:1072:9: PLR0915 Too many statements (77 > 50)
+ zerver/tests/test_message_edit.py:1310:9: PLR0915 Too many statements (112 > 50)
- zerver/tests/test_message_edit.py:1310:9: PLR0915 Too many statements (113 > 50)
+ zerver/tests/test_message_edit.py:1578:9: PLR0915 Too many statements (53 > 50)
- zerver/tests/test_message_edit.py:1578:9: PLR0915 Too many statements (54 > 50)
+ zerver/tests/test_message_edit.py:1728:9: PLR0915 Too many statements (58 > 50)
- zerver/tests/test_message_edit.py:1728:9: PLR0915 Too many statements (59 > 50)
+ zerver/tests/test_message_edit.py:2287:9: PLR0915 Too many statements (73 > 50)
- zerver/tests/test_message_edit.py:2287:9: PLR0915 Too many statements (74 > 50)
+ zerver/tests/test_message_edit.py:4308:9: PLR0915 Too many statements (61 > 50)
- zerver/tests/test_message_edit.py:4308:9: PLR0915 Too many statements (62 > 50)
+ zerver/tests/test_message_edit.py:775:9: PLR0915 Too many statements (111 > 50)
- zerver/tests/test_message_edit.py:775:9: PLR0915 Too many statements (112 > 50)
- zerver/tests/test_message_edit.py:984:9: PLR0915 Too many statements (51 > 50)
+ zerver/tests/test_message_fetch.py:2523:9: PLR0915 Too many statements (54 > 50)
- zerver/tests/test_message_fetch.py:2523:9: PLR0915 Too many statements (55 > 50)
+ zerver/tests/test_message_fetch.py:2819:9: PLR0915 Too many statements (200 > 50)
- zerver/tests/test_message_fetch.py:2819:9: PLR0915 Too many statements (201 > 50)
+ zerver/tests/test_message_fetch.py:663:9: PLR0915 Too many statements (56 > 50)
- zerver/tests/test_message_fetch.py:663:9: PLR0915 Too many statements (57 > 50)
+ zerver/tests/test_message_fetch.py:809:9: PLR0915 Too many statements (58 > 50)
- zerver/tests/test_message_fetch.py:809:9: PLR0915 Too many statements (59 > 50)
+ zerver/tests/test_message_flags.py:441:9: PLR0915 Too many statements (66 > 50)
- zerver/tests/test_message_flags.py:441:9: PLR0915 Too many statements (67 > 50)
+ zerver/tests/test_message_flags.py:950:9: PLR0915 Too many statements (96 > 50)
- zerver/tests/test_message_flags.py:950:9: PLR0915 Too many statements (97 > 50)
+ zerver/tests/test_push_notifications.py:464:9: PLR0915 Too many statements (51 > 50)
- zerver/tests/test_push_notifications.py:464:9: PLR0915 Too many statements (52 > 50)
+ zerver/tests/test_push_notifications.py:588:9: PLR0915 Too many statements (59 > 50)
- zerver/tests/test_push_notifications.py:588:9: PLR0915 Too many statements (60 > 50)
+ zerver/tests/test_queue_worker.py:114:9: PLR0915 Too many statements (98 > 50)
- zerver/tests/test_queue_worker.py:114:9: PLR0915 Too many statements (99 > 50)
+ zerver/tests/test_reactions.py:508:9: PLR0915 Too many statements (64 > 50)
- zerver/tests/test_reactions.py:508:9: PLR0915 Too many statements (65 > 50)
+ zerver/tests/test_read_receipts.py:213:9: PLR0915 Too many statements (54 > 50)
- zerver/tests/test_read_receipts.py:213:9: PLR0915 Too many statements (55 > 50)
+ zerver/tests/test_realm.py:1461:9: PLR0915 Too many statements (67 > 50)
- zerver/tests/test_realm.py:1461:9: PLR0915 Too many statements (68 > 50)
+ zerver/tests/test_realm_export.py:116:9: PLR0915 Too many statements (58 > 50)
- zerver/tests/test_realm_export.py:116:9: PLR0915 Too many statements (59 > 50)
+ zerver/tests/test_realm_linkifiers.py:25:9: PLR0915 Too many statements (81 > 50)
- zerver/tests/test_realm_linkifiers.py:25:9: PLR0915 Too many statements (82 > 50)
+ zerver/tests/test_rocketchat_importer.py:870:9: PLR0915 Too many statements (83 > 50)
- zerver/tests/test_rocketchat_importer.py:870:9: PLR0915 Too many statements (84 > 50)
+ zerver/tests/test_rocketchat_importer.py:87:9: PLR0915 Too many statements (56 > 50)
- zerver/tests/test_rocketchat_importer.py:87:9: PLR0915 Too many statements (57 > 50)
+ zerver/tests/test_scheduled_messages.py:470:9: PLR0915 Too many statements (66 > 50)
- zerver/tests/test_scheduled_messages.py:470:9: PLR0915 Too many statements (67 > 50)
+ zerver/tests/test_signup.py:1252:9: PLR0915 Too many statements (51 > 50)
- zerver/tests/test_signup.py:1252:9: PLR0915 Too many statements (52 > 50)
+ zerver/tests/test_slack_importer.py:271:9: PLR0915 Too many statements (75 > 50)
- zerver/tests/test_slack_importer.py:271:9: PLR0915 Too many statements (76 > 50)
+ zerver/tests/test_soft_deactivation.py:351:9: PLR0915 Too many statements (115 > 50)
- zerver/tests/test_soft_deactivation.py:351:9: PLR0915 Too many statements (116 > 50)
+ zerver/tests/test_soft_deactivation.py:574:9: PLR0915 Too many statements (100 > 50)
- zerver/tests/test_soft_deactivation.py:574:9: PLR0915 Too many statements (101 > 50)
+ zerver/tests/test_subs.py:1045:9: PLR0915 Too many statements (86 > 50)
- zerver/tests/test_subs.py:1045:9: PLR0915 Too many statements (87 > 50)
+ zerver/tests/test_subs.py:1406:9: PLR0915 Too many statements (64 > 50)
- zerver/tests/test_subs.py:1406:9: PLR0915 Too many statements (65 > 50)
+ zerver/tests/test_subs.py:2841:9: PLR0915 Too many statements (91 > 50)
- zerver/tests/test_subs.py:2841:9: PLR0915 Too many statements (92 > 50)
+ zerver/tests/test_subs.py:5798:9: PLR0915 Too many statements (51 > 50)
- zerver/tests/test_subs.py:5798:9: PLR0915 Too many statements (52 > 50)
+ zerver/tests/test_upload.py:356:9: PLR0915 Too many statements (65 > 50)
- zerver/tests/test_upload.py:356:9: PLR0915 Too many statements (66 > 50)
+ zerver/tests/test_upload_s3.py:180:9: PLR0915 Too many statements (54 > 50)
- zerver/tests/test_upload_s3.py:180:9: PLR0915 Too many statements (55 > 50)
+ zerver/tests/test_user_groups.py:352:9: PLR0915 Too many statements (64 > 50)
- zerver/tests/test_user_groups.py:352:9: PLR0915 Too many statements (65 > 50)
+ zerver/tests/test_user_groups.py:508:9: PLR0915 Too many statements (53 > 50)
- zerver/tests/test_user_groups.py:508:9: PLR0915 Too many statements (54 > 50)
+ zerver/tests/test_user_groups.py:707:9: PLR0915 Too many statements (62 > 50)
- zerver/tests/test_user_groups.py:707:9: PLR0915 Too many statements (63 > 50)
+ zerver/tests/test_user_groups.py:897:9: PLR0915 Too many statements (53 > 50)
- zerver/tests/test_user_groups.py:897:9: PLR0915 Too many statements (54 > 50)
+ zerver/tests/test_users.py:1716:9: PLR0915 Too many statements (88 > 50)
- zerver/tests/test_users.py:1716:9: PLR0915 Too many statements (89 > 50)
+ zerver/tests/test_users.py:2049:9: PLR0915 Too many statements (57 > 50)
- zerver/tests/test_users.py:2049:9: PLR0915 Too many statements (58 > 50)
+ zerver/tests/test_users.py:863:9: PLR0915 Too many statements (56 > 50)
- zerver/tests/test_users.py:863:9: PLR0915 Too many statements (57 > 50)
+ zerver/tornado/event_queue.py:916:5: PLR0915 Too many statements (56 > 50)
- zerver/tornado/event_queue.py:916:5: PLR0915 Too many statements (57 > 50)
- zerver/views/auth.py:154:5: PLR0915 Too many statements (51 > 50)
+ zerver/views/documentation.py:143:9: PLR0915 Too many statements (64 > 50)
- zerver/views/documentation.py:143:9: PLR0915 Too many statements (65 > 50)
+ zerver/views/message_fetch.py:82:5: PLR0915 Too many statements (63 > 50)
- zerver/views/message_fetch.py:82:5: PLR0915 Too many statements (64 > 50)
+ zerver/views/realm.py:46:5: PLR0915 Too many statements (88 > 50)
- zerver/views/realm.py:46:5: PLR0915 Too many statements (89 > 50)
+ zerver/views/registration.py:207:5: PLR0915 Too many statements (167 > 50)
- zerver/views/registration.py:207:5: PLR0915 Too many statements (168 > 50)
+ zerver/views/streams.py:254:5: PLR0915 Too many statements (71 > 50)
- zerver/views/streams.py:254:5: PLR0915 Too many statements (72 > 50)
+ zerver/views/streams.py:557:5: PLR0915 Too many statements (53 > 50)
- zerver/views/streams.py:557:5: PLR0915 Too many statements (54 > 50)
+ zerver/views/users.py:340:5: PLR0915 Too many statements (55 > 50)
- zerver/views/users.py:340:5: PLR0915 Too many statements (56 > 50)
+ zerver/views/users.py:461:5: PLR0915 Too many statements (59 > 50)
- zerver/views/users.py:461:5: PLR0915 Too many statements (60 > 50)
+ zerver/webhooks/pivotal/view.py:89:5: PLR0915 Too many statements (56 > 50)
- zerver/webhooks/pivotal/view.py:89:5: PLR0915 Too many statements (57 > 50)
+ zerver/webhooks/stripe/view.py:74:5: PLR0915 Too many statements (101 > 50)
- zerver/webhooks/stripe/view.py:74:5: PLR0915 Too many statements (102 > 50)
+ zerver/webhooks/taiga/view.py:207:5: PLR0915 Too many statements (59 > 50)
- zerver/webhooks/taiga/view.py:207:5: PLR0915 Too many statements (60 > 50)
+ zerver/worker/queue_processors.py:1002:9: PLR0915 Too many statements (71 > 50)
- zerver/worker/queue_processors.py:1002:9: PLR0915 Too many statements (72 > 50)
+ zilencer/management/commands/populate_db.py:1034:5: PLR0915 Too many statements (69 > 50)
- zilencer/management/commands/populate_db.py:1034:5: PLR0915 Too many statements (70 > 50)
+ zilencer/management/commands/populate_db.py:289:9: PLR0915 Too many statements (196 > 50)
- zilencer/management/commands/populate_db.py:289:9: PLR0915 Too many statements (197 > 50)
+ zproject/backends.py:1511:5: PLR0915 Too many statements (64 > 50)
- zproject/backends.py:1511:5: PLR0915 Too many statements (65 > 50)
+ zproject/backends.py:1687:5: PLR0915 Too many statements (55 > 50)
- zproject/backends.py:1687:5: PLR0915 Too many statements (56 > 50)
+ zproject/backends.py:958:9: PLR0915 Too many statements (52 > 50)
- zproject/backends.py:958:9: PLR0915 Too many statements (53 > 50)
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLR0915 | 453 | 219 | 234 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.8±0.26ms     2.4 MB/sec    1.00     16.8±0.08ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.05ms     4.2 MB/sec    1.02      4.0±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    490.9±8.01µs     6.0 MB/sec    1.02    501.0±1.76µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.09ms     3.7 MB/sec    1.01      7.0±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.13ms     5.1 MB/sec    1.02      8.1±0.03ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1781.4±16.69µs     9.3 MB/sec    1.00   1783.9±7.07µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   203.0±10.40µs    14.5 MB/sec    1.00    202.3±0.47µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     7.0 MB/sec    1.00      3.7±0.01ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.0±0.10ms     6.8 MB/sec    1.01      6.1±0.06ms     6.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1178.8±19.36µs    14.1 MB/sec    1.02  1201.2±14.52µs    13.9 MB/sec
parser/numpy/globals.py                    1.00    121.8±2.44µs    24.2 MB/sec    1.03    126.0±0.85µs    23.4 MB/sec
parser/pydantic/types.py                   1.00      2.6±0.04ms     9.8 MB/sec    1.03      2.7±0.01ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.1±0.24ms     2.4 MB/sec    1.02     17.4±0.26ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.08ms     3.9 MB/sec    1.01      4.3±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    502.5±6.88µs     5.9 MB/sec    1.01    508.8±7.76µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.40ms     3.5 MB/sec    1.01      7.3±0.11ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.07ms     4.8 MB/sec    1.05      8.8±0.10ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1773.8±23.44µs     9.4 MB/sec    1.05  1860.3±33.65µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    206.0±3.91µs    14.3 MB/sec    1.03    211.3±8.34µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.7 MB/sec    1.05      4.0±0.06ms     6.4 MB/sec
parser/large/dataset.py                    1.01      6.4±0.05ms     6.4 MB/sec    1.00      6.3±0.04ms     6.4 MB/sec
parser/numpy/ctypeslib.py                  1.00  1203.9±13.14µs    13.8 MB/sec    1.00  1204.0±12.25µs    13.8 MB/sec
parser/numpy/globals.py                    1.00    123.6±1.75µs    23.9 MB/sec    1.00    123.7±2.09µs    23.8 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.04ms     9.3 MB/sec    1.00      2.7±0.03ms     9.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
