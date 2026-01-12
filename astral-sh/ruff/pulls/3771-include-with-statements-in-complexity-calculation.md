```yaml
number: 3771
title: "Include `with` statements in complexity calculation"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/with
created_at: 2023-03-28T15:11:12Z
updated_at: 2023-03-28T15:41:31Z
url: https://github.com/astral-sh/ruff/pull/3771
synced_at: 2026-01-12T04:39:45Z
```

# Include `with` statements in complexity calculation

---

_Pull request opened by @charliermarsh on 2023-03-28 15:11_

## Summary

Right now, we're omitting code within `with` bodies.

Closes #3770.


---

_Renamed from "Include with statements in complexity calculation" to "Include `with` statements in complexity calculation" by @charliermarsh on 2023-03-28 15:11_

---

_Label `bug` added by @charliermarsh on 2023-03-28 15:11_

---

_Merged by @charliermarsh on 2023-03-28 15:20_

---

_Closed by @charliermarsh on 2023-03-28 15:20_

---

_Branch deleted on 2023-03-28 15:20_

---

_Comment by @github-actions[bot] on 2023-03-28 15:28_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+42, -20, 0 error(s))

<details><summary>airflow (+31, -13)</summary>
<p>

```diff
- airflow/cli/commands/celery_command.py:100:5: C901 `worker` is too complex (11 > 10)
+ airflow/cli/commands/celery_command.py:100:5: C901 `worker` is too complex (12 > 10)
- airflow/cli/commands/connection_command.py:200:5: C901 `connections_add` is too complex (15 > 10)
+ airflow/cli/commands/connection_command.py:200:5: C901 `connections_add` is too complex (16 > 10)
+ airflow/cli/commands/role_command.py:88:5: C901 `__roles_add_or_remove_permissions` is too complex (12 > 10)
- airflow/cli/commands/task_command.py:347:5: C901 `task_run` is too complex (11 > 10)
+ airflow/cli/commands/task_command.py:347:5: C901 `task_run` is too complex (12 > 10)
- airflow/cli/commands/task_command.py:563:5: C901 `task_test` is too complex (11 > 10)
+ airflow/cli/commands/task_command.py:563:5: C901 `task_test` is too complex (12 > 10)
- airflow/cli/commands/webserver_command.py:323:5: C901 `webserver` is too complex (15 > 10)
+ airflow/cli/commands/webserver_command.py:323:5: C901 `webserver` is too complex (17 > 10)
+ airflow/migrations/versions/0093_2_2_0_taskinstance_keyed_to_dagrun.py:67:5: C901 `upgrade` is too complex (11 > 10)
+ airflow/models/dagbag.py:371:9: C901 `_load_modules_from_zip` is too complex (11 > 10)
- airflow/models/dagrun.py:522:9: C901 `update_state` is too complex (14 > 10)
+ airflow/models/dagrun.py:522:9: C901 `update_state` is too complex (17 > 10)
+ airflow/models/taskinstance.py:1605:9: C901 `_execute_task` is too complex (11 > 10)
+ airflow/providers/apache/hive/hooks/hive.py:205:9: C901 `run_cli` is too complex (12 > 10)
+ airflow/providers/apache/hive/transfers/s3_to_hive.py:137:9: C901 `execute` is too complex (13 > 10)
+ airflow/providers/apache/livy/hooks/livy.py:492:15: C901 `_do_api_call_async` is too complex (13 > 10)
+ airflow/providers/common/sql/hooks/sql.py:268:9: C901 `run` is too complex (11 > 10)
+ airflow/providers/databricks/operators/databricks_sql.py:123:9: C901 `_process_output` is too complex (12 > 10)
+ airflow/providers/google/cloud/operators/gcs.py:737:9: C901 `execute` is too complex (16 > 10)
+ airflow/providers/http/hooks/http.py:303:15: C901 `run` is too complex (19 > 10)
+ airflow/serialization/serde.py:303:5: C901 `_register` is too complex (11 > 10)
- airflow/utils/db.py:1515:5: C901 `upgradedb` is too complex (13 > 10)
+ airflow/utils/db.py:1515:5: C901 `upgradedb` is too complex (14 > 10)
+ airflow/www/views.py:3738:9: C901 `datasets_summary` is too complex (12 > 10)
- airflow/www/views.py:685:9: C901 `index` is too complex (13 > 10)
+ airflow/www/views.py:685:9: C901 `index` is too complex (24 > 10)
+ dev/breeze/src/airflow_breeze/commands/release_management_commands.py:786:5: C901 `generate_issue_content_providers` is too complex (13 > 10)
- dev/breeze/src/airflow_breeze/utils/run_utils.py:52:5: C901 `run_command` is too complex (15 > 10)
+ dev/breeze/src/airflow_breeze/utils/run_utils.py:52:5: C901 `run_command` is too complex (23 > 10)
+ dev/prepare_bulk_issues.py:186:5: C901 `prepare_bulk_issues` is too complex (11 > 10)
+ dev/prepare_release_issue.py:248:5: C901 `generate_issue_content` is too complex (12 > 10)
- docs/build_docs.py:449:5: C901 `main` is too complex (12 > 10)
+ docs/build_docs.py:449:5: C901 `main` is too complex (14 > 10)
- docs/exts/airflow_intersphinx.py:101:9: C901 `main` is too complex (14 > 10)
+ docs/exts/airflow_intersphinx.py:101:9: C901 `main` is too complex (15 > 10)
- scripts/in_container/verify_providers.py:204:5: C901 `import_all_classes` is too complex (18 > 10)
+ scripts/in_container/verify_providers.py:204:5: C901 `import_all_classes` is too complex (20 > 10)
- tests/conftest.py:459:5: C901 `dag_maker` is too complex (30 > 10)
+ tests/conftest.py:459:5: C901 `dag_maker` is too complex (32 > 10)
+ tests/models/test_dagbag.py:496:9: C901 `test_load_subdags` is too complex (12 > 10)
+ tests/test_utils/perf/scheduler_dag_execution_timing.py:206:5: C901 `main` is too complex (11 > 10)
```

