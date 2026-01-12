```yaml
number: 8903
title: "[pylint] - implement C2701 `import-private-name`"
type: pull_request
state: closed
author: diceroll123
labels: []
assignees: []
base: main
head: add-C2701
created_at: 2023-11-29T06:20:27Z
updated_at: 2023-11-29T17:22:27Z
url: https://github.com/astral-sh/ruff/pull/8903
synced_at: 2026-01-12T15:55:27Z
```

# [pylint] - implement C2701 `import-private-name`

---

_@diceroll123_

## Summary

Implements [`import-private-name/C2701`](https://pylint.readthedocs.io/en/latest/user_guide/messages/convention/import-private-name.html)

See: #970

## Test Plan

`cargo test` and manually

---

_Renamed from "[pylint] - implement C2701" to "[pylint] - implement C2701 `import-private-name`" by @diceroll123 on 2023-11-29 06:21_

---

_Comment by @github-actions[bot] on 2023-11-29 06:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2524 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/594c12ad5230522a12008445217e71726cc60611/disnake/ext/commands/base_core.py#L26'>disnake/ext/commands/base_core.py:26:27:</a> PLC2701 Imported private object
+ <a href='https://github.com/DisnakeDev/disnake/blob/594c12ad5230522a12008445217e71726cc60611/disnake/ext/commands/base_core.py#L26'>disnake/ext/commands/base_core.py:26:39:</a> PLC2701 Imported private object
+ <a href='https://github.com/DisnakeDev/disnake/blob/594c12ad5230522a12008445217e71726cc60611/disnake/ext/commands/core.py#L31'>disnake/ext/commands/core.py:31:5:</a> PLC2701 Imported private object
+ <a href='https://github.com/DisnakeDev/disnake/blob/594c12ad5230522a12008445217e71726cc60611/disnake/ext/commands/core.py#L32'>disnake/ext/commands/core.py:32:5:</a> PLC2701 Imported private object
+ <a href='https://github.com/DisnakeDev/disnake/blob/594c12ad5230522a12008445217e71726cc60611/disnake/ext/commands/flags.py#L8'>disnake/ext/commands/flags.py:8:27:</a> PLC2701 Imported private object
+ <a href='https://github.com/DisnakeDev/disnake/blob/594c12ad5230522a12008445217e71726cc60611/disnake/ext/commands/params.py#L41'>disnake/ext/commands/params.py:41:29:</a> PLC2701 Imported private object
+ <a href='https://github.com/DisnakeDev/disnake/blob/594c12ad5230522a12008445217e71726cc60611/docs/extensions/attributetable.py#L13'>docs/extensions/attributetable.py:13:27:</a> PLC2701 Imported private object
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aiven/aiven-client/blob/1f64f18fc1631bf5f6d8102b846f63ea60439054/tests/test_cliarg.py#L7'>tests/test_cliarg.py:7:6:</a> PLC2701 Imported private module
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+353 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/92cc2ffd863b8925ed785d5e8b02ac38488e835e/airflow/api/auth/backend/kerberos_auth.py#L55'>airflow/api/auth/backend/kerberos_auth.py:55:29:</a> PLC2701 Imported private object
+ <a href='https://github.com/apache/airflow/blob/92cc2ffd863b8925ed785d5e8b02ac38488e835e/airflow/api/common/experimental/mark_tasks.py#L24'>airflow/api/common/experimental/mark_tasks.py:24:5:</a> PLC2701 Imported private object
+ <a href='https://github.com/apache/airflow/blob/92cc2ffd863b8925ed785d5e8b02ac38488e835e/airflow/api_connexion/security.py#L73'>airflow/api_connexion/security.py:73:59:</a> PLC2701 Imported private object
+ <a href='https://github.com/apache/airflow/blob/92cc2ffd863b8925ed785d5e8b02ac38488e835e/airflow/api_internal/internal_api_call.py#L29'>airflow/api_internal/internal_api_call.py:29:30:</a> PLC2701 Imported private object
+ <a href='https://github.com/apache/airflow/blob/92cc2ffd863b8925ed785d5e8b02ac38488e835e/airflow/auth/managers/fab/decorators/auth.py#L31'>airflow/auth/managers/fab/decorators/auth.py:31:30:</a> PLC2701 Imported private object
+ <a href='https://github.com/apache/airflow/blob/92cc2ffd863b8925ed785d5e8b02ac38488e835e/airflow/auth/managers/fab/fab_auth_manager.py#L86'>airflow/auth/managers/fab/fab_auth_manager.py:86:47:</a> PLC2701 Imported private object
+ <a href='https://github.com/apache/airflow/blob/92cc2ffd863b8925ed785d5e8b02ac38488e835e/airflow/auth/managers/fab/fab_auth_manager.py#L86'>airflow/auth/managers/fab/fab_auth_manager.py:86:81:</a> PLC2701 Imported private object
+ <a href='https://github.com/apache/airflow/blob/92cc2ffd863b8925ed785d5e8b02ac38488e835e/airflow/cli/cli_config.py#L34'>airflow/cli/cli_config.py:34:30:</a> PLC2701 Imported private object
+ <a href='https://github.com/apache/airflow/blob/92cc2ffd863b8925ed785d5e8b02ac38488e835e/airflow/cli/commands/db_command.py#L33'>airflow/cli/commands/db_command.py:33:30:</a> PLC2701 Imported private object
+ <a href='https://github.com/apache/airflow/blob/92cc2ffd863b8925ed785d5e8b02ac38488e835e/airflow/cli/commands/task_command.py#L63'>airflow/cli/commands/task_command.py:63:49:</a> PLC2701 Imported private object
... 343 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+286 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/0ae85690d3929f8b28443ff0826de3ad059af1c4/samcli/cli/main.py#L10'>samcli/cli/main.py:10:20:</a> PLC2701 Imported private object
+ <a href='https://github.com/aws/aws-sam-cli/blob/0ae85690d3929f8b28443ff0826de3ad059af1c4/samcli/cli/main.py#L15'>samcli/cli/main.py:15:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/aws/aws-sam-cli/blob/0ae85690d3929f8b28443ff0826de3ad059af1c4/samcli/commands/_utils/click_mutex.py#L8'>samcli/commands/_utils/click_mutex.py:8:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/aws/aws-sam-cli/blob/0ae85690d3929f8b28443ff0826de3ad059af1c4/samcli/commands/_utils/command_exception_handler.py#L10'>samcli/commands/_utils/command_exception_handler.py:10:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/aws/aws-sam-cli/blob/0ae85690d3929f8b28443ff0826de3ad059af1c4/samcli/commands/_utils/custom_options/hook_name_option.py#L12'>samcli/commands/_utils/custom_options/hook_name_option.py:12:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/aws/aws-sam-cli/blob/0ae85690d3929f8b28443ff0826de3ad059af1c4/samcli/commands/_utils/experimental.py#L12'>samcli/commands/_utils/experimental.py:12:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/aws/aws-sam-cli/blob/0ae85690d3929f8b28443ff0826de3ad059af1c4/samcli/commands/_utils/options.py#L22'>samcli/commands/_utils/options.py:22:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/aws/aws-sam-cli/blob/0ae85690d3929f8b28443ff0826de3ad059af1c4/samcli/commands/_utils/options.py#L28'>samcli/commands/_utils/options.py:28:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/aws/aws-sam-cli/blob/0ae85690d3929f8b28443ff0826de3ad059af1c4/samcli/commands/_utils/options.py#L29'>samcli/commands/_utils/options.py:29:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/aws/aws-sam-cli/blob/0ae85690d3929f8b28443ff0826de3ad059af1c4/samcli/commands/_utils/options.py#L30'>samcli/commands/_utils/options.py:30:6:</a> PLC2701 Imported from private module
... 276 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+103 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/docs/bokeh/source/conf.py#L15'>docs/bokeh/source/conf.py:15:19:</a> PLC2701 Imported private object
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/command/bootstrap.py#L51'>src/bokeh/command/bootstrap.py:51:19:</a> PLC2701 Imported private object
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/command/subcommands/info.py#L63'>src/bokeh/command/subcommands/info.py:63:19:</a> PLC2701 Imported private object
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/protocol/messages/server_info_reply.py#L24'>src/bokeh/protocol/messages/server_info_reply.py:24:19:</a> PLC2701 Imported private object
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/sphinxext/bokeh_options.py#L65'>src/bokeh/sphinxext/bokeh_options.py:65:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/sphinxext/bokeh_prop.py#L69'>src/bokeh/sphinxext/bokeh_prop.py:69:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/sphinxext/bokeh_releases.py#L51'>src/bokeh/sphinxext/bokeh_releases.py:51:19:</a> PLC2701 Imported private object
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/sphinxext/bokeh_sampledata_xref.py#L39'>src/bokeh/sphinxext/bokeh_sampledata_xref.py:39:27:</a> PLC2701 Imported private object
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/sphinxext/bokeh_settings.py#L46'>src/bokeh/sphinxext/bokeh_settings.py:46:48:</a> PLC2701 Imported private object
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/sphinxext/bokehjs_content.py#L68'>src/bokeh/sphinxext/bokehjs_content.py:68:27:</a> PLC2701 Imported private object
... 93 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/4d9b74b7e494b6a8e2d06677bc7c20d558b5f078/securedrop/manage.py#L13'>securedrop/manage.py:13:22:</a> PLC2701 Imported private object
+ <a href='https://github.com/freedomofpress/securedrop/blob/4d9b74b7e494b6a8e2d06677bc7c20d558b5f078/securedrop/management/submissions.py#L6'>securedrop/management/submissions.py:6:22:</a> PLC2701 Imported private object
+ <a href='https://github.com/freedomofpress/securedrop/blob/4d9b74b7e494b6a8e2d06677bc7c20d558b5f078/securedrop/secure_tempfile.py#L3'>securedrop/secure_tempfile.py:3:22:</a> PLC2701 Imported private object
+ <a href='https://github.com/freedomofpress/securedrop/blob/4d9b74b7e494b6a8e2d06677bc7c20d558b5f078/securedrop/store.py#L10'>securedrop/store.py:10:22:</a> PLC2701 Imported private object
+ <a href='https://github.com/freedomofpress/securedrop/blob/4d9b74b7e494b6a8e2d06677bc7c20d558b5f078/securedrop/tests/conftest.py#L24'>securedrop/tests/conftest.py:24:25:</a> PLC2701 Imported private object
+ <a href='https://github.com/freedomofpress/securedrop/blob/4d9b74b7e494b6a8e2d06677bc7c20d558b5f078/securedrop/tests/factories.py#L12'>securedrop/tests/factories.py:12:41:</a> PLC2701 Imported private object
+ <a href='https://github.com/freedomofpress/securedrop/blob/4d9b74b7e494b6a8e2d06677bc7c20d558b5f078/securedrop/tests/functional/app_navigators/journalist_app_nav.py#L16'>securedrop/tests/functional/app_navigators/journalist_app_nav.py:16:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/freedomofpress/securedrop/blob/4d9b74b7e494b6a8e2d06677bc7c20d558b5f078/securedrop/tests/functional/app_navigators/source_app_nav.py#L8'>securedrop/tests/functional/app_navigators/source_app_nav.py:8:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/freedomofpress/securedrop/blob/4d9b74b7e494b6a8e2d06677bc7c20d558b5f078/securedrop/tests/functional/pageslayout/test_source_static_pages.py#L6'>securedrop/tests/functional/pageslayout/test_source_static_pages.py:6:21:</a> PLC2701 Imported private object
+ <a href='https://github.com/freedomofpress/securedrop/blob/4d9b74b7e494b6a8e2d06677bc7c20d558b5f078/securedrop/tests/functional/test_source_warnings.py#L7'>securedrop/tests/functional/test_source_warnings.py:7:42:</a> PLC2701 Imported private object
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/32f3235c9f58294bb0ea8c6385220eff054eecb4/blinkpy/blinkpy.py#L35'>blinkpy/blinkpy.py:35:39:</a> PLC2701 Imported private object
+ <a href='https://github.com/fronzbot/blinkpy/blob/32f3235c9f58294bb0ea8c6385220eff054eecb4/tests/test_blinkpy.py#L14'>tests/test_blinkpy.py:14:39:</a> PLC2701 Imported private object
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+87 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/47b7cfcbda63c183fe369a67dcb961fc913002c8/docs/backends/app/backend_info_app.py#L14'>docs/backends/app/backend_info_app.py:14:18:</a> PLC2701 Imported private object
+ <a href='https://github.com/ibis-project/ibis/blob/47b7cfcbda63c183fe369a67dcb961fc913002c8/docs/how-to/visualization/example_streamlit_app/example_streamlit_app.py#L6'>docs/how-to/visualization/example_streamlit_app/example_streamlit_app.py:6:18:</a> PLC2701 Imported private object
+ <a href='https://github.com/ibis-project/ibis/blob/47b7cfcbda63c183fe369a67dcb961fc913002c8/ibis/backends/base/sql/alchemy/query_builder.py#L13'>ibis/backends/base/sql/alchemy/query_builder.py:13:32:</a> PLC2701 Imported private object
+ <a href='https://github.com/ibis-project/ibis/blob/47b7cfcbda63c183fe369a67dcb961fc913002c8/ibis/backends/base/sql/compiler/query_builder.py#L14'>ibis/backends/base/sql/compiler/query_builder.py:14:75:</a> PLC2701 Imported private object
+ <a href='https://github.com/ibis-project/ibis/blob/47b7cfcbda63c183fe369a67dcb961fc913002c8/ibis/backends/bigquery/registry.py#L24'>ibis/backends/bigquery/registry.py:24:53:</a> PLC2701 Imported private object
+ <a href='https://github.com/ibis-project/ibis/blob/47b7cfcbda63c183fe369a67dcb961fc913002c8/ibis/backends/bigquery/tests/unit/test_compiler.py#L15'>ibis/backends/bigquery/tests/unit/test_compiler.py:15:18:</a> PLC2701 Imported private object
+ <a href='https://github.com/ibis-project/ibis/blob/47b7cfcbda63c183fe369a67dcb961fc913002c8/ibis/backends/bigquery/tests/unit/udf/test_usage.py#L9'>ibis/backends/bigquery/tests/unit/udf/test_usage.py:9:40:</a> PLC2701 Imported private object
+ <a href='https://github.com/ibis-project/ibis/blob/47b7cfcbda63c183fe369a67dcb961fc913002c8/ibis/backends/clickhouse/compiler/core.py#L30'>ibis/backends/clickhouse/compiler/core.py:30:34:</a> PLC2701 Imported private object
+ <a href='https://github.com/ibis-project/ibis/blob/47b7cfcbda63c183fe369a67dcb961fc913002c8/ibis/backends/dask/__init__.py#L20'>ibis/backends/dask/__init__.py:20:39:</a> PLC2701 Imported private object
+ <a href='https://github.com/ibis-project/ibis/blob/47b7cfcbda63c183fe369a67dcb961fc913002c8/ibis/backends/dask/execution/generic.py#L45'>ibis/backends/dask/execution/generic.py:45:5:</a> PLC2701 Imported private object
... 77 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/d58447a8433752d3a97d8a6cb497dada63959f1e/pymilvus/client/grpc_handler.py#L11'>pymilvus/client/grpc_handler.py:11:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/milvus-io/pymilvus/blob/d58447a8433752d3a97d8a6cb497dada63959f1e/pymilvus/client/prepare.py#L7'>pymilvus/client/prepare.py:7:29:</a> PLC2701 Imported private object
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1417 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/gil.py#L32'>asv_bench/benchmarks/gil.py:32:10:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/indexing_engines.py#L13'>asv_bench/benchmarks/indexing_engines.py:13:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/io/parsers.py#L4'>asv_bench/benchmarks/io/parsers.py:4:10:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/io/parsers.py#L5'>asv_bench/benchmarks/io/parsers.py:5:9:</a> PLC2701 Imported private object
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/libs.py#L10'>asv_bench/benchmarks/libs.py:10:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/plotting.py#L25'>asv_bench/benchmarks/plotting.py:25:35:</a> PLC2701 Imported private object
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/plotting.py#L25'>asv_bench/benchmarks/plotting.py:25:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/timeseries.py#L17'>asv_bench/benchmarks/timeseries.py:17:10:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/tslibs/fields.py#L3'>asv_bench/benchmarks/tslibs/fields.py:3:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/tslibs/normalize.py#L2'>asv_bench/benchmarks/tslibs/normalize.py:2:10:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/tslibs/normalize.py#L7'>asv_bench/benchmarks/tslibs/normalize.py:7:10:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/tslibs/period.py#L22'>asv_bench/benchmarks/tslibs/period.py:22:10:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/tslibs/period.py#L24'>asv_bench/benchmarks/tslibs/period.py:24:10:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/tslibs/period.py#L8'>asv_bench/benchmarks/tslibs/period.py:8:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/tslibs/resolution.py#L23'>asv_bench/benchmarks/tslibs/resolution.py:23:10:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/tslibs/resolution.py#L25'>asv_bench/benchmarks/tslibs/resolution.py:25:10:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/tslibs/tslib.py#L31'>asv_bench/benchmarks/tslibs/tslib.py:31:10:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/tslibs/tslib.py#L33'>asv_bench/benchmarks/tslibs/tslib.py:33:10:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/tslibs/tz_convert.py#L14'>asv_bench/benchmarks/tslibs/tz_convert.py:14:10:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/tslibs/tz_convert.py#L18'>asv_bench/benchmarks/tslibs/tz_convert.py:18:14:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/tslibs/tz_convert.py#L21'>asv_bench/benchmarks/tslibs/tz_convert.py:21:14:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/asv_bench/benchmarks/tslibs/tz_convert.py#L4'>asv_bench/benchmarks/tslibs/tz_convert.py:4:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/doc/source/conf.py#L23'>doc/source/conf.py:23:36:</a> PLC2701 Imported private object
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/generate_version.py#L18'>generate_version.py:18:16:</a> PLC2701 Imported private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/generate_version.py#L61'>generate_version.py:61:20:</a> PLC2701 Imported private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/pandas/__init__.py#L140'>pandas/__init__.py:140:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/pandas/__init__.py#L175'>pandas/__init__.py:175:6:</a> PLC2701 Imported from private module
+ <a href='https://github.com/pandas-dev/pandas/blob/e8f05983b5743de3690cbd3d4f8b9896f9429c4c/pandas/__init__.py#L177'>pandas/__init__.py:177:6:</a> PLC2701 Imported from private module
... 1389 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed projected reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC2701 | 2522 | 2522 | 0 | 0 | 0 |
| F401 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Converted to draft by @diceroll123 on 2023-11-29 06:35_

---

_Marked ready for review by @diceroll123 on 2023-11-29 07:44_

---

_Comment by @tjkuson on 2023-11-29 13:40_

There's an old PR (#5920) that implements this rule that hasn't been merged.

---

_Comment by @diceroll123 on 2023-11-29 17:01_

Woop, I should have checked! I concede. 🤣 

Thanks for the heads-up, @tjkuson!

---

_Closed by @diceroll123 on 2023-11-29 17:01_

---

_Comment by @tjkuson on 2023-11-29 17:18_

Well, your implementation might be better! Mine is quite old…

---

_Comment by @diceroll123 on 2023-11-29 17:22_

Ah, I looked over yours, it's got things I didn't implement, I prefer yours!

---
