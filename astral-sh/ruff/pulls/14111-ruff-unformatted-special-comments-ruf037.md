```yaml
number: 14111
title: "[`ruff`] Unformatted special comments (`RUF037`)"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - rule
  - needs-decision
  - preview
assignees: []
base: main
head: RUF102
created_at: 2024-11-05T16:23:10Z
updated_at: 2024-12-19T16:41:15Z
url: https://github.com/astral-sh/ruff/pull/14111
synced_at: 2026-01-12T15:55:46Z
```

# [`ruff`] Unformatted special comments (`RUF037`)

---

_@InSyncWithFoo_

## Summary

Resolves #10160.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @InSyncWithFoo on 2024-11-05 16:26_

I'm not sure why the snapshot is empty. Debugging showed that new diagnostics are correctly added, yet it's almost as if the rule never ran.

---

_Comment by @github-actions[bot] on 2024-11-05 16:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1074 -0 violations, +0 -0 fixes in 11 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/agent.py#L422'>rasa/core/agent.py:422:95:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/tensorflow/model_data.py#L106'>rasa/utils/tensorflow/model_data.py:106:110:</a> RUF037 [*] Unformatted special comment
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+90 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/951b3a13bb3950b979650744223b328b6ebb2bb5/airflow/www/views.py#L4936'>airflow/www/views.py:4936:62:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/apache/airflow/blob/951b3a13bb3950b979650744223b328b6ebb2bb5/dev/breeze/src/airflow_breeze/configure_rich_click.py#L21'>dev/breeze/src/airflow_breeze/configure_rich_click.py:21:47:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/apache/airflow/blob/951b3a13bb3950b979650744223b328b6ebb2bb5/docs/exts/docs_build/errors.py#L27'>docs/exts/docs_build/errors.py:27:62:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/apache/airflow/blob/951b3a13bb3950b979650744223b328b6ebb2bb5/kubernetes_tests/test_kubernetes_executor.py#L25'>kubernetes_tests/test_kubernetes_executor.py:25:21:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/apache/airflow/blob/951b3a13bb3950b979650744223b328b6ebb2bb5/kubernetes_tests/test_other_executors.py#L25'>kubernetes_tests/test_other_executors.py:25:21:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/apache/airflow/blob/951b3a13bb3950b979650744223b328b6ebb2bb5/providers/src/airflow/providers/edge/models/edge_worker.py#L137'>providers/src/airflow/providers/edge/models/edge_worker.py:137:36:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/apache/airflow/blob/951b3a13bb3950b979650744223b328b6ebb2bb5/providers/src/airflow/providers/edge/models/edge_worker.py#L154'>providers/src/airflow/providers/edge/models/edge_worker.py:154:41:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/apache/airflow/blob/951b3a13bb3950b979650744223b328b6ebb2bb5/providers/src/airflow/providers/google/cloud/hooks/bigquery.py#L1154'>providers/src/airflow/providers/google/cloud/hooks/bigquery.py:1154:33:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/apache/airflow/blob/951b3a13bb3950b979650744223b328b6ebb2bb5/providers/src/airflow/providers/google/cloud/hooks/compute_ssh.py#L39'>providers/src/airflow/providers/google/cloud/hooks/compute_ssh.py:39:20:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/apache/airflow/blob/951b3a13bb3950b979650744223b328b6ebb2bb5/providers/src/airflow/providers/google/cloud/hooks/mlengine.py#L600'>providers/src/airflow/providers/google/cloud/hooks/mlengine.py:600:42:</a> RUF037 [*] Unformatted special comment
... 80 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+46 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/58f9be9b85cfd34f861d232cf834c96747133a39/superset/commands/chart/export.py#L17'>superset/commands/chart/export.py:17:3:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/apache/superset/blob/58f9be9b85cfd34f861d232cf834c96747133a39/superset/commands/dashboard/export.py#L17'>superset/commands/dashboard/export.py:17:3:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/apache/superset/blob/58f9be9b85cfd34f861d232cf834c96747133a39/superset/commands/database/export.py#L17'>superset/commands/database/export.py:17:3:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/apache/superset/blob/58f9be9b85cfd34f861d232cf834c96747133a39/superset/commands/dataset/export.py#L17'>superset/commands/dataset/export.py:17:3:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/apache/superset/blob/58f9be9b85cfd34f861d232cf834c96747133a39/superset/commands/query/export.py#L17'>superset/commands/query/export.py:17:3:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/apache/superset/blob/58f9be9b85cfd34f861d232cf834c96747133a39/tests/integration_tests/access_tests.py#L17'>tests/integration_tests/access_tests.py:17:3:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/apache/superset/blob/58f9be9b85cfd34f861d232cf834c96747133a39/tests/integration_tests/advanced_data_type/api_tests.py#L17'>tests/integration_tests/advanced_data_type/api_tests.py:17:3:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/apache/superset/blob/58f9be9b85cfd34f861d232cf834c96747133a39/tests/integration_tests/annotation_layers/api_tests.py#L17'>tests/integration_tests/annotation_layers/api_tests.py:17:3:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/apache/superset/blob/58f9be9b85cfd34f861d232cf834c96747133a39/tests/integration_tests/base_api_tests.py#L17'>tests/integration_tests/base_api_tests.py:17:3:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/apache/superset/blob/58f9be9b85cfd34f861d232cf834c96747133a39/tests/integration_tests/base_tests.py#L17'>tests/integration_tests/base_tests.py:17:3:</a> RUF037 [*] Unformatted special comment
... 36 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+872 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/ui.py#L27'>release/ui.py:27:24:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L36'>src/bokeh/__init__.py:36:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L88'>src/bokeh/__init__.py:88:28:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L91'>src/bokeh/__init__.py:91:31:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L96'>src/bokeh/__init__.py:96:19:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L97'>src/bokeh/__init__.py:97:72:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__main__.py#L27'>src/bokeh/__main__.py:27:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/__init__.py#L22'>src/bokeh/application/__init__.py:22:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L24'>src/bokeh/application/application.py:24:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/__init__.py#L17'>src/bokeh/application/handlers/__init__.py:17:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L31'>src/bokeh/application/handlers/code.py:31:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L17'>src/bokeh/application/handlers/code_runner.py:17:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L43'>src/bokeh/application/handlers/directory.py:43:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/document_lifecycle.py#L17'>src/bokeh/application/handlers/document_lifecycle.py:17:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/function.py#L33'>src/bokeh/application/handlers/function.py:33:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/handler.py#L38'>src/bokeh/application/handlers/handler.py:38:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/lifecycle.py#L17'>src/bokeh/application/handlers/lifecycle.py:17:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/notebook.py#L24'>src/bokeh/application/handlers/notebook.py:24:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/request_handler.py#L17'>src/bokeh/application/handlers/request_handler.py:17:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/script.py#L41'>src/bokeh/application/handlers/script.py:41:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/server_lifecycle.py#L17'>src/bokeh/application/handlers/server_lifecycle.py:17:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/server_request_handler.py#L17'>src/bokeh/application/handlers/server_request_handler.py:17:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/__init__.py#L26'>src/bokeh/client/__init__.py:26:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L20'>src/bokeh/client/connection.py:20:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/session.py#L29'>src/bokeh/client/session.py:29:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L17'>src/bokeh/client/states.py:17:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/util.py#L16'>src/bokeh/client/util.py:16:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/websocket.py#L17'>src/bokeh/client/websocket.py:17:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/__init__.py#L17'>src/bokeh/colors/__init__.py:17:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L16'>src/bokeh/colors/color.py:16:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/groups.py#L16'>src/bokeh/colors/groups.py:16:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/named.py#L16'>src/bokeh/colors/named.py:16:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/util.py#L16'>src/bokeh/colors/util.py:16:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/bootstrap.py#L38'>src/bokeh/command/bootstrap.py:38:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommand.py#L17'>src/bokeh/command/subcommand.py:17:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/__init__.py#L16'>src/bokeh/command/subcommands/__init__.py:16:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/build.py#L16'>src/bokeh/command/subcommands/build.py:16:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L16'>src/bokeh/command/subcommands/file_output.py:16:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/info.py#L50'>src/bokeh/command/subcommands/info.py:50:18:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/init.py#L16'>src/bokeh/command/subcommands/init.py:16:18:</a> RUF037 [*] Unformatted special comment
... 832 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/56ec3719fc2e8433dce2f5bea82e4283f443e2d1/.github/workflows/algolia/upload-algolia-api.py#L164'>.github/workflows/algolia/upload-algolia-api.py:164:56:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/ibis-project/ibis/blob/56ec3719fc2e8433dce2f5bea82e4283f443e2d1/.github/workflows/algolia/upload-algolia-api.py#L178'>.github/workflows/algolia/upload-algolia-api.py:178:58:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/ibis-project/ibis/blob/56ec3719fc2e8433dce2f5bea82e4283f443e2d1/.github/workflows/algolia/upload-algolia-api.py#L190'>.github/workflows/algolia/upload-algolia-api.py:190:68:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/ibis-project/ibis/blob/56ec3719fc2e8433dce2f5bea82e4283f443e2d1/.github/workflows/algolia/upload-algolia-api.py#L203'>.github/workflows/algolia/upload-algolia-api.py:203:55:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/ibis-project/ibis/blob/56ec3719fc2e8433dce2f5bea82e4283f443e2d1/.github/workflows/algolia/upload-algolia-api.py#L208'>.github/workflows/algolia/upload-algolia-api.py:208:68:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/ibis-project/ibis/blob/56ec3719fc2e8433dce2f5bea82e4283f443e2d1/ibis/backends/tests/test_client.py#L1475'>ibis/backends/tests/test_client.py:1475:39:</a> RUF037 [*] Unformatted special comment
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+17 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/b12cc0248c98c5dba04f3738d5906e2bdaa92feb/pytestgeventwrapper.py#L1'>pytestgeventwrapper.py:1:37:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/rotki/rotki/blob/b12cc0248c98c5dba04f3738d5906e2bdaa92feb/pytestgeventwrapper.py#L2'>pytestgeventwrapper.py:2:23:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/rotki/rotki/blob/b12cc0248c98c5dba04f3738d5906e2bdaa92feb/rotkehlchen/__main__.py#L1'>rotkehlchen/__main__.py:1:30:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/rotki/rotki/blob/b12cc0248c98c5dba04f3738d5906e2bdaa92feb/rotkehlchen/__main__.py#L2'>rotkehlchen/__main__.py:2:23:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/rotki/rotki/blob/b12cc0248c98c5dba04f3738d5906e2bdaa92feb/rotkehlchen/api/rest.py#L2748'>rotkehlchen/api/rest.py:2748:70:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/rotki/rotki/blob/b12cc0248c98c5dba04f3738d5906e2bdaa92feb/rotkehlchen/chain/aggregator.py#L312'>rotkehlchen/chain/aggregator.py:312:87:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/rotki/rotki/blob/b12cc0248c98c5dba04f3738d5906e2bdaa92feb/rotkehlchen/chain/aggregator.py#L313'>rotkehlchen/chain/aggregator.py:313:89:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/rotki/rotki/blob/b12cc0248c98c5dba04f3738d5906e2bdaa92feb/rotkehlchen/chain/aggregator.py#L316'>rotkehlchen/chain/aggregator.py:316:85:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/rotki/rotki/blob/b12cc0248c98c5dba04f3738d5906e2bdaa92feb/rotkehlchen/chain/aggregator.py#L319'>rotkehlchen/chain/aggregator.py:319:85:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/rotki/rotki/blob/b12cc0248c98c5dba04f3738d5906e2bdaa92feb/rotkehlchen/chain/zksync_lite/manager.py#L634'>rotkehlchen/chain/zksync_lite/manager.py:634:52:</a> RUF037 [*] Unformatted special comment
... 7 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/c4ad9d8e098746141416f9d0d7566b817d4db79b/zilencer/views.py#L809'>zilencer/views.py:809:53:</a> RUF037 [*] Unformatted special comment
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/f5853a71dd69942f6a7d59466793c7b50c88b606/indico/core/celery/__init__.py#L51'>indico/core/celery/__init__.py:51:34:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/indico/indico/blob/f5853a71dd69942f6a7d59466793c7b50c88b606/indico/core/db/sqlalchemy/__init__.py#L9'>indico/core/db/sqlalchemy/__init__.py:9:26:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/indico/indico/blob/f5853a71dd69942f6a7d59466793c7b50c88b606/indico/core/signals/event/__init__.py#L8'>indico/core/signals/event/__init__.py:8:3:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/indico/indico/blob/f5853a71dd69942f6a7d59466793c7b50c88b606/indico/modules/categories/serialize.py#L55'>indico/modules/categories/serialize.py:55:102:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/indico/indico/blob/f5853a71dd69942f6a7d59466793c7b50c88b606/indico/web/forms/fields/__init__.py#L8'>indico/web/forms/fields/__init__.py:8:3:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/indico/indico/blob/f5853a71dd69942f6a7d59466793c7b50c88b606/indico/web/http_api/__init__.py#L8'>indico/web/http_api/__init__.py:8:3:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/indico/indico/blob/f5853a71dd69942f6a7d59466793c7b50c88b606/indico/web/http_api/metadata/serializer.py#L53'>indico/web/http_api/metadata/serializer.py:53:65:</a> RUF037 [*] Unformatted special comment
+ <a href='https://github.com/indico/indico/blob/f5853a71dd69942f6a7d59466793c7b50c88b606/indico/web/http_api/metadata/serializer.py#L54'>indico/web/http_api/metadata/serializer.py:54:63:</a> RUF037 [*] Unformatted special comment
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/wntrblm/nox">wntrblm/nox</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/wntrblm/nox/blob/bdd84a4d135b9c7471aad664a168fd441e4dfb64/nox/_options.py#L278'>nox/_options.py:278:36:</a> RUF037 [*] Unformatted special comment
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF037 | 1074 | 1074 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "[`ruff`] Unformatted special comments (`RUF102`)" to "[`ruff`] Unformatted special comments (`RUF104`)" by @InSyncWithFoo on 2024-11-06 02:21_