</p>
</details>
<details><summary>bokeh (+3, -2)</summary>
<p>

```diff
- src/bokeh/command/subcommands/serve.py:797:9: C901 `invoke` is too complex (31 > 10)
+ src/bokeh/command/subcommands/serve.py:797:9: C901 `invoke` is too complex (38 > 10)
- src/bokeh/embed/bundle.py:282:5: C901 `_bundle_extensions` is too complex (13 > 10)
+ src/bokeh/embed/bundle.py:282:5: C901 `_bundle_extensions` is too complex (14 > 10)
+ tests/unit/bokeh/test_client_server.py:798:9: C901 `test_lots_of_concurrent_messages` is too complex (13 > 10)
```

</p>
</details>
<details><summary>zulip (+8, -5)</summary>
<p>

```diff
- zerver/actions/create_realm.py:137:5: C901 `do_create_realm` is too complex (18 > 10)
+ zerver/actions/create_realm.py:137:5: C901 `do_create_realm` is too complex (20 > 10)
- zerver/actions/message_send.py:745:5: C901 `do_send_messages` is too complex (18 > 10)
+ zerver/actions/message_send.py:745:5: C901 `do_send_messages` is too complex (23 > 10)
- zerver/lib/import_realm.py:752:5: C901 `import_uploads` is too complex (25 > 10)
+ zerver/lib/import_realm.py:752:5: C901 `import_uploads` is too complex (26 > 10)
+ zerver/lib/markdown/__init__.py:491:5: C901 `fetch_open_graph_image` is too complex (12 > 10)
- zerver/lib/test_helpers.py:439:5: C901 `write_instrumentation_reports` is too complex (23 > 10)
+ zerver/lib/test_helpers.py:439:5: C901 `write_instrumentation_reports` is too complex (24 > 10)
+ zerver/management/commands/runtornado.py:47:9: C901 `handle` is too complex (13 > 10)
+ zerver/tests/test_docs.py:41:9: C901 `_test` is too complex (12 > 10)
- zerver/views/streams.py:691:5: C901 `send_messages_for_new_subscribers` is too complex (11 > 10)
+ zerver/views/streams.py:691:5: C901 `send_messages_for_new_subscribers` is too complex (13 > 10)
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.9±0.05ms     2.9 MB/sec    1.00     14.0±0.05ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    470.6±0.62µs     6.3 MB/sec    1.00    469.6±1.04µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.02ms     4.3 MB/sec    1.00      6.0±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.03ms     5.6 MB/sec    1.00      7.2±0.03ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1638.0±11.66µs    10.2 MB/sec    1.00   1633.4±2.54µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    184.3±0.42µs    16.0 MB/sec    1.00    185.0±1.01µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.6 MB/sec    1.00      3.4±0.01ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     19.4±0.69ms     2.1 MB/sec    1.00     19.3±0.51ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      5.1±0.16ms     3.3 MB/sec    1.00      4.8±0.11ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.02   604.6±23.96µs     4.9 MB/sec    1.00   591.3±18.29µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.03      8.2±0.25ms     3.1 MB/sec    1.00      8.0±0.36ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.02     10.1±0.25ms     4.0 MB/sec    1.00      9.9±0.24ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.06ms     7.6 MB/sec    1.01      2.2±0.08ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.03    237.6±9.99µs    12.4 MB/sec    1.00    231.6±8.68µs    12.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.16ms     5.6 MB/sec    1.01      4.6±0.20ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
