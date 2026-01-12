```yaml
number: 21076
title: "[`pydoclint`] Implement `docstring-missing-parameter` (`DOC101`)"
type: pull_request
state: open
author: augustelalande
labels: []
assignees: []
base: main
head: doc101
created_at: 2025-10-25T21:22:15Z
updated_at: 2025-10-25T21:33:06Z
url: https://github.com/astral-sh/ruff/pull/21076
synced_at: 2026-01-12T15:57:15Z
```

# [`pydoclint`] Implement `docstring-missing-parameter` (`DOC101`)

---

_@augustelalande_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement `docstring-missing-parameter` (`DOC101`). This rule checks that all parameters present in a functions signature are also documented in the docstring.

Split from #13280.

Part of #12434.

## Test Plan

Test cases added.


---

_Comment by @github-actions[bot] on 2025-10-25 21:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+13587 -19 violations, +0 -0 fixes in 7 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+218 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/79a8027a14e8ef99789490fd94f9b7d52b6a1422/disnake/abc.py#L1189'>disnake/abc.py:1189:9:</a> DOC101 Parameter `kwargs` missing from the docstring
+ <a href='https://github.com/DisnakeDev/disnake/blob/79a8027a14e8ef99789490fd94f9b7d52b6a1422/disnake/app_commands.py#L1114'>disnake/app_commands.py:1114:9:</a> DOC101 These parameters are missing from the docstring: `name`, `description`, `type`, `required`, `choices`, `options`, `channel_types`, `autocomplete`, `min_value`, `max_value`, `min_length`, `max_length`
+ <a href='https://github.com/DisnakeDev/disnake/blob/79a8027a14e8ef99789490fd94f9b7d52b6a1422/disnake/app_commands.py#L386'>disnake/app_commands.py:386:9:</a> DOC101 These parameters are missing from the docstring: `name`, `value`
+ <a href='https://github.com/DisnakeDev/disnake/blob/79a8027a14e8ef99789490fd94f9b7d52b6a1422/disnake/app_commands.py#L411'>disnake/app_commands.py:411:9:</a> DOC101 These parameters are missing from the docstring: `name`, `description`, `type`, `required`, `choices`, `options`, `channel_types`, `autocomplete`, `min_value`, `max_value`, `min_length`, `max_length`
+ <a href='https://github.com/DisnakeDev/disnake/blob/79a8027a14e8ef99789490fd94f9b7d52b6a1422/disnake/automod.py#L345'>disnake/automod.py:345:9:</a> DOC101 These parameters are missing from the docstring: `keyword_filter`, `regex_patterns`, `presets`, `allow_list`, `mention_total_limit`, `mention_raid_protection_enabled`
+ <a href='https://github.com/DisnakeDev/disnake/blob/79a8027a14e8ef99789490fd94f9b7d52b6a1422/disnake/channel.py#L1624'>disnake/channel.py:1624:9:</a> DOC101 Parameter `kwargs` missing from the docstring
+ <a href='https://github.com/DisnakeDev/disnake/blob/79a8027a14e8ef99789490fd94f9b7d52b6a1422/disnake/channel.py#L2478'>disnake/channel.py:2478:9:</a> DOC101 Parameter `kwargs` missing from the docstring
+ <a href='https://github.com/DisnakeDev/disnake/blob/79a8027a14e8ef99789490fd94f9b7d52b6a1422/disnake/channel.py#L3011'>disnake/channel.py:3011:9:</a> DOC101 Parameter `kwargs` missing from the docstring
+ <a href='https://github.com/DisnakeDev/disnake/blob/79a8027a14e8ef99789490fd94f9b7d52b6a1422/disnake/channel.py#L3205'>disnake/channel.py:3205:9:</a> DOC101 These parameters are missing from the docstring: `name`, `options`
+ <a href='https://github.com/DisnakeDev/disnake/blob/79a8027a14e8ef99789490fd94f9b7d52b6a1422/disnake/channel.py#L3220'>disnake/channel.py:3220:9:</a> DOC101 These parameters are missing from the docstring: `name`, `options`
... 208 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+7846 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/docs/conf.py#L147'>airflow-core/docs/conf.py:147:5:</a> DOC101 Parameter `exclude_patterns` missing from the docstring
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api/common/delete_dag.py#L44'>airflow-core/src/airflow/api/common/delete_dag.py:44:5:</a> DOC101 These parameters are missing from the docstring: `dag_id`, `keep_records_in_log`, `session`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api/common/mark_tasks.py#L110'>airflow-core/src/airflow/api/common/mark_tasks.py:110:5:</a> DOC101 These parameters are missing from the docstring: `dag`, `state`, `task_ids`, `run_ids`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api/common/mark_tasks.py#L124'>airflow-core/src/airflow/api/common/mark_tasks.py:124:5:</a> DOC101 These parameters are missing from the docstring: `tasks`, `downstream`, `upstream`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api/common/mark_tasks.py#L142'>airflow-core/src/airflow/api/common/mark_tasks.py:142:5:</a> DOC101 These parameters are missing from the docstring: `dag`, `run_id`, `future`, `past`, `session`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api/common/mark_tasks.py#L190'>airflow-core/src/airflow/api/common/mark_tasks.py:190:5:</a> DOC101 These parameters are missing from the docstring: `dag_id`, `run_id`, `state`, `session`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api/common/mark_tasks.py#L213'>airflow-core/src/airflow/api/common/mark_tasks.py:213:5:</a> DOC101 These parameters are missing from the docstring: `dag`, `run_id`, `commit`, `session`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api/common/mark_tasks.py#L272'>airflow-core/src/airflow/api/common/mark_tasks.py:272:5:</a> DOC101 These parameters are missing from the docstring: `dag`, `run_id`, `commit`, `session`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api/common/mark_tasks.py#L358'>airflow-core/src/airflow/api/common/mark_tasks.py:358:5:</a> DOC101 These parameters are missing from the docstring: `new_state`, `dag`, `run_id`, `commit`, `session`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api/common/mark_tasks.py#L389'>airflow-core/src/airflow/api/common/mark_tasks.py:389:5:</a> DOC101 These parameters are missing from the docstring: `dag`, `run_id`, `commit`, `session`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api/common/mark_tasks.py#L55'>airflow-core/src/airflow/api/common/mark_tasks.py:55:5:</a> DOC101 These parameters are missing from the docstring: `tasks`, `run_id`, `upstream`, `downstream`, `future`, `past`, `state`, `commit`, `session`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api/common/trigger_dag.py#L140'>airflow-core/src/airflow/api/common/trigger_dag.py:140:5:</a> DOC101 These parameters are missing from the docstring: `dag_id`, `triggered_by`, `triggering_user_name`, `run_after`, `run_id`, `conf`, `logical_date`, `replace_microseconds`, `session`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api/common/trigger_dag.py#L55'>airflow-core/src/airflow/api/common/trigger_dag.py:55:5:</a> DOC101 These parameters are missing from the docstring: `dag_id`, `dag_bag`, `triggered_by`, `triggering_user_name`, `run_after`, `run_id`, `conf`, `logical_date`, `replace_microseconds`, `session`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api_fastapi/app.py#L111'>airflow-core/src/airflow/api_fastapi/app.py:111:5:</a> DOC101 These parameters are missing from the docstring: `config`, `testing`, `apps`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api_fastapi/app.py#L150'>airflow-core/src/airflow/api_fastapi/app.py:150:5:</a> DOC101 Parameter `app` missing from the docstring
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api_fastapi/app.py#L175'>airflow-core/src/airflow/api_fastapi/app.py:175:5:</a> DOC101 Parameter `app` missing from the docstring
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L109'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:109:9:</a> DOC101 Parameter `token` missing from the docstring
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L125'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:125:9:</a> DOC101 These parameters are missing from the docstring: `user`, `expiration_time_in_seconds`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L145'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:145:9:</a> DOC101 Parameter `user` missing from the docstring
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L333'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:333:9:</a> DOC101 These parameters are missing from the docstring: `requests`, `user`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L358'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:358:9:</a> DOC101 These parameters are missing from the docstring: `requests`, `user`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L384'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:384:9:</a> DOC101 These parameters are missing from the docstring: `requests`, `user`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L409'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:409:9:</a> DOC101 These parameters are missing from the docstring: `requests`, `user`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L436'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:436:9:</a> DOC101 These parameters are missing from the docstring: `user`, `method`, `session`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L467'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:467:9:</a> DOC101 These parameters are missing from the docstring: `conn_ids`, `user`, `method`, `team_name`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L496'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:496:9:</a> DOC101 These parameters are missing from the docstring: `user`, `method`, `session`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L536'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:536:9:</a> DOC101 These parameters are missing from the docstring: `dag_ids`, `user`, `method`, `team_name`
+ <a href='https://github.com/apache/airflow/blob/a0b4d2979bf6e72c2d99bb3a670dfc097b390180/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L565'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:565:9:</a> DOC101 These parameters are missing from the docstring: `user`, `method`, `session`
... 7818 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+2175 -13 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/cc6a5dc29a45f6c75dcbd2f387d172695ba90926/RELEASING/changelog.py#L107'>RELEASING/changelog.py:107:9:</a> DOC101 Parameter `git_log` missing from the docstring
+ <a href='https://github.com/apache/superset/blob/cc6a5dc29a45f6c75dcbd2f387d172695ba90926/RELEASING/changelog.py#L335'>RELEASING/changelog.py:335:5:</a> DOC101 These parameters are missing from the docstring: `ctx`, `previous_version`, `current_version`
+ <a href='https://github.com/apache/superset/blob/cc6a5dc29a45f6c75dcbd2f387d172695ba90926/RELEASING/changelog.py#L347'>RELEASING/changelog.py:347:5:</a> DOC101 Parameter `base_parameters` missing from the docstring
+ <a href='https://github.com/apache/superset/blob/cc6a5dc29a45f6c75dcbd2f387d172695ba90926/RELEASING/changelog.py#L380'>RELEASING/changelog.py:380:5:</a> D400 First line should end with a period
- <a href='https://github.com/apache/superset/blob/cc6a5dc29a45f6c75dcbd2f387d172695ba90926/RELEASING/changelog.py#L380'>RELEASING/changelog.py:380:5:</a> D400 First line should end with a period
+ <a href='https://github.com/apache/superset/blob/cc6a5dc29a45f6c75dcbd2f387d172695ba90926/RELEASING/changelog.py#L380'>RELEASING/changelog.py:380:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/apache/superset/blob/cc6a5dc29a45f6c75dcbd2f387d172695ba90926/RELEASING/changelog.py#L380'>RELEASING/changelog.py:380:5:</a> D415 First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/apache/superset/blob/cc6a5dc29a45f6c75dcbd2f387d172695ba90926/RELEASING/changelog.py#L380'>RELEASING/changelog.py:380:5:</a> DOC101 These parameters are missing from the docstring: `base_parameters`, `csv`, `access_token`, `risk`
+ <a href='https://github.com/apache/superset/blob/cc6a5dc29a45f6c75dcbd2f387d172695ba90926/RELEASING/changelog.py#L52'>RELEASING/changelog.py:52:9:</a> DOC101 Parameter `other` missing from the docstring
+ <a href='https://github.com/apache/superset/blob/cc6a5dc29a45f6c75dcbd2f387d172695ba90926/RELEASING/changelog.py#L87'>RELEASING/changelog.py:87:9:</a> DOC101 Parameter `pr_number` missing from the docstring
... 2157 additional changes omitted for rule DOC101
... 2178 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+313 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L15'>examples/advanced/extensions/parallel_plot/parallel_plot.py:15:5:</a> DOC101 These parameters are missing from the docstring: `df`, `color`, `palette`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/js_on_event.py#L16'>examples/interaction/js_callbacks/js_on_event.py:16:5:</a> DOC101 These parameters are missing from the docstring: `div`, `attributes`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/gauges.py#L33'>examples/models/gauges.py:33:5:</a> DOC101 Parameter `val` missing from the docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/trail.py#L20'>examples/models/trail.py:20:5:</a> DOC101 These parameters are missing from the docstring: `p1`, `p2`
+ examples/output/jupyter/push_notebook/Numba Image Example.ipynb:cell 22:3:5: DOC101 These parameters are missing from the docstring: `img`, `tmp`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/events_app.py#L17'>examples/server/app/events_app.py:17:5:</a> DOC101 These parameters are missing from the docstring: `div`, `attributes`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/events_app.py#L39'>examples/server/app/events_app.py:39:5:</a> DOC101 Parameter `attributes` missing from the docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/geo/tile_demo.py#L10'>examples/topics/geo/tile_demo.py:10:5:</a> DOC101 These parameters are missing from the docstring: `longitude`, `latitude`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/milestone.py#L196'>scripts/milestone.py:196:5:</a> DOC101 These parameters are missing from the docstring: `title`, `token`, `allow_closed`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/milestone.py#L26'>scripts/milestone.py:26:5:</a> DOC101 These parameters are missing from the docstring: `query`, `token`
... 303 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+2408 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/f3d7152074f3f19c2387dae685d1646d2d1cb286/libs/cli/langchain_cli/cli.py#L74'>libs/cli/langchain_cli/cli.py:74:5:</a> DOC101 These parameters are missing from the docstring: `port`, `host`
+ <a href='https://github.com/langchain-ai/langchain/blob/f3d7152074f3f19c2387dae685d1646d2d1cb286/libs/cli/langchain_cli/namespaces/app.py#L159'>libs/cli/langchain_cli/namespaces/app.py:159:5:</a> DOC101 These parameters are missing from the docstring: `dependencies`, `api_path`, `project_dir`, `repo`, `branch`, `pip`
+ <a href='https://github.com/langchain-ai/langchain/blob/f3d7152074f3f19c2387dae685d1646d2d1cb286/libs/cli/langchain_cli/namespaces/app.py#L312'>libs/cli/langchain_cli/namespaces/app.py:312:5:</a> DOC101 These parameters are missing from the docstring: `api_paths`, `project_dir`
+ <a href='https://github.com/langchain-ai/langchain/blob/f3d7152074f3f19c2387dae685d1646d2d1cb286/libs/cli/langchain_cli/namespaces/app.py#L359'>libs/cli/langchain_cli/namespaces/app.py:359:5:</a> DOC101 These parameters are missing from the docstring: `port`, `host`, `app`
+ <a href='https://github.com/langchain-ai/langchain/blob/f3d7152074f3f19c2387dae685d1646d2d1cb286/libs/cli/langchain_cli/namespaces/app.py#L63'>libs/cli/langchain_cli/namespaces/app.py:63:5:</a> DOC101 These parameters are missing from the docstring: `name`, `package`, `pip`, `noninteractive`
+ <a href='https://github.com/langchain-ai/langchain/blob/f3d7152074f3f19c2387dae685d1646d2d1cb286/libs/cli/langchain_cli/namespaces/integration.py#L248'>libs/cli/langchain_cli/namespaces/integration.py:248:5:</a> DOC101 These parameters are missing from the docstring: `name`, `name_class`, `component_type`, `destination_dir`
... 2397 additional changes omitted for rule DOC101
+ <a href='https://github.com/langchain-ai/langchain/blob/f3d7152074f3f19c2387dae685d1646d2d1cb286/libs/core/langchain_core/language_models/fake_chat_models.py#L317'>libs/core/langchain_core/language_models/fake_chat_models.py:317:9:</a> PLR1702 Too many nested blocks (6 > 5)
- <a href='https://github.com/langchain-ai/langchain/blob/f3d7152074f3f19c2387dae685d1646d2d1cb286/libs/core/langchain_core/language_models/fake_chat_models.py#L317'>libs/core/langchain_core/language_models/fake_chat_models.py:317:9:</a> PLR1702 Too many nested blocks (6 > 5)
- <a href='https://github.com/langchain-ai/langchain/blob/f3d7152074f3f19c2387dae685d1646d2d1cb286/libs/core/langchain_core/runnables/graph_mermaid.py#L410'>libs/core/langchain_core/runnables/graph_mermaid.py:410:5:</a> DOC501 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/langchain-ai/langchain/blob/f3d7152074f3f19c2387dae685d1646d2d1cb286/libs/core/langchain_core/runnables/graph_mermaid.py#L410'>libs/core/langchain_core/runnables/graph_mermaid.py:410:5:</a> DOC501 Raised exception `ValueError` missing from docstring
... 2404 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+15 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/fe0f946dc0c240c6c1e513318c21db407e191c78/reflex/event.py#L854'>reflex/event.py:854:9:</a> DOC101 Parameter `_prog` missing from the docstring
+ <a href='https://github.com/reflex-dev/reflex/blob/fe0f946dc0c240c6c1e513318c21db407e191c78/reflex/reflex.py#L129'>reflex/reflex.py:129:5:</a> DOC101 These parameters are missing from the docstring: `name`, `template`, `ai`
+ <a href='https://github.com/reflex-dev/reflex/blob/fe0f946dc0c240c6c1e513318c21db407e191c78/reflex/reflex.py#L142'>reflex/reflex.py:142:5:</a> DOC101 These parameters are missing from the docstring: `env`, `frontend`, `backend`, `frontend_port`, `backend_port`, `backend_host`, `single_port`
+ <a href='https://github.com/reflex-dev/reflex/blob/fe0f946dc0c240c6c1e513318c21db407e191c78/reflex/reflex.py#L359'>reflex/reflex.py:359:5:</a> DOC101 These parameters are missing from the docstring: `env`, `frontend_only`, `backend_only`, `frontend_port`, `backend_port`, `backend_host`, `single_port`
+ <a href='https://github.com/reflex-dev/reflex/blob/fe0f946dc0c240c6c1e513318c21db407e191c78/reflex/reflex.py#L417'>reflex/reflex.py:417:5:</a> DOC101 These parameters are missing from the docstring: `dry`, `rich`
+ <a href='https://github.com/reflex-dev/reflex/blob/fe0f946dc0c240c6c1e513318c21db407e191c78/reflex/reflex.py#L497'>reflex/reflex.py:497:5:</a> DOC101 These parameters are missing from the docstring: `zip`, `frontend_only`, `backend_only`, `zip_dest_dir`, `upload_db_file`, `env`, `backend_excluded_dirs`, `ssr`
+ <a href='https://github.com/reflex-dev/reflex/blob/fe0f946dc0c240c6c1e513318c21db407e191c78/reflex/reflex.py#L63'>reflex/reflex.py:63:5:</a> DOC101 These parameters are missing from the docstring: `name`, `template`, `ai`
+ <a href='https://github.com/reflex-dev/reflex/blob/fe0f946dc0c240c6c1e513318c21db407e191c78/reflex/reflex.py#L658'>reflex/reflex.py:658:5:</a> DOC101 Parameter `message` missing from the docstring
+ <a href='https://github.com/reflex-dev/reflex/blob/fe0f946dc0c240c6c1e513318c21db407e191c78/reflex/reflex.py#L767'>reflex/reflex.py:767:5:</a> DOC101 These parameters are missing from the docstring: `app_name`, `app_id`, `region`, `env`, `vmtype`, `hostname`, `interactive`, `envfile`, `project`, `project_name`, `token`, `config_path`, `backend_excluded_dirs`, `ssr`
+ <a href='https://github.com/reflex-dev/reflex/blob/fe0f946dc0c240c6c1e513318c21db407e191c78/reflex/reflex.py#L841'>reflex/reflex.py:841:5:</a> DOC101 Parameter `new_name` missing from the docstring
... 5 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (9 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DOC101 | 13568 | 13568 | 0 | 0 | 0 |
| D415 | 12 | 6 | 6 | 0 | 0 |
| D200 | 6 | 3 | 3 | 0 | 0 |
| DOC501 | 6 | 3 | 3 | 0 | 0 |
| D202 | 4 | 2 | 2 | 0 | 0 |
| DOC201 | 4 | 2 | 2 | 0 | 0 |
| D400 | 2 | 1 | 1 | 0 | 0 |
| D212 | 2 | 1 | 1 | 0 | 0 |
| PLR1702 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---
