```yaml
number: 14824
title: "[`ruff`] Mark autofix for `RUF052` as always unsafe"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: alex/unsafe-ruf052
created_at: 2024-12-06T18:21:26Z
updated_at: 2024-12-10T23:51:54Z
url: https://github.com/astral-sh/ruff/pull/14824
synced_at: 2026-01-10T20:42:27Z
```

# [`ruff`] Mark autofix for `RUF052` as always unsafe

---

_Pull request opened by @AlexWaygood on 2024-12-06 18:21_

## Summary

#14819 improved the autofix offered by this rule for a bunch of standard-library classes that must be instantiated according to the pattern `T = TypeVar("T")`, where the string literal passed to the constructor _must_ match the name of the variable the call is being assigned to. However, there may be examples of this pattern in third-party libraries that we don't know about, so we can't assume that this fix is now safe.

TODO: needs a comment in `crates/ruff_linter/src/rules/ruff/rules/used_dummy_variable.rs` about why it's marked as unsafe

## Test Plan

`cargo test -p ruff_linter`


---

_Label `fixes` added by @AlexWaygood on 2024-12-06 18:21_

---

_Label `preview` added by @AlexWaygood on 2024-12-06 18:21_

---

_@dylwil3 approved on 2024-12-06 18:37_

Seems like the safest thing to do!

---

