```yaml
number: 20270
title: "[`flake8-simplify`] Stabilize fix safety of `multiple-with-statements` (`SIM117`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - fixes
assignees: []
merged: true
base: brent/0.13.0
head: dylan/stabilize-sim-fix
created_at: 2025-09-05T18:52:43Z
updated_at: 2025-09-08T17:34:01Z
url: https://github.com/astral-sh/ruff/pull/20270
synced_at: 2026-01-10T17:46:21Z
```

# [`flake8-simplify`] Stabilize fix safety of `multiple-with-statements` (`SIM117`)

---

_Pull request opened by @dylwil3 on 2025-09-05 18:52_

Introduced in #18208. Removed gating, updated tests and docs.


---

_@amyreese approved on 2025-09-05 19:01_

---

_Comment by @github-actions[bot] on 2025-09-05 19:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +436 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +318 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/src/airflow/cli/commands/rotate_fernet_key_command.py#L36'>airflow-core/src/airflow/cli/commands/rotate_fernet_key_command.py:36:5:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/src/airflow/cli/commands/rotate_fernet_key_command.py#L36'>airflow-core/src/airflow/cli/commands/rotate_fernet_key_command.py:36:5:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/always/test_secrets_local_filesystem.py#L401'>airflow-core/tests/unit/always/test_secrets_local_filesystem.py:401:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/always/test_secrets_local_filesystem.py#L401'>airflow-core/tests/unit/always/test_secrets_local_filesystem.py:401:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/always/test_secrets_local_filesystem.py#L413'>airflow-core/tests/unit/always/test_secrets_local_filesystem.py:413:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/always/test_secrets_local_filesystem.py#L413'>airflow-core/tests/unit/always/test_secrets_local_filesystem.py:413:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/always/test_secrets_local_filesystem.py#L427'>airflow-core/tests/unit/always/test_secrets_local_filesystem.py:427:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/always/test_secrets_local_filesystem.py#L427'>airflow-core/tests/unit/always/test_secrets_local_filesystem.py:427:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/api/common/test_mark_tasks.py#L39'>airflow-core/tests/unit/api/common/test_mark_tasks.py:39:5:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/api/common/test_mark_tasks.py#L39'>airflow-core/tests/unit/api/common/test_mark_tasks.py:39:5:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/api/common/test_mark_tasks.py#L66'>airflow-core/tests/unit/api/common/test_mark_tasks.py:66:5:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/api/common/test_mark_tasks.py#L66'>airflow-core/tests/unit/api/common/test_mark_tasks.py:66:5:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/api/common/test_mark_tasks.py#L95'>airflow-core/tests/unit/api/common/test_mark_tasks.py:95:5:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/api/common/test_mark_tasks.py#L95'>airflow-core/tests/unit/api/common/test_mark_tasks.py:95:5:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/api_fastapi/core_api/routes/ui/test_structure.py#L697'>airflow-core/tests/unit/api_fastapi/core_api/routes/ui/test_structure.py:697:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/api_fastapi/core_api/routes/ui/test_structure.py#L697'>airflow-core/tests/unit/api_fastapi/core_api/routes/ui/test_structure.py:697:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1902'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1902:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1902'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1902:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/cli/commands/test_dag_command.py#L325'>airflow-core/tests/unit/cli/commands/test_dag_command.py:325:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/cli/commands/test_dag_command.py#L325'>airflow-core/tests/unit/cli/commands/test_dag_command.py:325:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/cli/commands/test_dag_command.py#L345'>airflow-core/tests/unit/cli/commands/test_dag_command.py:345:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/cli/commands/test_dag_command.py#L345'>airflow-core/tests/unit/cli/commands/test_dag_command.py:345:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/cli/commands/test_dag_command.py#L384'>airflow-core/tests/unit/cli/commands/test_dag_command.py:384:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/cli/commands/test_dag_command.py#L384'>airflow-core/tests/unit/cli/commands/test_dag_command.py:384:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/cli/commands/test_variable_command.py#L261'>airflow-core/tests/unit/cli/commands/test_variable_command.py:261:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/cli/commands/test_variable_command.py#L261'>airflow-core/tests/unit/cli/commands/test_variable_command.py:261:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/cli/commands/test_variable_command.py#L437'>airflow-core/tests/unit/cli/commands/test_variable_command.py:437:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/cli/commands/test_variable_command.py#L437'>airflow-core/tests/unit/cli/commands/test_variable_command.py:437:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/cli/commands/test_variable_command.py#L495'>airflow-core/tests/unit/cli/commands/test_variable_command.py:495:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/cli/commands/test_variable_command.py#L495'>airflow-core/tests/unit/cli/commands/test_variable_command.py:495:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/core/test_configuration.py#L1211'>airflow-core/tests/unit/core/test_configuration.py:1211:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/core/test_configuration.py#L1211'>airflow-core/tests/unit/core/test_configuration.py:1211:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/core/test_configuration.py#L1584'>airflow-core/tests/unit/core/test_configuration.py:1584:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/core/test_configuration.py#L1584'>airflow-core/tests/unit/core/test_configuration.py:1584:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/core/test_configuration.py#L1782'>airflow-core/tests/unit/core/test_configuration.py:1782:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/core/test_configuration.py#L1782'>airflow-core/tests/unit/core/test_configuration.py:1782:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
... 282 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +62 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/9efb80dbf4a07a5272b22ae4ea5684d3ba535b9f/superset/db_engine_specs/gsheets.py#L376'>superset/db_engine_specs/gsheets.py:376:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/superset/blob/9efb80dbf4a07a5272b22ae4ea5684d3ba535b9f/superset/db_engine_specs/gsheets.py#L376'>superset/db_engine_specs/gsheets.py:376:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/superset/blob/9efb80dbf4a07a5272b22ae4ea5684d3ba535b9f/superset/models/core.py#L579'>superset/models/core.py:579:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/superset/blob/9efb80dbf4a07a5272b22ae4ea5684d3ba535b9f/superset/models/core.py#L579'>superset/models/core.py:579:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/superset/blob/9efb80dbf4a07a5272b22ae4ea5684d3ba535b9f/superset/sql_lab.py#L658'>superset/sql_lab.py:658:5:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/superset/blob/9efb80dbf4a07a5272b22ae4ea5684d3ba535b9f/superset/sql_lab.py#L658'>superset/sql_lab.py:658:5:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/superset/blob/9efb80dbf4a07a5272b22ae4ea5684d3ba535b9f/tests/conftest.py#L73'>tests/conftest.py:73:5:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/superset/blob/9efb80dbf4a07a5272b22ae4ea5684d3ba535b9f/tests/conftest.py#L73'>tests/conftest.py:73:5:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/superset/blob/9efb80dbf4a07a5272b22ae4ea5684d3ba535b9f/tests/integration_tests/dashboards/commands_tests.py#L781'>tests/integration_tests/dashboards/commands_tests.py:781:13:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/superset/blob/9efb80dbf4a07a5272b22ae4ea5684d3ba535b9f/tests/integration_tests/dashboards/commands_tests.py#L781'>tests/integration_tests/dashboards/commands_tests.py:781:13:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
... 52 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+0 -0 violations, +10 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/review_fns/data_sources/openalex_source.py#L115'>crazy_functions/review_fns/data_sources/openalex_source.py:115:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/review_fns/data_sources/openalex_source.py#L115'>crazy_functions/review_fns/data_sources/openalex_source.py:115:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/review_fns/data_sources/openalex_source.py#L22'>crazy_functions/review_fns/data_sources/openalex_source.py:22:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/review_fns/data_sources/openalex_source.py#L22'>crazy_functions/review_fns/data_sources/openalex_source.py:22:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/review_fns/data_sources/openalex_source.py#L88'>crazy_functions/review_fns/data_sources/openalex_source.py:88:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/review_fns/data_sources/openalex_source.py#L88'>crazy_functions/review_fns/data_sources/openalex_source.py:88:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/review_fns/data_sources/openalex_source.py#L99'>crazy_functions/review_fns/data_sources/openalex_source.py:99:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/review_fns/data_sources/openalex_source.py#L99'>crazy_functions/review_fns/data_sources/openalex_source.py:99:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/review_fns/data_sources/unpaywall_source.py#L14'>crazy_functions/review_fns/data_sources/unpaywall_source.py:14:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/review_fns/data_sources/unpaywall_source.py#L14'>crazy_functions/review_fns/data_sources/unpaywall_source.py:14:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +44 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L157'>src/bokeh/application/handlers/code.py:157:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L157'>src/bokeh/application/handlers/code.py:157:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/command/subcommands/test_json__subcommands.py#L92'>tests/unit/bokeh/command/subcommands/test_json__subcommands.py:92:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/command/subcommands/test_json__subcommands.py#L92'>tests/unit/bokeh/command/subcommands/test_json__subcommands.py:92:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/command/subcommands/test_serve.py#L584'>tests/unit/bokeh/command/subcommands/test_serve.py:584:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/command/subcommands/test_serve.py#L584'>tests/unit/bokeh/command/subcommands/test_serve.py:584:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/command/subcommands/test_serve.py#L612'>tests/unit/bokeh/command/subcommands/test_serve.py:612:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/command/subcommands/test_serve.py#L612'>tests/unit/bokeh/command/subcommands/test_serve.py:612:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/command/subcommands/test_serve.py#L629'>tests/unit/bokeh/command/subcommands/test_serve.py:629:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/command/subcommands/test_serve.py#L629'>tests/unit/bokeh/command/subcommands/test_serve.py:629:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
... 34 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM117 | 436 | 0 | 0 | 436 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dylwil3 on 2025-09-05 19:14_

---

_Closed by @dylwil3 on 2025-09-05 19:14_

---

_Branch deleted on 2025-09-05 19:14_

---

_Label `fixes` added by @ntBre on 2025-09-08 17:34_

---