---

_@MichaReiser reviewed on 2024-11-06 11:45_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unformatted_special_comment.rs`:2 on 2024-11-06 11:45_

You can use https://doc.rust-lang.org/std/sync/struct.LazyLock.html 

---

_Marked ready for review by @InSyncWithFoo on 2024-11-09 18:14_

---

_Converted to draft by @InSyncWithFoo on 2024-11-10 09:32_

---

_Marked ready for review by @InSyncWithFoo on 2024-11-10 17:09_

---

_Label `rule` added by @dhruvmanila on 2024-11-11 07:53_

---

_Label `preview` added by @dhruvmanila on 2024-11-11 07:53_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/noqa.rs`:57 on 2024-11-11 13:04_

Can you explain the reasoning for this special handling? I think I would find this behavior surprising and I'm not sure if it justifies the added complexity

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/mod.rs`:390 on 2024-11-11 13:05_

Are these files testing different aspects? If so, can we give them more meaningful names than `_1` etc.?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unformatted_special_comment.rs`:34 on 2024-11-11 13:08_

I'm somewhat hesitant supporting non-ruff suppression comments:
1. All other `RUF1XX` rules are specific to ruff pragma comments
2. I don't see us as the authority defining how `pyright` and `mypy` comments should be formatted.

---

_@MichaReiser reviewed on 2024-11-11 13:14_

