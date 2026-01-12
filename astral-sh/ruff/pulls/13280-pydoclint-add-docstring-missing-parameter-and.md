```yaml
number: 13280
title: "[`pydoclint`] Add `docstring-missing-parameter` and `docstring-extraneous-parameter` (`DOC101`, `DOC102`)"
type: pull_request
state: open
author: augustelalande
labels:
  - rule
  - preview
assignees: []
base: main
head: doc10x
created_at: 2024-09-07T23:57:55Z
updated_at: 2025-10-29T15:05:28Z
url: https://github.com/astral-sh/ruff/pull/13280
synced_at: 2026-01-12T15:55:43Z
```

# [`pydoclint`] Add `docstring-missing-parameter` and `docstring-extraneous-parameter` (`DOC101`, `DOC102`)

---

_@augustelalande_

## Summary

Add `docstring-missing-parameter` and `docstring-extraneous-parameter` (`DOC101`, `DOC102`). These rules check that the parameters defined in a functions signature match thos defined in the docstring.

Part of #12434.

## Test Plan

Test cases added.

---

_Comment by @codspeed-hq[bot] on 2024-09-08 00:15_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/augustelalande%3Adoc10x?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #13280 will **not alter performance**

<sub>Comparing <code>augustelalande:doc10x</code> (8639117) with <code>main</code> (ec86a4e)</sub>



### Summary

`✅ 42` untouched  





---

