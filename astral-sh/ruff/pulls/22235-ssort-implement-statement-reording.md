```yaml
number: 22235
title: "[`ssort`] Implement statement reording"
type: pull_request
state: open
author: JackAshwell11
labels:
  - isort
assignees: []
base: main
head: jack/ssort
created_at: 2025-12-28T15:40:18Z
updated_at: 2026-01-01T20:41:50Z
url: https://github.com/astral-sh/ruff/pull/22235
synced_at: 2026-01-12T15:57:44Z
```

# [`ssort`] Implement statement reording

---

_@JackAshwell11_

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Closes #3946 and #2436

- Adds a new SS001 (unsorted statements) rule allowing a file to be automatically sorted based on the dependencies between its functions/methods/classes. This also comes with an extra linter option `narrative_order` allowing the statement ordering to be reversed meaning the calling functions are defined before the dependency functions.

Note that this PR intentionally avoids sorting statements inside classes such as the docstring/special attribute/lifecycle methods/etc which ssort implements (https://github.com/bwhmather/ssort?tab=readme-ov-file#output) as these fall under #2425 which needs further discussion.

## Test Plan

<!-- How was it tested? -->

- Multiple Python files have been added to test the diagnostic reporting of the two rules in both dependency and narrative ordering mode.
- Additional Rust unit tests have been added to test extra functionality not directly covered by the Python tests.

I've checked the `ruff-ecosystem` results and each change looks to be correct with this new rule (although there will be many rule violations).

---

_Label `isort` added by @MichaReiser on 2025-12-29 07:30_

---

_Review requested from @ntBre by @ntBre on 2025-12-29 14:19_

---

_Marked ready for review by @JackAshwell11 on 2025-12-29 17:35_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ssort/async_function.py`:1 on 2025-12-29 17:45_

Wow that's a lot of new test files! Thanks for being thorough here, but is there any chance we could condense this down a bit? I haven't taken a look at the implementation yet, but something we often do is create outer functions or classes to contain test cases. For example, could we test the same sorting behavior if this file were like:

```py
class C:
	def function():
	    return dependency()
	
	
	def dependency():
	    return True
	
	
	async def async_function():
	    return function()
```

and then another file could be:

```py
class D:
    # another test case
    ...
```

or do they really need to be separate files?

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/statements.rs`:1 on 2025-12-29 17:47_

I think we already have a file for statement analysis in `checkers/ast/analyze`:

https://github.com/astral-sh/ruff/blob/0584081dc87d2cb662bf7927a1de2cb9ba003a3e/crates/ruff_linter/src/checkers/ast/analyze/statement.rs#L1

Could this check go there instead? Or maybe `module.rs` in the same directory.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ssort/rules/organize_statements.rs`:61 on 2025-12-29 17:48_

```suggestion
#[violation_metadata(preview_since = "0.14.11")]
```

We don't use a `v` in our version numbers anymore.

---

_@ntBre reviewed on 2025-12-29 17:52_

Thanks! I need to do a much deeper review at some point, just a couple of passing thoughts while scrolling through to approve the workflows.

---

_Comment by @codspeed-hq[bot] on 2025-12-29 17:57_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/JackAshwell11%3Ajack%2Fssort?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22235 will **not alter performance**

<sub>Comparing <code>JackAshwell11:jack/ssort</code> (fb6b12a) with <code>main</code> (26230b1)</sub>



### Summary

`✅ 30` untouched  
`⏩ 22` skipped[^skipped]  




[^skipped]: 22 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/JackAshwell11%3Ajack%2Fssort?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @astral-sh-bot[bot] on 2025-12-29 18:06_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1296 -0 violations, +0 -0 fixes in 8 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+81 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/abc.py#L3'>disnake/abc.py:3:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/activity.py#L3'>disnake/activity.py:3:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/app_commands.py#L3'>disnake/app_commands.py:3:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/asset.py#L3'>disnake/asset.py:3:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/audit_logs.py#L3'>disnake/audit_logs.py:3:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/automod.py#L3'>disnake/automod.py:3:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/channel.py#L3'>disnake/channel.py:3:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/client.py#L3'>disnake/client.py:3:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/components.py#L3'>disnake/components.py:3:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/embeds.py#L3'>disnake/embeds.py:3:1:</a> SS001 [*] Statements are unsorted
... 71 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+579 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/hatch_build.py#L17'>airflow-core/hatch_build.py:17:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/api/common/mark_tasks.py#L18'>airflow-core/src/airflow/api/common/mark_tasks.py:18:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/api_fastapi/app.py#L17'>airflow-core/src/airflow/api_fastapi/app.py:17:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L18'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:18:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/api_fastapi/auth/managers/simple/simple_auth_manager.py#L18'>airflow-core/src/airflow/api_fastapi/auth/managers/simple/simple_auth_manager.py:18:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/api_fastapi/auth/tokens.py#L17'>airflow-core/src/airflow/api_fastapi/auth/tokens.py:17:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/api_fastapi/common/exceptions.py#L18'>airflow-core/src/airflow/api_fastapi/common/exceptions.py:18:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/api_fastapi/common/parameters.py#L18'>airflow-core/src/airflow/api_fastapi/common/parameters.py:18:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/api_fastapi/common/types.py#L17'>airflow-core/src/airflow/api_fastapi/common/types.py:17:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/api_fastapi/core_api/datamodels/hitl.py#L17'>airflow-core/src/airflow/api_fastapi/core_api/datamodels/hitl.py:17:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/api_fastapi/core_api/services/public/common.py#L18'>airflow-core/src/airflow/api_fastapi/core_api/services/public/common.py:18:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/api_fastapi/core_api/services/ui/calendar.py#L17'>airflow-core/src/airflow/api_fastapi/core_api/services/ui/calendar.py:17:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/api_fastapi/core_api/services/ui/grid.py#L18'>airflow-core/src/airflow/api_fastapi/core_api/services/ui/grid.py:18:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/api_fastapi/execution_api/app.py#L18'>airflow-core/src/airflow/api_fastapi/execution_api/app.py:18:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/api_fastapi/execution_api/routes/assets.py#L18'>airflow-core/src/airflow/api_fastapi/execution_api/routes/assets.py:18:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/api_fastapi/execution_api/routes/dag_runs.py#L18'>airflow-core/src/airflow/api_fastapi/execution_api/routes/dag_runs.py:18:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/api_fastapi/execution_api/routes/task_instances.py#L18'>airflow-core/src/airflow/api_fastapi/execution_api/routes/task_instances.py:18:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/cli/commands/api_server_command.py#L17'>airflow-core/src/airflow/cli/commands/api_server_command.py:17:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/cli/commands/cheat_sheet_command.py#L17'>airflow-core/src/airflow/cli/commands/cheat_sheet_command.py:17:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/cli/commands/connection_command.py#L17'>airflow-core/src/airflow/cli/commands/connection_command.py:17:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/cli/commands/dag_command.py#L18'>airflow-core/src/airflow/cli/commands/dag_command.py:18:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/cli/commands/pool_command.py#L18'>airflow-core/src/airflow/cli/commands/pool_command.py:18:1:</a> SS001 [*] Statements are unsorted
... 557 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+177 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/RELEASING/changelog.py#L16'>RELEASING/changelog.py:16:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/scripts/check-env.py#L19'>scripts/check-env.py:19:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset/app.py#L17'>superset/app.py:17:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset/async_events/async_query_manager.py#L17'>superset/async_events/async_query_manager.py:17:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset/charts/schemas.py#L18'>superset/charts/schemas.py:18:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset/cli/test_db.py#L18'>superset/cli/test_db.py:18:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset/cli/viz_migrations.py#L17'>superset/cli/viz_migrations.py:17:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset/commands/annotation_layer/annotation/create.py#L17'>superset/commands/annotation_layer/annotation/create.py:17:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset/commands/annotation_layer/annotation/delete.py#L17'>superset/commands/annotation_layer/annotation/delete.py:17:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset/commands/annotation_layer/annotation/update.py#L17'>superset/commands/annotation_layer/annotation/update.py:17:1:</a> SS001 [*] Statements are unsorted
... 167 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+94 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ examples/output/jupyter/push_notebook/Numba Image Example.ipynb:cell 2:1:1: SS001 [*] Statements are unsorted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/duffing_oscillator.py#L1'>examples/server/app/duffing_oscillator.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/auth.py#L1'>examples/server/app/server_auth/auth.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/stocks/main.py#L1'>examples/server/app/stocks/main.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/logger.py#L7'>release/logger.py:7:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/setup.py#L7'>setup.py:7:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L7'>src/bokeh/application/application.py:7:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L7'>src/bokeh/application/handlers/code.py:7:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L7'>src/bokeh/application/handlers/code_runner.py:7:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L7'>src/bokeh/client/connection.py:7:1:</a> SS001 [*] Statements are unsorted
... 84 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+119 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/cli/langchain_cli/namespaces/app.py#L1'>libs/cli/langchain_cli/namespaces/app.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/cli/langchain_cli/namespaces/migrate/generate/utils.py#L1'>libs/cli/langchain_cli/namespaces/migrate/generate/utils.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/cli/tests/unit_tests/migrate/cli_runner/folder.py#L1'>libs/cli/tests/unit_tests/migrate/cli_runner/folder.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/core/langchain_core/agents.py#L1'>libs/core/langchain_core/agents.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/core/langchain_core/callbacks/base.py#L1'>libs/core/langchain_core/callbacks/base.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/core/langchain_core/callbacks/file.py#L1'>libs/core/langchain_core/callbacks/file.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/core/langchain_core/callbacks/manager.py#L1'>libs/core/langchain_core/callbacks/manager.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/core/langchain_core/indexing/api.py#L1'>libs/core/langchain_core/indexing/api.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/core/langchain_core/language_models/_utils.py#L1'>libs/core/langchain_core/language_models/_utils.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/core/langchain_core/language_models/chat_models.py#L1'>libs/core/langchain_core/language_models/chat_models.py:1:1:</a> SS001 [*] Statements are unsorted
... 109 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+41 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/5b3aa272183504eae277ef61f72c162d72a290a5/reflex/app.py#L1'>reflex/app.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/reflex-dev/reflex/blob/5b3aa272183504eae277ef61f72c162d72a290a5/reflex/compiler/compiler.py#L1'>reflex/compiler/compiler.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/reflex-dev/reflex/blob/5b3aa272183504eae277ef61f72c162d72a290a5/reflex/compiler/templates.py#L1'>reflex/compiler/templates.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/reflex-dev/reflex/blob/5b3aa272183504eae277ef61f72c162d72a290a5/reflex/compiler/utils.py#L1'>reflex/compiler/utils.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/reflex-dev/reflex/blob/5b3aa272183504eae277ef61f72c162d72a290a5/reflex/components/component.py#L1'>reflex/components/component.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/reflex-dev/reflex/blob/5b3aa272183504eae277ef61f72c162d72a290a5/reflex/components/core/upload.py#L1'>reflex/components/core/upload.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/reflex-dev/reflex/blob/5b3aa272183504eae277ef61f72c162d72a290a5/reflex/components/datadisplay/shiki_code_block.py#L1'>reflex/components/datadisplay/shiki_code_block.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/reflex-dev/reflex/blob/5b3aa272183504eae277ef61f72c162d72a290a5/reflex/components/el/elements/forms.py#L1'>reflex/components/el/elements/forms.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/reflex-dev/reflex/blob/5b3aa272183504eae277ef61f72c162d72a290a5/reflex/components/markdown/markdown.py#L1'>reflex/components/markdown/markdown.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/reflex-dev/reflex/blob/5b3aa272183504eae277ef61f72c162d72a290a5/reflex/components/props.py#L1'>reflex/components/props.py:1:1:</a> SS001 [*] Statements are unsorted
... 31 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/302e0fde412a4545adcc81d75d6bb49ee583e81c/src/scikit_build_core/build/__main__.py#L1'>src/scikit_build_core/build/__main__.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/302e0fde412a4545adcc81d75d6bb49ee583e81c/src/scikit_build_core/build/_wheelfile.py#L1'>src/scikit_build_core/build/_wheelfile.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/302e0fde412a4545adcc81d75d6bb49ee583e81c/src/scikit_build_core/build/wheel.py#L1'>src/scikit_build_core/build/wheel.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/302e0fde412a4545adcc81d75d6bb49ee583e81c/src/scikit_build_core/builder/wheel_tag.py#L1'>src/scikit_build_core/builder/wheel_tag.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/302e0fde412a4545adcc81d75d6bb49ee583e81c/src/scikit_build_core/cmake.py#L1'>src/scikit_build_core/cmake.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/302e0fde412a4545adcc81d75d6bb49ee583e81c/src/scikit_build_core/format.py#L1'>src/scikit_build_core/format.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/302e0fde412a4545adcc81d75d6bb49ee583e81c/src/scikit_build_core/hatch/plugin.py#L3'>src/scikit_build_core/hatch/plugin.py:3:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/302e0fde412a4545adcc81d75d6bb49ee583e81c/src/scikit_build_core/resources/_editable_redirect.py#L1'>src/scikit_build_core/resources/_editable_redirect.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/302e0fde412a4545adcc81d75d6bb49ee583e81c/src/scikit_build_core/settings/auto_requires.py#L1'>src/scikit_build_core/settings/auto_requires.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/302e0fde412a4545adcc81d75d6bb49ee583e81c/src/scikit_build_core/setuptools/build_cmake.py#L1'>src/scikit_build_core/setuptools/build_cmake.py:1:1:</a> SS001 [*] Statements are unsorted
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+194 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/8c6467cafb972b7bcef63737df1c4cb785d991a2/analytics/lib/counts.py#L1'>analytics/lib/counts.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/zulip/zulip/blob/8c6467cafb972b7bcef63737df1c4cb785d991a2/analytics/management/commands/check_analytics_state.py#L1'>analytics/management/commands/check_analytics_state.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/zulip/zulip/blob/8c6467cafb972b7bcef63737df1c4cb785d991a2/analytics/management/commands/update_analytics_counts.py#L1'>analytics/management/commands/update_analytics_counts.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/zulip/zulip/blob/8c6467cafb972b7bcef63737df1c4cb785d991a2/analytics/views/stats.py#L1'>analytics/views/stats.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/zulip/zulip/blob/8c6467cafb972b7bcef63737df1c4cb785d991a2/confirmation/models.py#L3'>confirmation/models.py:3:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/zulip/zulip/blob/8c6467cafb972b7bcef63737df1c4cb785d991a2/corporate/lib/stripe.py#L1'>corporate/lib/stripe.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/zulip/zulip/blob/8c6467cafb972b7bcef63737df1c4cb785d991a2/corporate/models/plans.py#L1'>corporate/models/plans.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/zulip/zulip/blob/8c6467cafb972b7bcef63737df1c4cb785d991a2/corporate/models/stripe_state.py#L1'>corporate/models/stripe_state.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/zulip/zulip/blob/8c6467cafb972b7bcef63737df1c4cb785d991a2/corporate/tests/test_stripe.py#L1'>corporate/tests/test_stripe.py:1:1:</a> SS001 [*] Statements are unsorted
+ <a href='https://github.com/zulip/zulip/blob/8c6467cafb972b7bcef63737df1c4cb785d991a2/scripts/lib/puppet_cache.py#L1'>scripts/lib/puppet_cache.py:1:1:</a> SS001 [*] Statements are unsorted
... 184 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SS001 | 1296 | 1296 | 0 | 0 | 0 |

</p>
</details>





---

_@MichaReiser reviewed on 2025-12-30 08:22_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ssort/dependencies.rs`:70 on 2025-12-30 08:22_

Don't we end up visiting `attr.value` twice? Or did you want to return early when seeing an attribute expression?

---

_@MichaReiser reviewed on 2025-12-30 08:23_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ssort/dependencies.rs`:112 on 2025-12-30 08:23_

Does this visitor correctly handle scopes? What if two functions define the same variables but they're nested within each other? What about type vars?

---

_@JackAshwell11 reviewed on 2025-12-30 14:06_

---

_Review comment by @JackAshwell11 on `crates/ruff_linter/src/checkers/statements.rs`:1 on 2025-12-30 14:06_

Should be fixed in latest

---

_@JackAshwell11 reviewed on 2025-12-30 14:07_

---

_Review comment by @JackAshwell11 on `crates/ruff_linter/src/rules/ssort/rules/organize_statements.rs`:61 on 2025-12-30 14:07_

Should be fixed in latest

---

_Review comment by @JackAshwell11 on `crates/ruff_linter/src/rules/ssort/dependencies.rs`:70 on 2025-12-30 14:14_

This is unintentional, I'll fix it. Thanks 👍 

---

_@JackAshwell11 reviewed on 2025-12-30 14:14_

---

_@JackAshwell11 reviewed on 2025-12-30 14:52_

---

_Review comment by @JackAshwell11 on `crates/ruff_linter/src/rules/ssort/dependencies.rs`:112 on 2025-12-30 14:52_

From my understanding, `ssort` is intentionally scope-agnostic as it only tracks text references, so as long as the text reference matches another function, this should work.

If two functions define the same variables but they're nested then this shouldn't handle they would be considered a separate suite (although I'd argue that is a problem with your code as you shouldn't name two things with the same name).

I'm not sure if type variables will work as I don't think `visit_expr()` handles annotations, but I could be wrong

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ssort/dependencies.rs`:112 on 2025-12-30 15:27_

How does ssort ensure that type annotations don't break (specificially in project that don't use `from __future__ import annotations`)? 

You probably want to adjust your visitor so that it doesn't traverse into nested classes or functions.

---

_@MichaReiser reviewed on 2025-12-30 15:27_

---

_@JackAshwell11 reviewed on 2025-12-30 18:10_

---

_Review comment by @JackAshwell11 on `crates/ruff_linter/resources/test/fixtures/ssort/async_function.py`:1 on 2025-12-30 18:10_

Some of them could be compacted, but I'm hesitant to do this unless necessary as, since this rule rearranges files based on the function calls, I'm not sure how many side effects there will be if some of the tests are combined

---

_Comment by @ntBre on 2026-01-01 17:09_

Thank you again for all of your work here! It's going to take me some time to digest this and review, but I have it on my todo list for the month.

Happy new year!

---