Thanks for working on this rule. 

I think we need to define the scope of this rule more carefully, especially when it comes to non-ruff suppression comments.

1. All other `RUF1XX` rules are specific to ruff pragma comments
2. I don't see us as the authority defining how `pyright` and `mypy` comments should be formatted.

I see two possible versions of this rule:

* One specific to ruff's suppression comments that enforces consistent formatting, including how rule codes are formatted
* A general purpose rule that handles all kinds of pragma comments. It only enforces a single space between `#` and the keyword and, optionally, that the colon is followed by a space. We can add a configuration option to support custom pragma codes. This is not a `RUF1XX` rule. 



---

_@InSyncWithFoo reviewed on 2024-11-11 13:37_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/checkers/noqa.rs`:57 on 2024-11-11 13:37_

`blanket-noqa` (`PGH004`) gets similar treatment (right above, [line 48](https://github.com/astral-sh/ruff/pull/14111/files/9c5ec194216fd0bbe1e9b5b5be0476a122b72087#diff-e078b56e67f3e7989390caeeee1bbe9b7f301a66ed0ceeddb159eed523880bedR48)), and must either not be enabled or explicitly disabled using `per-file-ignores`/`# ruff: noqa: PGH004` ([line 223](https://github.com/astral-sh/ruff/blob/b3b5c191051a4ea2b81b11ed3088a09ec6d051fc/crates/ruff_linter/src/checkers/noqa.rs#L223-L225)):