_Comment by @github-actions[bot] on 2024-12-06 19:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+66 -0 violations, +0 -1002 fixes in 20 projects; 35 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -0 violations, +0 -32 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/errors.py#L80'>disnake/errors.py:80:17:</a> RUF052 Local dummy variable `_errors` is accessed
- <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/errors.py#L80'>disnake/errors.py:80:17:</a> RUF052 [*] Local dummy variable `_errors` is accessed
+ <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/ext/commands/converter.py#L1294'>disnake/ext/commands/converter.py:1294:9:</a> RUF052 Local dummy variable `_NoneType` is accessed
- <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/ext/commands/converter.py#L1294'>disnake/ext/commands/converter.py:1294:9:</a> RUF052 [*] Local dummy variable `_NoneType` is accessed
+ <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/ext/commands/view.py#L118'>disnake/ext/commands/view.py:118:13:</a> RUF052 Local dummy variable `_escaped_quotes` is accessed
- <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/ext/commands/view.py#L118'>disnake/ext/commands/view.py:118:13:</a> RUF052 [*] Local dummy variable `_escaped_quotes` is accessed
+ <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/guild.py#L5159'>disnake/guild.py:5159:9:</a> RUF052 Local dummy variable `_id` is accessed
- <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/guild.py#L5159'>disnake/guild.py:5159:9:</a> RUF052 [*] Local dummy variable `_id` is accessed
+ <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/guild.py#L5309'>disnake/guild.py:5309:9:</a> RUF052 Local dummy variable `_id` is accessed
- <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/guild.py#L5309'>disnake/guild.py:5309:9:</a> RUF052 [*] Local dummy variable `_id` is accessed
... 22 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -0 violations, +0 -112 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/api.py#L42'>rasa/api.py:42:5:</a> RUF052 Local dummy variable `_endpoints` is accessed
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/api.py#L42'>rasa/api.py:42:5:</a> RUF052 [*] Local dummy variable `_endpoints` is accessed
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/actions/action.py#L1218'>rasa/core/actions/action.py:1218:9:</a> RUF052 Local dummy variable `_tracker` is accessed
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/actions/action.py#L1218'>rasa/core/actions/action.py:1218:9:</a> RUF052 [*] Local dummy variable `_tracker` is accessed
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/actions/action.py#L603'>rasa/core/actions/action.py:603:9:</a> RUF052 Local dummy variable `_events` is accessed
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/actions/action.py#L603'>rasa/core/actions/action.py:603:9:</a> RUF052 [*] Local dummy variable `_events` is accessed
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/actions/forms.py#L274'>rasa/core/actions/forms.py:274:9:</a> RUF052 Local dummy variable `_tracker` is accessed
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/actions/forms.py#L274'>rasa/core/actions/forms.py:274:9:</a> RUF052 [*] Local dummy variable `_tracker` is accessed
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/actions/forms.py#L276'>rasa/core/actions/forms.py:276:9:</a> RUF052 Local dummy variable `_action` is accessed
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/actions/forms.py#L276'>rasa/core/actions/forms.py:276:9:</a> RUF052 [*] Local dummy variable `_action` is accessed
... 102 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -328 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/bb259eaa670240ead9bb9964e9f0b0e19f0f5cde/airflow/api_connexion/endpoints/pool_endpoint.py#L135'>airflow/api_connexion/endpoints/pool_endpoint.py:135:9:</a> RUF052 Local dummy variable `_patch_body` is accessed
- <a href='https://github.com/apache/airflow/blob/bb259eaa670240ead9bb9964e9f0b0e19f0f5cde/airflow/api_connexion/endpoints/pool_endpoint.py#L135'>airflow/api_connexion/endpoints/pool_endpoint.py:135:9:</a> RUF052 [*] Local dummy variable `_patch_body` is accessed
+ <a href='https://github.com/apache/airflow/blob/bb259eaa670240ead9bb9964e9f0b0e19f0f5cde/airflow/assets/manager.py#L280'>airflow/assets/manager.py:280:5:</a> RUF052 Local dummy variable `_asset_manager_class` is accessed
- <a href='https://github.com/apache/airflow/blob/bb259eaa670240ead9bb9964e9f0b0e19f0f5cde/airflow/assets/manager.py#L280'>airflow/assets/manager.py:280:5:</a> RUF052 [*] Local dummy variable `_asset_manager_class` is accessed
+ <a href='https://github.com/apache/airflow/blob/bb259eaa670240ead9bb9964e9f0b0e19f0f5cde/airflow/assets/manager.py#L285'>airflow/assets/manager.py:285:5:</a> RUF052 Local dummy variable `_asset_manager_kwargs` is accessed
- <a href='https://github.com/apache/airflow/blob/bb259eaa670240ead9bb9964e9f0b0e19f0f5cde/airflow/assets/manager.py#L285'>airflow/assets/manager.py:285:5:</a> RUF052 [*] Local dummy variable `_asset_manager_kwargs` is accessed
+ <a href='https://github.com/apache/airflow/blob/bb259eaa670240ead9bb9964e9f0b0e19f0f5cde/airflow/cli/commands/local_commands/info_command.py#L263'>airflow/cli/commands/local_commands/info_command.py:263:9:</a> RUF052 Local dummy variable `_locale` is accessed
- <a href='https://github.com/apache/airflow/blob/bb259eaa670240ead9bb9964e9f0b0e19f0f5cde/airflow/cli/commands/local_commands/info_command.py#L263'>airflow/cli/commands/local_commands/info_command.py:263:9:</a> RUF052 [*] Local dummy variable `_locale` is accessed
+ <a href='https://github.com/apache/airflow/blob/bb259eaa670240ead9bb9964e9f0b0e19f0f5cde/airflow/cli/commands/remote_commands/connection_command.py#L152'>airflow/cli/commands/remote_commands/connection_command.py:152:5:</a> RUF052 Local dummy variable `_connection_types` is accessed
- <a href='https://github.com/apache/airflow/blob/bb259eaa670240ead9bb9964e9f0b0e19f0f5cde/airflow/cli/commands/remote_commands/connection_command.py#L152'>airflow/cli/commands/remote_commands/connection_command.py:152:5:</a> RUF052 [*] Local dummy variable `_connection_types` is accessed
+ <a href='https://github.com/apache/airflow/blob/bb259eaa670240ead9bb9964e9f0b0e19f0f5cde/airflow/cli/commands/remote_commands/task_command.py#L456'>airflow/cli/commands/remote_commands/task_command.py:456:9:</a> RUF052 Local dummy variable `_dag` is accessed
- <a href='https://github.com/apache/airflow/blob/bb259eaa670240ead9bb9964e9f0b0e19f0f5cde/airflow/cli/commands/remote_commands/task_command.py#L456'>airflow/cli/commands/remote_commands/task_command.py:456:9:</a> RUF052 [*] Local dummy variable `_dag` is accessed
+ <a href='https://github.com/apache/airflow/blob/bb259eaa670240ead9bb9964e9f0b0e19f0f5cde/airflow/configuration.py#L1300'>airflow/configuration.py:1300:13:</a> RUF052 Local dummy variable `_section` is accessed
- <a href='https://github.com/apache/airflow/blob/bb259eaa670240ead9bb9964e9f0b0e19f0f5cde/airflow/configuration.py#L1300'>airflow/configuration.py:1300:13:</a> RUF052 [*] Local dummy variable `_section` is accessed
+ <a href='https://github.com/apache/airflow/blob/bb259eaa670240ead9bb9964e9f0b0e19f0f5cde/airflow/dag_processing/processor.py#L237'>airflow/dag_processing/processor.py:237:26:</a> RUF052 Local dummy variable `_child_channel` is accessed
... 313 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +0 -78 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/654701af4cc66e25a3703d2658351af49361f26b/superset/connectors/sqla/models.py#L461'>superset/connectors/sqla/models.py:461:17:</a> RUF052 Local dummy variable `_columns` is accessed
- <a href='https://github.com/apache/superset/blob/654701af4cc66e25a3703d2658351af49361f26b/superset/connectors/sqla/models.py#L461'>superset/connectors/sqla/models.py:461:17:</a> RUF052 [*] Local dummy variable `_columns` is accessed
+ <a href='https://github.com/apache/superset/blob/654701af4cc66e25a3703d2658351af49361f26b/superset/db_engine_specs/presto.py#L149'>superset/db_engine_specs/presto.py:149:13:</a> RUF052 Local dummy variable `_column` is accessed
- <a href='https://github.com/apache/superset/blob/654701af4cc66e25a3703d2658351af49361f26b/superset/db_engine_specs/presto.py#L149'>superset/db_engine_specs/presto.py:149:13:</a> RUF052 [*] Local dummy variable `_column` is accessed
+ <a href='https://github.com/apache/superset/blob/654701af4cc66e25a3703d2658351af49361f26b/superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py#L71'>superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py:71:29:</a> RUF052 Local dummy variable `__from` is accessed
- <a href='https://github.com/apache/superset/blob/654701af4cc66e25a3703d2658351af49361f26b/superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py#L71'>superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py:71:29:</a> RUF052 [*] Local dummy variable `__from` is accessed
+ <a href='https://github.com/apache/superset/blob/654701af4cc66e25a3703d2658351af49361f26b/superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py#L72'>superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py:72:29:</a> RUF052 Local dummy variable `__to` is accessed
- <a href='https://github.com/apache/superset/blob/654701af4cc66e25a3703d2658351af49361f26b/superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py#L72'>superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py:72:29:</a> RUF052 [*] Local dummy variable `__to` is accessed
+ <a href='https://github.com/apache/superset/blob/654701af4cc66e25a3703d2658351af49361f26b/superset/models/helpers.py#L1648'>superset/models/helpers.py:1648:21:</a> RUF052 Local dummy variable `_sql` is accessed
- <a href='https://github.com/apache/superset/blob/654701af4cc66e25a3703d2658351af49361f26b/superset/models/helpers.py#L1648'>superset/models/helpers.py:1648:21:</a> RUF052 [*] Local dummy variable `_sql` is accessed
... 68 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +0 -38 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L219'>src/bokeh/application/handlers/code_runner.py:219:9:</a> RUF052 Local dummy variable `_cwd` is accessed
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L219'>src/bokeh/application/handlers/code_runner.py:219:9:</a> RUF052 [*] Local dummy variable `_cwd` is accessed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L220'>src/bokeh/application/handlers/code_runner.py:220:9:</a> RUF052 Local dummy variable `_sys_path` is accessed
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L220'>src/bokeh/application/handlers/code_runner.py:220:9:</a> RUF052 [*] Local dummy variable `_sys_path` is accessed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L221'>src/bokeh/application/handlers/code_runner.py:221:9:</a> RUF052 Local dummy variable `_sys_argv` is accessed
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L221'>src/bokeh/application/handlers/code_runner.py:221:9:</a> RUF052 [*] Local dummy variable `_sys_argv` is accessed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/static.py#L79'>src/bokeh/command/subcommands/static.py:79:9:</a> RUF052 Local dummy variable `_allowed_keys` is accessed
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/static.py#L79'>src/bokeh/command/subcommands/static.py:79:9:</a> RUF052 [*] Local dummy variable `_allowed_keys` is accessed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/bases.py#L450'>src/bokeh/core/property/bases.py:450:9:</a> RUF052 Local dummy variable `_type_params` is accessed
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/bases.py#L450'>src/bokeh/core/property/bases.py:450:9:</a> RUF052 [*] Local dummy variable `_type_params` is accessed
... 28 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+0 -0 violations, +0 -20 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/35bd10ef46d77d52eb8aaa1edeeafe2ebe763581/.github/workflows/algolia/upload-algolia-api.py#L174'>.github/workflows/algolia/upload-algolia-api.py:174:9:</a> RUF052 Local dummy variable `_creator` is accessed
- <a href='https://github.com/ibis-project/ibis/blob/35bd10ef46d77d52eb8aaa1edeeafe2ebe763581/.github/workflows/algolia/upload-algolia-api.py#L174'>.github/workflows/algolia/upload-algolia-api.py:174:9:</a> RUF052 [*] Local dummy variable `_creator` is accessed
+ <a href='https://github.com/ibis-project/ibis/blob/35bd10ef46d77d52eb8aaa1edeeafe2ebe763581/ibis/backends/impala/ddl.py#L16'>ibis/backends/impala/ddl.py:16:9:</a> RUF052 Local dummy variable `_format_aliases` is accessed
- <a href='https://github.com/ibis-project/ibis/blob/35bd10ef46d77d52eb8aaa1edeeafe2ebe763581/ibis/backends/impala/ddl.py#L16'>ibis/backends/impala/ddl.py:16:9:</a> RUF052 [*] Local dummy variable `_format_aliases` is accessed
+ <a href='https://github.com/ibis-project/ibis/blob/35bd10ef46d77d52eb8aaa1edeeafe2ebe763581/ibis/backends/mssql/__init__.py#L720'>ibis/backends/mssql/__init__.py:720:17:</a> RUF052 Local dummy variable `_table` is accessed
- <a href='https://github.com/ibis-project/ibis/blob/35bd10ef46d77d52eb8aaa1edeeafe2ebe763581/ibis/backends/mssql/__init__.py#L720'>ibis/backends/mssql/__init__.py:720:17:</a> RUF052 [*] Local dummy variable `_table` is accessed
+ <a href='https://github.com/ibis-project/ibis/blob/35bd10ef46d77d52eb8aaa1edeeafe2ebe763581/ibis/backends/polars/compiler.py#L621'>ibis/backends/polars/compiler.py:621:5:</a> RUF052 Local dummy variable `_lpad` is accessed
- <a href='https://github.com/ibis-project/ibis/blob/35bd10ef46d77d52eb8aaa1edeeafe2ebe763581/ibis/backends/polars/compiler.py#L621'>ibis/backends/polars/compiler.py:621:5:</a> RUF052 [*] Local dummy variable `_lpad` is accessed
+ <a href='https://github.com/ibis-project/ibis/blob/35bd10ef46d77d52eb8aaa1edeeafe2ebe763581/ibis/backends/polars/compiler.py#L628'>ibis/backends/polars/compiler.py:628:5:</a> RUF052 Local dummy variable `_rpad` is accessed
- <a href='https://github.com/ibis-project/ibis/blob/35bd10ef46d77d52eb8aaa1edeeafe2ebe763581/ibis/backends/polars/compiler.py#L628'>ibis/backends/polars/compiler.py:628:5:</a> RUF052 [*] Local dummy variable `_rpad` is accessed
... 10 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +0 -12 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch_cli/services/get_params.py#L140'>src/latch_cli/services/get_params.py:140:17:</a> RUF052 Local dummy variable `_enum_literal` is accessed
- <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch_cli/services/get_params.py#L140'>src/latch_cli/services/get_params.py:140:17:</a> RUF052 [*] Local dummy variable `_enum_literal` is accessed
+ <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch_cli/services/get_params.py#L146'>src/latch_cli/services/get_params.py:146:21:</a> RUF052 Local dummy variable `_enum_literal` is accessed
- <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch_cli/services/get_params.py#L146'>src/latch_cli/services/get_params.py:146:21:</a> RUF052 [*] Local dummy variable `_enum_literal` is accessed
+ <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch_cli/services/launch.py#L143'>src/latch_cli/services/launch.py:143:5:</a> RUF052 Local dummy variable `_interface_request` is accessed
- <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch_cli/services/launch.py#L143'>src/latch_cli/services/launch.py:143:5:</a> RUF052 [*] Local dummy variable `_interface_request` is accessed
+ <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch_cli/services/launch.py#L208'>src/latch_cli/services/launch.py:208:5:</a> RUF052 Local dummy variable `_interface_request` is accessed
- <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch_cli/services/launch.py#L208'>src/latch_cli/services/launch.py:208:5:</a> RUF052 [*] Local dummy variable `_interface_request` is accessed
+ <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch_cli/services/register/utils.py#L127'>src/latch_cli/services/register/utils.py:127:5:</a> RUF052 Local dummy variable `_env` is accessed
- <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch_cli/services/register/utils.py#L127'>src/latch_cli/services/register/utils.py:127:5:</a> RUF052 [*] Local dummy variable `_env` is accessed
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+0 -0 violations, +0 -8 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/tests/wallets/fixtures/models.py#L35'>tests/wallets/fixtures/models.py:35:9:</a> RUF052 Local dummy variable `_mock` is accessed
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/tests/wallets/fixtures/models.py#L35'>tests/wallets/fixtures/models.py:35:9:</a> RUF052 [*] Local dummy variable `_mock` is accessed
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/tests/wallets/helpers.py#L117'>tests/wallets/helpers.py:117:13:</a> RUF052 Local dummy variable `_assert` is accessed
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/tests/wallets/helpers.py#L117'>tests/wallets/helpers.py:117:13:</a> RUF052 [*] Local dummy variable `_assert` is accessed
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/tests/wallets/test_rpc_wallets.py#L159'>tests/wallets/test_rpc_wallets.py:159:17:</a> RUF052 Local dummy variable `_mock_class` is accessed
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/tests/wallets/test_rpc_wallets.py#L159'>tests/wallets/test_rpc_wallets.py:159:17:</a> RUF052 [*] Local dummy variable `_mock_class` is accessed
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/tests/wallets/test_rpc_wallets.py#L66'>tests/wallets/test_rpc_wallets.py:66:9:</a> RUF052 Local dummy variable `_mock_class` is accessed
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/tests/wallets/test_rpc_wallets.py#L66'>tests/wallets/test_rpc_wallets.py:66:9:</a> RUF052 [*] Local dummy variable `_mock_class` is accessed
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF052 | 1068 | 66 | 0 | 0 | 1002 |

