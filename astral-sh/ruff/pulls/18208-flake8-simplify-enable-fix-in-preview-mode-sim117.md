```yaml
number: 18208
title: "[`flake8-simplify`] enable fix in preview mode (`SIM117`)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-safety-ast-with
created_at: 2025-05-19T21:16:17Z
updated_at: 2025-05-20T13:34:51Z
url: https://github.com/astral-sh/ruff/pull/18208
synced_at: 2026-01-10T18:51:02Z
```

# [`flake8-simplify`] enable fix in preview mode (`SIM117`)

---

_Pull request opened by @VascoSch92 on 2025-05-19 21:16_

The PR add the `fix safety` section for rule `SIM117` (#15584 ), and enable a fix in preview mode.

@dylwil3 Is the first time I'm exposed to the preview mode. So please let me know what should I do/fix. I hope I got what you wanted to say.



---

_Comment by @github-actions[bot] on 2025-05-19 21:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +356 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +270 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/src/airflow/cli/commands/rotate_fernet_key_command.py#L36'>airflow-core/src/airflow/cli/commands/rotate_fernet_key_command.py:36:5:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/src/airflow/cli/commands/rotate_fernet_key_command.py#L36'>airflow-core/src/airflow/cli/commands/rotate_fernet_key_command.py:36:5:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/always/test_secrets_local_filesystem.py#L380'>airflow-core/tests/unit/always/test_secrets_local_filesystem.py:380:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/always/test_secrets_local_filesystem.py#L380'>airflow-core/tests/unit/always/test_secrets_local_filesystem.py:380:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/always/test_secrets_local_filesystem.py#L392'>airflow-core/tests/unit/always/test_secrets_local_filesystem.py:392:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/always/test_secrets_local_filesystem.py#L392'>airflow-core/tests/unit/always/test_secrets_local_filesystem.py:392:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/always/test_secrets_local_filesystem.py#L406'>airflow-core/tests/unit/always/test_secrets_local_filesystem.py:406:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/always/test_secrets_local_filesystem.py#L406'>airflow-core/tests/unit/always/test_secrets_local_filesystem.py:406:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/api/common/test_mark_tasks.py#L38'>airflow-core/tests/unit/api/common/test_mark_tasks.py:38:5:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/api/common/test_mark_tasks.py#L38'>airflow-core/tests/unit/api/common/test_mark_tasks.py:38:5:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/api/common/test_mark_tasks.py#L63'>airflow-core/tests/unit/api/common/test_mark_tasks.py:63:5:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/api/common/test_mark_tasks.py#L63'>airflow-core/tests/unit/api/common/test_mark_tasks.py:63:5:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/api/common/test_mark_tasks.py#L93'>airflow-core/tests/unit/api/common/test_mark_tasks.py:93:5:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/api/common/test_mark_tasks.py#L93'>airflow-core/tests/unit/api/common/test_mark_tasks.py:93:5:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1669'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1669:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1669'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1669:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/cli/commands/test_dag_command.py#L378'>airflow-core/tests/unit/cli/commands/test_dag_command.py:378:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/cli/commands/test_dag_command.py#L378'>airflow-core/tests/unit/cli/commands/test_dag_command.py:378:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/core/test_configuration.py#L1161'>airflow-core/tests/unit/core/test_configuration.py:1161:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/core/test_configuration.py#L1161'>airflow-core/tests/unit/core/test_configuration.py:1161:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/core/test_configuration.py#L1206'>airflow-core/tests/unit/core/test_configuration.py:1206:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/core/test_configuration.py#L1206'>airflow-core/tests/unit/core/test_configuration.py:1206:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/core/test_configuration.py#L1579'>airflow-core/tests/unit/core/test_configuration.py:1579:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/core/test_configuration.py#L1579'>airflow-core/tests/unit/core/test_configuration.py:1579:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/core/test_configuration.py#L1775'>airflow-core/tests/unit/core/test_configuration.py:1775:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/core/test_configuration.py#L1775'>airflow-core/tests/unit/core/test_configuration.py:1775:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/core/test_configuration.py#L488'>airflow-core/tests/unit/core/test_configuration.py:488:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/core/test_configuration.py#L488'>airflow-core/tests/unit/core/test_configuration.py:488:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/core/test_configuration.py#L493'>airflow-core/tests/unit/core/test_configuration.py:493:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/core/test_configuration.py#L493'>airflow-core/tests/unit/core/test_configuration.py:493:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/core/test_configuration.py#L695'>airflow-core/tests/unit/core/test_configuration.py:695:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/core/test_configuration.py#L695'>airflow-core/tests/unit/core/test_configuration.py:695:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/core/test_logging_config.py#L198'>airflow-core/tests/unit/core/test_logging_config.py:198:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/core/test_logging_config.py#L198'>airflow-core/tests/unit/core/test_logging_config.py:198:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/core/test_settings.py#L223'>airflow-core/tests/unit/core/test_settings.py:223:5:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/core/test_settings.py#L223'>airflow-core/tests/unit/core/test_settings.py:223:5:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/airflow/blob/bcb42e29c2d34e0e332deb2ae7af89208b1dc076/airflow-core/tests/unit/core/test_sqlalchemy_config.py#L111'>airflow-core/tests/unit/core/test_sqlalchemy_config.py:111:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
... 233 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +40 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/39b3de6b5dea3746c9838a46e982f2097fa8187a/superset/db_engine_specs/gsheets.py#L376'>superset/db_engine_specs/gsheets.py:376:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/superset/blob/39b3de6b5dea3746c9838a46e982f2097fa8187a/superset/db_engine_specs/gsheets.py#L376'>superset/db_engine_specs/gsheets.py:376:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/superset/blob/39b3de6b5dea3746c9838a46e982f2097fa8187a/superset/models/core.py#L567'>superset/models/core.py:567:9:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/superset/blob/39b3de6b5dea3746c9838a46e982f2097fa8187a/superset/models/core.py#L567'>superset/models/core.py:567:9:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/superset/blob/39b3de6b5dea3746c9838a46e982f2097fa8187a/superset/sql_lab.py#L706'>superset/sql_lab.py:706:5:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/superset/blob/39b3de6b5dea3746c9838a46e982f2097fa8187a/superset/sql_lab.py#L706'>superset/sql_lab.py:706:5:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/superset/blob/39b3de6b5dea3746c9838a46e982f2097fa8187a/tests/conftest.py#L72'>tests/conftest.py:72:5:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/superset/blob/39b3de6b5dea3746c9838a46e982f2097fa8187a/tests/conftest.py#L72'>tests/conftest.py:72:5:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
- <a href='https://github.com/apache/superset/blob/39b3de6b5dea3746c9838a46e982f2097fa8187a/tests/integration_tests/dashboards/commands_tests.py#L776'>tests/integration_tests/dashboards/commands_tests.py:776:13:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/apache/superset/blob/39b3de6b5dea3746c9838a46e982f2097fa8187a/tests/integration_tests/dashboards/commands_tests.py#L776'>tests/integration_tests/dashboards/commands_tests.py:776:13:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
... 30 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +44 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
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
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/8bff275a53e521f49db4a136109ea053c397febf/src/latch_cli/services/login.py#L38'>src/latch_cli/services/login.py:38:5:</a> SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
+ <a href='https://github.com/latchbio/latch/blob/8bff275a53e521f49db4a136109ea053c397febf/src/latch_cli/services/login.py#L38'>src/latch_cli/services/login.py:38:5:</a> SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM117 | 356 | 0 | 0 | 356 | 0 |

</p>
</details>




---

_Label `rule` added by @ntBre on 2025-05-19 21:38_

---

_Label `preview` added by @ntBre on 2025-05-19 21:38_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/preview.rs`:130 on 2025-05-19 22:04_

While you're here can you add `_enabled` to the end of this function name too? I forgot to put it in there 😄 

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/preview.rs`:134 on 2025-05-19 22:04_

Can you put the word "safe" somewhere in this function name?

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_with.rs`:51 on 2025-05-19 22:06_

```suggestion
/// # Fix safety
///
/// This fix is marked as always unsafe unless [preview] mode is enabled, in which case it is always
/// marked as safe. Note that the fix is unavailable if it would remove comments (in either case).
```

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_with.rs`:54 on 2025-05-19 22:10_

```suggestion
/// ## References
/// - [Python documentation: The `with` statement](https://docs.python.org/3/reference/compound_stmts.html#the-with-statement)
///
/// [preview]: https://docs.astral.sh/ruff/preview/
```

---

_@dylwil3 requested changes on 2025-05-19 22:11_

Thank you! This is great - could you add a test as well? You should be able to just copy the same test fixture path here: https://github.com/astral-sh/ruff/blob/a2c87c2bc18f26a212f73cc5b3d7b2a23cd2caf6/crates/ruff_linter/src/rules/flake8_simplify/mod.rs#L61

---

_Review requested from @dylwil3 by @VascoSch92 on 2025-05-20 06:10_

---

_Comment by @VascoSch92 on 2025-05-20 06:11_

@dylwil3 thanks for the guidance ;-) 

---

_@dylwil3 approved on 2025-05-20 13:34_

Thank you!!

---

_Merged by @dylwil3 on 2025-05-20 13:34_

---

_Closed by @dylwil3 on 2025-05-20 13:34_

---