_Comment by @github-actions[bot] on 2024-09-08 00:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+12319 -13 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+7445 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/docs/conf.py#L147'>airflow-core/docs/conf.py:147:5:</a> DOC101 Parameter `exclude_patterns` missing from the docstring
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api/common/delete_dag.py#L44'>airflow-core/src/airflow/api/common/delete_dag.py:44:5:</a> DOC101 These parameters are missing from the docstring: `dag_id`, `keep_records_in_log`, `session`
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api/common/mark_tasks.py#L114'>airflow-core/src/airflow/api/common/mark_tasks.py:114:5:</a> DOC101 These parameters are missing from the docstring: `dag`, `state`, `task_ids`, `run_ids`
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api/common/mark_tasks.py#L128'>airflow-core/src/airflow/api/common/mark_tasks.py:128:5:</a> DOC101 These parameters are missing from the docstring: `tasks`, `downstream`, `upstream`
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api/common/mark_tasks.py#L146'>airflow-core/src/airflow/api/common/mark_tasks.py:146:5:</a> DOC101 These parameters are missing from the docstring: `dag`, `run_id`, `future`, `past`, `session`
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api/common/mark_tasks.py#L180'>airflow-core/src/airflow/api/common/mark_tasks.py:180:5:</a> DOC101 These parameters are missing from the docstring: `dag_id`, `run_id`, `state`, `session`
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api/common/mark_tasks.py#L203'>airflow-core/src/airflow/api/common/mark_tasks.py:203:5:</a> DOC101 These parameters are missing from the docstring: `dag`, `run_id`, `commit`, `session`
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api/common/mark_tasks.py#L264'>airflow-core/src/airflow/api/common/mark_tasks.py:264:5:</a> DOC101 These parameters are missing from the docstring: `dag`, `run_id`, `commit`, `session`
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api/common/mark_tasks.py#L356'>airflow-core/src/airflow/api/common/mark_tasks.py:356:5:</a> DOC101 These parameters are missing from the docstring: `new_state`, `dag`, `run_id`, `commit`, `session`
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api/common/mark_tasks.py#L387'>airflow-core/src/airflow/api/common/mark_tasks.py:387:5:</a> DOC101 These parameters are missing from the docstring: `dag`, `run_id`, `commit`, `session`
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api/common/mark_tasks.py#L56'>airflow-core/src/airflow/api/common/mark_tasks.py:56:5:</a> DOC101 These parameters are missing from the docstring: `tasks`, `run_id`, `upstream`, `downstream`, `future`, `past`, `state`, `commit`, `session`
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api/common/trigger_dag.py#L140'>airflow-core/src/airflow/api/common/trigger_dag.py:140:5:</a> DOC101 These parameters are missing from the docstring: `dag_id`, `triggered_by`, `triggering_user_name`, `run_after`, `run_id`, `conf`, `logical_date`, `replace_microseconds`, `session`
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api/common/trigger_dag.py#L55'>airflow-core/src/airflow/api/common/trigger_dag.py:55:5:</a> DOC101 These parameters are missing from the docstring: `dag_id`, `dag_bag`, `triggered_by`, `triggering_user_name`, `run_after`, `run_id`, `conf`, `logical_date`, `replace_microseconds`, `session`
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api_fastapi/app.py#L108'>airflow-core/src/airflow/api_fastapi/app.py:108:5:</a> DOC101 These parameters are missing from the docstring: `config`, `testing`, `apps`
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api_fastapi/app.py#L147'>airflow-core/src/airflow/api_fastapi/app.py:147:5:</a> DOC101 Parameter `app` missing from the docstring
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api_fastapi/app.py#L172'>airflow-core/src/airflow/api_fastapi/app.py:172:5:</a> DOC101 Parameter `app` missing from the docstring
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L100'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:100:9:</a> DOC101 Parameter `token` missing from the docstring
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L116'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:116:9:</a> DOC101 These parameters are missing from the docstring: `user`, `expiration_time_in_seconds`
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L321'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:321:9:</a> DOC101 These parameters are missing from the docstring: `requests`, `user`
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L349'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:349:9:</a> DOC101 These parameters are missing from the docstring: `user`, `method`, `session`
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L370'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:370:9:</a> DOC101 These parameters are missing from the docstring: `dag_ids`, `user`, `method`
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L401'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:401:9:</a> DOC101 Parameter `user` missing from the docstring
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L405'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:405:9:</a> DOC101 Parameter `user` missing from the docstring
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L427'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:427:9:</a> DOC101 Parameter `expiration_time_in_seconds` missing from the docstring
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api_fastapi/auth/managers/simple/routes/login.py#L109'>airflow-core/src/airflow/api_fastapi/auth/managers/simple/routes/login.py:109:5:</a> DOC101 Parameter `body` missing from the docstring
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api_fastapi/auth/managers/simple/routes/login.py#L66'>airflow-core/src/airflow/api_fastapi/auth/managers/simple/routes/login.py:66:5:</a> DOC101 Parameter `body` missing from the docstring
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api_fastapi/auth/managers/simple/routes/login.py#L86'>airflow-core/src/airflow/api_fastapi/auth/managers/simple/routes/login.py:86:5:</a> DOC101 Parameter `request` missing from the docstring
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api_fastapi/auth/managers/simple/services/login.py#L36'>airflow-core/src/airflow/api_fastapi/auth/managers/simple/services/login.py:36:9:</a> DOC101 These parameters are missing from the docstring: `body`, `expiration_time_in_seconds`
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api_fastapi/auth/managers/simple/simple_auth_manager.py#L146'>airflow-core/src/airflow/api_fastapi/auth/managers/simple/simple_auth_manager.py:146:9:</a> DOC101 Parameter `kwargs` missing from the docstring
+ <a href='https://github.com/apache/airflow/blob/5babf15bb4289419e3326aded0670883113d01c0/airflow-core/src/airflow/api_fastapi/auth/managers/simple/simple_auth_manager.py#L325'>airflow-core/src/airflow/api_fastapi/auth/managers/simple/simple_auth_manager.py:325:9:</a> DOC101 These parameters are missing from the docstring: `method`, `allow_role`, `user`, `allow_get_role`
... 7415 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1972 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/RELEASING/changelog.py#L107'>RELEASING/changelog.py:107:9:</a> DOC101 Parameter `git_log` missing from the docstring
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/RELEASING/changelog.py#L335'>RELEASING/changelog.py:335:5:</a> DOC101 These parameters are missing from the docstring: `ctx`, `previous_version`, `current_version`
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/RELEASING/changelog.py#L347'>RELEASING/changelog.py:347:5:</a> DOC101 Parameter `base_parameters` missing from the docstring
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/RELEASING/changelog.py#L380'>RELEASING/changelog.py:380:5:</a> D400 First line should end with a period
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/RELEASING/changelog.py#L380'>RELEASING/changelog.py:380:5:</a> D400 First line should end with a period
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/RELEASING/changelog.py#L380'>RELEASING/changelog.py:380:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/RELEASING/changelog.py#L380'>RELEASING/changelog.py:380:5:</a> D415 First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/RELEASING/changelog.py#L380'>RELEASING/changelog.py:380:5:</a> DOC101 These parameters are missing from the docstring: `base_parameters`, `csv`, `access_token`, `risk`
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/RELEASING/changelog.py#L52'>RELEASING/changelog.py:52:9:</a> DOC101 Parameter `other` missing from the docstring
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/RELEASING/changelog.py#L87'>RELEASING/changelog.py:87:9:</a> DOC101 Parameter `pr_number` missing from the docstring
... 1930 additional changes omitted for rule DOC101
... 1973 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+342 -0 violations, +0 -0 fixes)</summary>
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
... 306 additional changes omitted for rule DOC101
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L424'>src/bokeh/core/has_props.py:424:9:</a> DOC102 These documented parameters are not in the function's signature: `json`, `models`, `setter(ClientSession`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/dataspec.py#L449'>src/bokeh/core/property/dataspec.py:449:9:</a> DOC102 Documented parameter `name` is not in the function's signature
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptors.py#L399'>src/bokeh/core/property/descriptors.py:399:9:</a> DOC102 These documented parameters are not in the function's signature: `json`, `models`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptors.py#L664'>src/bokeh/core/property/descriptors.py:664:9:</a> DOC102 Documented parameter `new` is not in the function's signature
... 332 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1895 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/2d0713c2fc5a457578635b03b7a00e970ce534ee/libs/cli/langchain_cli/cli.py#L72'>libs/cli/langchain_cli/cli.py:72:5:</a> DOC101 These parameters are missing from the docstring: `port`, `host`
+ <a href='https://github.com/langchain-ai/langchain/blob/2d0713c2fc5a457578635b03b7a00e970ce534ee/libs/cli/langchain_cli/dev_scripts.py#L17'>libs/cli/langchain_cli/dev_scripts.py:17:5:</a> DOC101 These parameters are missing from the docstring: `config_keys`, `playground_type`
+ <a href='https://github.com/langchain-ai/langchain/blob/2d0713c2fc5a457578635b03b7a00e970ce534ee/libs/cli/langchain_cli/namespaces/app.py#L164'>libs/cli/langchain_cli/namespaces/app.py:164:5:</a> DOC101 These parameters are missing from the docstring: `dependencies`, `api_path`, `project_dir`, `repo`, `branch`, `pip`
+ <a href='https://github.com/langchain-ai/langchain/blob/2d0713c2fc5a457578635b03b7a00e970ce534ee/libs/cli/langchain_cli/namespaces/app.py#L317'>libs/cli/langchain_cli/namespaces/app.py:317:5:</a> DOC101 These parameters are missing from the docstring: `api_paths`, `project_dir`
+ <a href='https://github.com/langchain-ai/langchain/blob/2d0713c2fc5a457578635b03b7a00e970ce534ee/libs/cli/langchain_cli/namespaces/app.py#L364'>libs/cli/langchain_cli/namespaces/app.py:364:5:</a> DOC101 These parameters are missing from the docstring: `port`, `host`, `app`
+ <a href='https://github.com/langchain-ai/langchain/blob/2d0713c2fc5a457578635b03b7a00e970ce534ee/libs/cli/langchain_cli/namespaces/app.py#L65'>libs/cli/langchain_cli/namespaces/app.py:65:5:</a> DOC101 These parameters are missing from the docstring: `name`, `package`, `pip`, `noninteractive`
... 1866 additional changes omitted for rule DOC101
+ <a href='https://github.com/langchain-ai/langchain/blob/2d0713c2fc5a457578635b03b7a00e970ce534ee/libs/core/langchain_core/beta/runnables/context.py#L399'>libs/core/langchain_core/beta/runnables/context.py:399:9:</a> DOC102 These documented parameters are not in the function's signature: `_key`, `_value`
+ <a href='https://github.com/langchain-ai/langchain/blob/2d0713c2fc5a457578635b03b7a00e970ce534ee/libs/core/langchain_core/beta/runnables/context.py#L437'>libs/core/langchain_core/beta/runnables/context.py:437:9:</a> DOC102 These documented parameters are not in the function's signature: `_key`, `_value`
+ <a href='https://github.com/langchain-ai/langchain/blob/2d0713c2fc5a457578635b03b7a00e970ce534ee/libs/core/langchain_core/callbacks/file.py#L132'>libs/core/langchain_core/callbacks/file.py:132:9:</a> DOC102 Documented parameter `file` is not in the function's signature
+ <a href='https://github.com/langchain-ai/langchain/blob/2d0713c2fc5a457578635b03b7a00e970ce534ee/libs/core/langchain_core/messages/ai.py#L313'>libs/core/langchain_core/messages/ai.py:313:9:</a> DOC102 Documented parameter `values` is not in the function's signature
... 1887 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+45 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/d9b242a757185ca65266ddb2f3c6a65c8304c56a/reflex/app.py#L1780'>reflex/app.py:1780:5:</a> DOC102 Documented parameter `_request` is not in the function's signature
+ <a href='https://github.com/reflex-dev/reflex/blob/d9b242a757185ca65266ddb2f3c6a65c8304c56a/reflex/app.py#L1792'>reflex/app.py:1792:5:</a> DOC102 Documented parameter `_request` is not in the function's signature
+ <a href='https://github.com/reflex-dev/reflex/blob/d9b242a757185ca65266ddb2f3c6a65c8304c56a/reflex/assets.py#L38'>reflex/assets.py:38:5:</a> DOC102 Documented parameter `_stack_level` is not in the function's signature
+ <a href='https://github.com/reflex-dev/reflex/blob/d9b242a757185ca65266ddb2f3c6a65c8304c56a/reflex/components/component.py#L2919'>reflex/components/component.py:2919:9:</a> DOC102 Documented parameter `_var_data` is not in the function's signature
+ <a href='https://github.com/reflex-dev/reflex/blob/d9b242a757185ca65266ddb2f3c6a65c8304c56a/reflex/event.py#L1907'>reflex/event.py:1907:9:</a> DOC102 Documented parameter `_var_data` is not in the function's signature
+ <a href='https://github.com/reflex-dev/reflex/blob/d9b242a757185ca65266ddb2f3c6a65c8304c56a/reflex/event.py#L1994'>reflex/event.py:1994:9:</a> DOC102 Documented parameter `_var_data` is not in the function's signature
... 28 additional changes omitted for rule DOC102
+ <a href='https://github.com/reflex-dev/reflex/blob/d9b242a757185ca65266ddb2f3c6a65c8304c56a/reflex/reflex.py#L124'>reflex/reflex.py:124:5:</a> DOC101 These parameters are missing from the docstring: `name`, `template`, `ai`
+ <a href='https://github.com/reflex-dev/reflex/blob/d9b242a757185ca65266ddb2f3c6a65c8304c56a/reflex/reflex.py#L136'>reflex/reflex.py:136:5:</a> DOC101 These parameters are missing from the docstring: `env`, `frontend`, `backend`, `frontend_port`, `backend_port`, `backend_host`
+ <a href='https://github.com/reflex-dev/reflex/blob/d9b242a757185ca65266ddb2f3c6a65c8304c56a/reflex/reflex.py#L330'>reflex/reflex.py:330:5:</a> DOC101 These parameters are missing from the docstring: `env`, `frontend_only`, `backend_only`, `frontend_port`, `backend_port`, `backend_host`
+ <a href='https://github.com/reflex-dev/reflex/blob/d9b242a757185ca65266ddb2f3c6a65c8304c56a/reflex/reflex.py#L364'>reflex/reflex.py:364:5:</a> DOC101 Parameter `dry` missing from the docstring
... 35 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+620 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/analytics/lib/fixtures.py#L19'>analytics/lib/fixtures.py:19:5:</a> DOC101 These parameters are missing from the docstring: `days`, `business_hours_base`, `non_business_hours_base`, `growth`, `autocorrelation`, `spikiness`, `holiday_rate`, `frequency`, `partial_sum`, `random_seed`
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/analytics/migrations/0015_clear_duplicate_counts.py#L8'>analytics/migrations/0015_clear_duplicate_counts.py:8:5:</a> DOC101 These parameters are missing from the docstring: `apps`, `schema_editor`
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/analytics/tests/test_counts.py#L222'>analytics/tests/test_counts.py:222:9:</a> DOC101 These parameters are missing from the docstring: `table`, `arg_keys`, `arg_values`
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/confirmation/migrations/0016_realmcreationkey_to_realmcreationstatus.py#L15'>confirmation/migrations/0016_realmcreationkey_to_realmcreationstatus.py:15:5:</a> DOC101 These parameters are missing from the docstring: `apps`, `schema_editor`
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/confirmation/models.py#L296'>confirmation/models.py:296:5:</a> DOC101 These parameters are missing from the docstring: `user_profile`, `email_type`
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/confirmation/models.py#L92'>confirmation/models.py:92:5:</a> DOC101 These parameters are missing from the docstring: `confirmation_key`, `confirmation_types`, `mark_object_used`, `allow_used`
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/corporate/lib/activity.py#L93'>corporate/lib/activity.py:93:5:</a> DOC101 Parameter `cursor` missing from the docstring
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/corporate/lib/stripe.py#L1143'>corporate/lib/stripe.py:1143:9:</a> DOC101 Parameter `metadata` missing from the docstring
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/corporate/lib/stripe.py#L386'>corporate/lib/stripe.py:386:5:</a> DOC101 These parameters are missing from the docstring: `customer`, `plan_tier`
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/corporate/lib/stripe.py#L5370'>corporate/lib/stripe.py:5370:5:</a> DOC101 Parameter `remote_server` missing from the docstring
... 610 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (8 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DOC101 | 12194 | 12194 | 0 | 0 | 0 |
| DOC102 | 112 | 112 | 0 | 0 | 0 |
| D415 | 8 | 4 | 4 | 0 | 0 |
| D202 | 4 | 2 | 2 | 0 | 0 |
| D200 | 4 | 2 | 2 | 0 | 0 |
| D212 | 4 | 2 | 2 | 0 | 0 |
| DOC201 | 4 | 2 | 2 | 0 | 0 |
| D400 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2024-09-10 18:32_

---

_Label `preview` added by @MichaReiser on 2024-09-10 18:32_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:127 on 2024-09-10 18:37_

I like grouping extraneous parameters to reduce the total number of parameters. It only feels a bit inconsistent that we don't group missing parameters the same way. Is it an intentional decision to only group extraneous parameters?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:143 on 2024-09-10 18:39_

I think we could go with a more generic message here and just say `Remove the extraneous parameters from the docstring` because the same information is already present in the message.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:595 on 2024-09-10 18:40_

Is it intentional that `SectionKind::` uses `Args`/`Arguments` and `ParametersSection` and `docstring_sections` use the term `Parameters`?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:622 on 2024-09-10 18:44_

I think we might need some extra handling for multiline parameter descriptions because they could contain a colon:

```
    Args:
        param1 (int): The first parameter.
        param2 (:obj:`str`, optional): The second parameter. Defaults to None.
            Second line of description: should be indented.
        *args: Variable length argument list.
        **kwargs: Arbitrary keyword arguments.
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:650 on 2024-09-10 18:46_

It seems that the `:` is optional for numpy docstrings

```python
def function_with_pep484_type_annotations(param1: int, param2: str) -> bool:
    """Example function with PEP 484 type annotations.

    The return type must be duplicated in the docstring to comply
    with the NumPy docstring style.

    Parameters
    ----------
    param1
        The first parameter.
    param2
        The second parameter.

    Returns
    -------
    bool
        True if successful, False otherwise.

    """
```

[source](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_numpy.html#example-numpy)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:1082 on 2024-09-10 18:48_

Should this respect the `dummy_variable_rgx` option:

https://github.com/astral-sh/ruff/blob/3ed707f2457a464b7c3bbe120c40a6badf81c270/crates/ruff_linter/src/rules/pylint/rules/redefined_loop_name.rs#L290

It might be worth introducing a helper function

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:1079 on 2024-09-10 18:48_

Nit: You could call `param.name()` directly

---

_@MichaReiser reviewed on 2024-09-10 18:49_

This is great. I haven't gone through the ecossytem results but I think there are a few cases where we don't parse the parameter names correctly.

---

_@augustelalande reviewed on 2024-09-10 20:51_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:595 on 2024-09-10 20:51_

The SectionKind is based on what the sections are usually called by convention. Google typically uses "Args" while NumPy typically uses "Parameters". I think the more correct term is parameters, so I called it `ParametersSection`.

---

_@augustelalande reviewed on 2024-09-10 21:14_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:650 on 2024-09-10 21:14_

This should still work though

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:584 on 2024-09-10 21:54_

What's the motivation for adding `Return` here? Is this a fix? Should this be its own PR? 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:631 on 2024-09-10 21:55_

Do we need a `break` after this line? We otherwise find the indentation of the last item rather than the first.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:639 on 2024-09-10 21:57_

Nit
```suggestion
            if entry.chars().next().is_some_and(|first_char| !first_char.is_whitespace())
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:643 on 2024-09-10 21:57_

Nit
```suggestion
                    let Some(before_colon) = entry.split_once(':') else {
                        continue;
                    };
                    if let Some(param) = before_colon.split_whitespace().next() {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:644 on 2024-09-10 21:58_

Is it okay to trim `*` and `**` from all parameters or should it only do so from `args` and `kwargs`?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:674 on 2024-09-10 22:00_

Nit
```suggestion
            if entry.chars().next().is_some_and(|first_char| !first_char.is_whitespace())
                    if let Some(param) = entry.split(':').next() {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/snapshots/ruff_linter__rules__pydoclint__tests__docstring-extraneous-parameter_DOC102_google.py.snap`:25 on 2024-09-10 22:04_

Highlighting a multiline-range always leads to somewhat awkward code frame rendering. I think it would be better to change the diagnostic range to the `multiplier` code frame rather than the entire args section. Or is there precedence in other pydocstyle rules to highlight the entire section?

If this range is due to creating a single diagnostic for all the function's extraneous parameters than I think I prefer having multiple diagnostics instead (also makes it easier to suppress an error in case that's needed without risking silencing new extraneous parameters)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/snapshots/ruff_linter__rules__pydoclint__tests__docstring-extraneous-parameter_DOC102_google.py.snap`:65 on 2024-09-10 22:07_

Nit: What do you think of "Documented parameter `tax_rate` is not in the function's signature.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/snapshots/ruff_linter__rules__pydoclint__tests__docstring-extraneous-parameter_DOC102_google.py.snap`:39 on 2024-09-10 22:08_

Would you mind adding an example for a multiline argument description that also contains a `:`?

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pydoclint/DOC101_google.py`:372 on 2024-09-10 22:10_

What happens if the method has overloads? Is it required to document all parameters for each overload? What about other abstract methods?

---

_@MichaReiser reviewed on 2024-09-10 22:11_

---

_@augustelalande reviewed on 2024-09-10 22:12_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:584 on 2024-09-10 22:12_

Could be its own PR

---

_@augustelalande reviewed on 2024-09-10 22:13_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:631 on 2024-09-10 22:13_

yes lol I already fixed it

---

_Comment by @MichaReiser on 2024-09-10 22:19_

Hmm, there's an overlap with https://docs.astral.sh/ruff/rules/undocumented-param/. Not quiet sure how to resolve the overlap.

---

_@augustelalande reviewed on 2024-09-10 22:19_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:644 on 2024-09-10 22:19_

`*` and `**` can be applied to any parameter. The only downside to triming indiscriminately is that you might allow a docstring that uses a star on a parameter without a star, but that's not what the rule is trying to catch anyway.

---

_Comment by @augustelalande on 2024-09-10 22:23_

`UndocumentedParam` in its current implimentation is restricted to google-style docstring only. I would suggest deprecating that rule in favor of this one, since it makes more sense as part of the `pydoclint` package.

---

_Comment by @MichaReiser on 2024-09-10 22:24_

> UndocumentedParam in its current implimentation is restricted to google-style docstring only. I would suggest deprecating that rule in favor of this one, since it makes more sense as part of the pydoclint package.

That's fair. But the google style one is less strict than the new one. It doesn't require that you document the parameters of all functions. It only requires that the parameters match the function's parameters **if** there's an Arguments section. 



---

_@augustelalande reviewed on 2024-09-10 22:28_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/snapshots/ruff_linter__rules__pydoclint__tests__docstring-extraneous-parameter_DOC102_google.py.snap`:25 on 2024-09-10 22:28_

That is how I have done it previously for example DOC502. The main decision factor is that it's a bit awkward to get the range of the single entry.

---

_@augustelalande reviewed on 2024-09-10 22:33_

---

_Review comment by @augustelalande on `crates/ruff_linter/resources/test/fixtures/pydoclint/DOC101_google.py`:372 on 2024-09-10 22:33_

There is already a discussion to optionally ignore those type of functions (https://github.com/astral-sh/ruff/issues/12434#issuecomment-2305425941)

But I think it should be a seperate PR.

---

_@augustelalande reviewed on 2024-09-10 22:34_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/snapshots/ruff_linter__rules__pydoclint__tests__docstring-extraneous-parameter_DOC102_google.py.snap`:39 on 2024-09-10 22:34_

I have done it (line 198)

---

_Comment by @augustelalande on 2024-09-10 22:36_

We could add an option to only highlight violations when a section is present. I tried something similar #13302.

---

_Comment by @MichaReiser on 2024-09-20 07:15_

We plan to look into the conflict but it may take some time. What we could do to move `DOC102` forward is to move its implementation out of this PR and try to see if it is possible to share logic with D417:

https://github.com/astral-sh/ruff/blob/281e6d9791576aaf5c6eae0f13a06272fb8adc9c/crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs#L1784-L1846

---

_Comment by @charliermarsh on 2024-09-23 23:45_

I agree that the rule makes more sense here semantically (since it's about content rather than style)... though the `D` rules are a lot more popular so we'd lose something by moving the rule over. I find this one to be a really tough call (and it's making me desperate for re-categorized rulesets with a single coherent docstring section).

Anyway... I _think_ I'm comfortable adding this to `DOC` in preview, then eventually redirecting `D417` to it. I'd like to get @zanieb's input on this one though.


---

_Comment by @zanieb on 2024-09-24 13:47_

@charliermarsh that sounds okay to me

---

_Comment by @augustelalande on 2024-09-28 05:03_

Thanks guys. Is this ready for merge, or are there more changes requested?

---

_Review requested from @charliermarsh by @zanieb on 2024-09-30 15:46_

---

_Comment by @AllanChain on 2025-01-03 10:58_

This PR has been pending for some time. Just want to gently bump this thread to see if there are any updates or if there's anything blocking its progress. This rule would be very useful, so I'm eager to see it merged.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:60 on 2025-01-06 10:41_

There seems to be a typo here.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:681 on 2025-01-06 10:47_

Why do we not try to find the indentation similar to Google style docstring? Or, why do we need to _find_ the indentation in Google style docstring? Is it that Google style can contain blank lines while NumPy style cannot?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:1131 on 2025-01-06 10:48_

nit: can we add a small comment stating that this logic is to skip the `self` argument if it's present?

Also, do we need to do the same for `cls` variable if it's a class method?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pydoclint/snapshots/ruff_linter__rules__pydoclint__tests__docstring-extraneous-parameter_DOC102_google.py.snap`:25 on 2025-01-06 10:50_

Should we then only highlight the "Args:" / "Parameters:" section header? The diagnostic message contains all the parameters that needs to be either added or removed.

---

_@dhruvmanila reviewed on 2025-01-06 10:50_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-01-06 10:51_

---

_@augustelalande reviewed on 2025-01-06 16:44_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:681 on 2025-01-06 16:44_

We do it this way because the dashes in the numpy style docs directly give us the indentation, and the args should use the same indent. There is no dashes in google style so we have to infer it from the args.

I don't really understand your follow up question.

---

_@augustelalande reviewed on 2025-01-06 16:53_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:1131 on 2025-01-06 16:53_

Added comment.

The logic already works for classmethods, I added a test to show it.

---

_@augustelalande reviewed on 2025-01-06 17:10_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/snapshots/ruff_linter__rules__pydoclint__tests__docstring-extraneous-parameter_DOC102_google.py.snap`:25 on 2025-01-06 17:10_

Done

---

_@dhruvmanila reviewed on 2025-01-08 05:05_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:681 on 2025-01-08 05:05_

What I mean is why don't we use the first line to determine the indentation in Google style docstring similar to what we're doing here for NumPy style docstring. This is just a minor nit so feel free to ignore.

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:681 on 2025-01-08 22:53_

Oh ya makes sense. I implemented the suggestion.

---

_@augustelalande reviewed on 2025-01-08 22:53_

---

_Comment by @augustelalande on 2025-02-06 15:20_

Is there anymore feedback here?

---

_Comment by @mschoettle on 2025-03-18 20:28_

Would love to have these extra rules!

---

_Comment by @dobeerman-sts on 2025-08-22 07:07_

Still in progress?

---

_Comment by @augustelalande on 2025-08-23 19:58_

I am waiting for feedback from the astral team

---

_Comment by @dobeerman-sts on 2025-08-24 14:24_

> I am waiting for feedback from the astral team

@augustelalande meantime, would you mind to fix merge conflicts? ;) 

---

_Comment by @Samoed on 2025-08-25 06:49_

Maybe tag maintainer for review?

---

_Review requested from @dhruvmanila by @augustelalande on 2025-08-25 15:15_

---

_Comment by @ntBre on 2025-09-11 13:08_

`DOC102` came up today in https://github.com/astral-sh/ruff/issues/20346, so I was looking at this PR again. I think I agree with @MichaReiser  [here](https://github.com/astral-sh/ruff/pull/13280#issuecomment-2362997178) that it could make sense to separate the two rules into separate PRs. I don't think we have any overlapping rules with `DOC102`, so it may be easier to get that part landed on its own.

Micha will be back next week and can weigh in then, but the conflict between `DOC101` and `D417` seems like the main blocker to me from my read of the thread here.

---

_Comment by @augustelalande on 2025-09-12 04:14_

@ntBre I thought the blocker was resolved by @charliermarsh's [comment](https://github.com/astral-sh/ruff/pull/13280#issuecomment-2369789351)

---

_Comment by @ntBre on 2025-09-12 13:07_

I think that's the long-term solution, but it's been almost a year, so we'd need to double check that everyone is still on board with that plan.  If we have two heavily overlapping rules, we need to think about a conflict warning, a deprecation plan, etc. That only affects `DOC101`, so I thought splitting the two up would be an easy way to defer that decision and have a shorter path to `DOC102`.

It would also make it easier for reviewers coming back to this (or a potential new reviewer like me) to look at two PRs half this size.

We can wait and see what Micha thinks, though. He might have a better idea :)

---