```rust
if matches!(diagnostic.kind.rule(), Rule::BlanketNOQA) {
    continue;
}
```

```rust
if settings.rules.enabled(Rule::BlanketNOQA)
    && !per_file_ignores.contains(Rule::BlanketNOQA)
    && !exemption.enumerates(Rule::BlanketNOQA)
```

`RUF104` isn't a `noqa`-only rule, so `unformatted_special_comment()` shouldn't be called from `check_noqa()`. However, `exemption` is [computed](https://github.com/astral-sh/ruff/blob/b3b5c191051a4ea2b81b11ed3088a09ec6d051fc/crates/ruff_linter/src/checkers/noqa.rs#L38) within `check_noqa()` using a function that [might emit a warning](https://github.com/astral-sh/ruff/blob/b3b5c191051a4ea2b81b11ed3088a09ec6d051fc/crates/ruff_linter/src/noqa.rs#L134-L137) if a `# noqa:` comment does not have any codes. This means the necessary information cannot be retrieved from functions other than `check_noqa()` without undesirable side-effects.

---

_@InSyncWithFoo reviewed on 2024-11-11 13:51_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/unformatted_special_comment.rs`:34 on 2024-11-11 13:51_

I see. This rule will be in charge of Ruff-related comments only, then. I take it that `Nopycln` should also be removed, given that there are no instances of such a comment in the entire codebase.

---

_Comment by @MichaReiser on 2024-11-12 11:00_

@AlexWaygood do you have any recommendations on the rule's scope?

---

_Review comment by @DetachHead on `crates/ruff_linter/resources/test/fixtures/ruff/RUF104.py`:1 on 2024-11-13 12:37_

are `# pyright:` comments supported too?

---

_@DetachHead reviewed on 2024-11-13 12:37_

---

_Comment by @AlexWaygood on 2024-11-13 14:09_

I agree that we're getting into very hairy territory if we start formatting other tools' pragma comments by default. However, it does seem useful to have a rule that does generalised formatting of pragma comments.

I like @MichaReiser's second suggestion in https://github.com/astral-sh/ruff/pull/14111#pullrequestreview-2427244566 -- I would suggest a rule
- That is not in the `RUF1XX` category (which as @MichaReiser says is a category specifically for Ruff pragma comments)
- That by default, only formats Ruff pragma comments. (And maybe also `type: ignore` comments, since they are standardised across tools according to a specification?)
- That can be configured so that it also includes other tool-specific pragma comments such as `pyright: ignore`, `isort: skip_file` and `pragma: nocover`

---

_@InSyncWithFoo reviewed on 2024-11-13 16:54_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/ruff/RUF104.py`:1 on 2024-11-13 16:54_

Originally, but I removed `# pyright:`, `# mypy:` and `# nopycln:` as per [this comment](https://github.com/astral-sh/ruff/pull/14111#pullrequestreview-2427244566). If this rule is accepted, I'll try my hands on another rule that does the same thing but for custom comments.

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/RUF104.py`:1 on 2024-11-13 16:58_

@InSyncWithFoo I suggest we go forward with @AlexWaygood suggestions. That allows users to opt-in to formatting pyright comments or any other comment they like. 

---

_@MichaReiser reviewed on 2024-11-13 16:58_

---

_Comment by @InSyncWithFoo on 2024-11-13 17:04_

@AlexWaygood

1. I'll move it to `RUF037`, then.
2. Currently the rule is handling (aside from `# type`) `# noqa`, `# ruff: noqa`, `# flake8: noqa`, `# isort`, `# ruff: isort` and `# fmt`. Aren't these all comments that Ruff reads?
3. I'm in favor of limiting this rule to just Ruff comments, as I have already rewritten this PR a couple of times and I don't feel like redoing it all over again. I'll give custom comments their own rule in another PR.

---

_Comment by @AlexWaygood on 2024-11-13 17:15_

> Currently the rule is handling (aside from `# type`) `# noqa`, `# ruff: noqa`, `# flake8: noqa`, `# isort`, `# ruff: isort` and `# fmt`. Aren't these all comments that Ruff reads?

They are! I wasn't necessarily criticising anything you were doing -- was just giving my response to Micha's request in https://github.com/astral-sh/ruff/pull/14111#issuecomment-2470227442 on what the scope of the rule should be :-)

> I'm in favor of limiting this rule to just Ruff comments, as I have already rewritten this PR a couple of times and I don't feel like redoing it all over again. I'll give custom comments their own rule in another PR.

Feel free to keep this PR small and not implement any configurability now. Small, incremental PRs are definitely encouraged! However, if you want to open a followup PR, please make this rule configurable in the followup PR rather than creating a separate rule just for custom comments. I think Micha and I are aligned that a single, configurable rule would be preferable to two rules that do very similar things.

---

_Renamed from "[`ruff`] Unformatted special comments (`RUF104`)" to "[`ruff`] Unformatted special comments (`RUF037`)" by @InSyncWithFoo on 2024-11-13 17:31_

---

_Comment by @MichaReiser on 2024-11-13 17:43_

I do think that we can drastically simplify the implementation because we no longer need to special case different comments. All we need is to match for `# (fmt|noqa|...)` and then see if it has an optional `:` following it. 

> Currently the rule is handling (aside from # type) # noqa, # ruff: noqa, # flake8: noqa, # isort, # ruff: isort and # fmt. Aren't these all comments that Ruff reads?

Technically, there's also `yapf: disable|enable`

---

_Comment by @InSyncWithFoo on 2024-11-13 18:19_

> I do think that we can drastically simplify the implementation because we no longer need to special case different comments.

Special casing `# noqa` so that all such comments uses `, ` has been my intent all along, and I do want to keep it that way even if it turns out to be a little more complex than necessary. I don't have a strong opinion about the others, though at the moment only valid pragma comments are recognized.

@AlexWaygood If we were to add a "custom" formatter, that would need to be generic enough to apply to most, if not all, pragma comments. The only way to do so (that I can see) is to use regex search-and-replace.

Rust's regex engine guarantees linear time matching, so there is no risk of ReDoS. However, there are also disadvantages: No advanced features. Having no lookarounds, no conditional replacements, no multicaptures and no backreferences means that some comments would require multiple runs to be formatted correctly. For example:

```toml
[tool.ruff.lint.format-special-comments]
# `#pyright:ignore[reportFoo,reportBar,reportQux]` -> `# pyright: ignore[reportFoo,reportBar,reportQux]`
# https://regex101.com/r/zd9esV/1
'(?i)#\s*pyright\s*:\s*' = '# pyright: '

# `# pyright: ignore[reportFoo,reportBar,reportQux]` -> `# pyright: ignore[reportFoo, reportBar,reportQux]`
# https://regex101.com/r/zd9esV/2
'(# pyright: ignore\[[^\s,]+(?:, [^\s,]+)*?),(?:\s{2,})?(\S)' = '$1, $2'

# `# pyright: ignore[reportFoo, reportBar,reportQux]` -> `# pyright: ignore[reportFoo, reportBar, reportQux]`
# https://regex101.com/r/zd9esV/3
'(# pyright: ignore\[[^\s,]+(?:, [^\s,]+)*?),(?:\s{2,})?(\S)' = '$1, $2'

# And so on.
```

With PCRE features, it's much easier (less readable too, though):

```toml
# https://regex101.com/r/URCpwl/1
'(?i)(?:(?:(\#\s*pyright\s*:\s*)|\G(?:ignore\[)?[^\s,]+\K,\s*))' = '${1:+# pyright\: :, }'
```

We could also consider [`fancy-regex`](https://github.com/fancy-regex/fancy-regex).

---

_Label `needs-decision` added by @MichaReiser on 2024-11-13 18:35_

---

_Comment by @MichaReiser on 2024-11-14 07:19_

The problem I see with special casing is that it isn't a good fit for a general-purpose, extensible rule; it should instead be a specific rule for `noqa`. 

The question is if we can come up with some general rules that work for all suppression comments without being specific to any pragma comment and that allows easy configuration:

* Require a space between `#` and `name` in # <name>`
* Require no space left of a `:` but require a space right of it: `# name: ignore` ok, `# name:ignore` not-ok
* Require no a space left of a `,` but require a space right of it: `# type: ignore[a, b]` is ok, `type: ignore[a,b]` isn't
* Require no space around `[` but require a space right of `]` if it isn't the end of the comment.

The rule doesn't require that any suppression comment is valid. E.g. it accepts `#type: ignore, a, b, cd` t

Note: What's interesting is that e.g. pyright's documentation uses a space before `[`: `# pyright: ignore [reportPrivateUsage, reportGeneralTypeIssues]` whereas mypy recommends using no-space `# type: ignore[<code>]` (which I prefer)

---

_Comment by @InSyncWithFoo on 2024-11-14 07:29_

I would prefer it if we could have two rules, one for Ruff-specific comments and one customizable, somewhat similar to `I001` and `I002`. I don't want to rewrite `#noqa:A123,B456` to just `# noqa: A123,B456`; `# noqa: A123, B456` is clearly better. If special-casing in an extensible rule is so bad, then let's use two rules instead.

---

_Comment by @MichaReiser on 2024-11-14 07:32_

> I don't want to rewrite #noqa:A123,B456 to just # noqa: A123,B456; # noqa: A123, B456 is clearly better.

The rule would handle this, when using the rules that I outlined above. What the rule wouldn't handle is that e.g. all rule codes are upper-case (although I'm not sure if ruff even accepts lower case rule codes?)

I'm open to having a noqa specific rule, but I don't think it should handle other ruff suppression comments.

---

_Comment by @InSyncWithFoo on 2024-11-14 07:44_

> although I'm not sure if ruff even accepts lower case rule codes?

`# ruff: noqa` does, `# noqa` doesn't, though these codes are not taken into account during suppression. Previously, both of these uses `[A-Z]+[0-9]+`; Charlie changed both of them recently (#12809), then only reverted the latter (#14229).

---

_Comment by @Avasam on 2024-11-18 00:18_

> A general purpose rule that handles all kinds of pragma comments. It only enforces a single space between # and the keyword and, optionally, that the colon is followed by a space. We can add a configuration option to support custom pragma codes. 

Making this rule extensible is also what I've suggesting in https://github.com/astral-sh/ruff/issues/10160#issuecomment-1971452721 . Even if it comes as a follow-up PR, I'd like if it could handle comments that Ruff doesn't need to be aware of (like pyright)

And if it supports regex somehow, this would superseed TD007 (https://docs.astral.sh/ruff/rules/missing-space-after-todo-colon/) entirely

---

_Comment by @MichaReiser on 2024-12-19 12:14_

I'll close this PR because there's no agreement on the rule's semantics. We should define and align on that first before working on a new PR. 

Thank you @InSyncWithFoo for initiating the discussion and creating this pr.

---

_Closed by @MichaReiser on 2024-12-19 12:14_

---

_Branch deleted on 2024-12-19 16:41_

---