</p>
</details>




---

_Marked ready for review by @AlexWaygood on 2024-12-09 13:05_

---

_Comment by @AlexWaygood on 2024-12-09 13:07_

Thanks @dylwil3! I just pushed some docs to try to explain why it's marked as unsafe; could you take a quick second look?

---

_Review requested from @dylwil3 by @AlexWaygood on 2024-12-09 13:07_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/used_dummy_variable.rs`:59 on 2024-12-09 18:44_

How about:

```diff
- /// As well as renaming the variable itself, the fix iterates through all references to the
- /// variable and adapts them to become references to the renamed variable. However, some renamings
+ /// Ruff will attempt to rename the variable and all references to it. However, some renamings
```

---

_@dylwil3 approved on 2024-12-09 18:46_

Looks good! Suggested possible rewording, but it's also fine as is

---

_@AlexWaygood reviewed on 2024-12-09 22:58_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/used_dummy_variable.rs`:59 on 2024-12-09 22:58_

Hmm... I like the improved concision... but I'm not sure about talking about "renaming references". I feel like, strictly speaking, the binding is renamed, but the references do not have a "name" as such (so they cannot be renamed), they only _point to_ (refer) to a name.

I'll push something that's more concise than what I've got currently, though ;)

Thanks!

---

_Merged by @AlexWaygood on 2024-12-09 23:11_

---

_Closed by @AlexWaygood on 2024-12-09 23:11_

---

_Branch deleted on 2024-12-09 23:11_

---

_@Elno5 reviewed on 2024-12-10 23:50_

__.

---
