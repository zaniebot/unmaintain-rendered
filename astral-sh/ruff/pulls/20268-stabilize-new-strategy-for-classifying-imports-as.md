```yaml
number: 20268
title: Stabilize new strategy for classifying imports as first party
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
  - breaking
assignees: []
merged: true
base: brent/0.13.0
head: dylan/stabilize-match-source
created_at: 2025-09-05T18:31:56Z
updated_at: 2025-09-08T17:32:38Z
url: https://github.com/astral-sh/ruff/pull/20268
synced_at: 2026-01-12T15:56:58Z
```

# Stabilize new strategy for classifying imports as first party

---

_@dylwil3_

This stabilizes the behavior introduced in #16565 which (roughly) tries to match an import like `import a.b.c` to an actual directory path `a/b/c` in order to label it as first-party, rather than simply looking for a directory `a`.

Mainly this affects the sorting of imports in the presence of namespace packages, but a few other rules are affected as well.


---

_Comment by @github-actions[bot] on 2025-09-05 18:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+22 -14 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+15 -14 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/scripts/cleanup.py#L14'>scripts/cleanup.py:14:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/src/snowflake/cli/_plugins/connection/util.py#L15'>src/snowflake/cli/_plugins/connection/util.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/src/snowflake/cli/_plugins/logs/manager.py#L1'>src/snowflake/cli/_plugins/logs/manager.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/src/snowflake/cli/_plugins/logs/utils.py#L1'>src/snowflake/cli/_plugins/logs/utils.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/src/snowflake/cli/_plugins/nativeapp/sf_sql_facade.py#L14'>src/snowflake/cli/_plugins/nativeapp/sf_sql_facade.py:14:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/src/snowflake/cli/_plugins/spcs/compute_pool/manager.py#L15'>src/snowflake/cli/_plugins/spcs/compute_pool/manager.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/src/snowflake/cli/_plugins/spcs/image_repository/manager.py#L15'>src/snowflake/cli/_plugins/spcs/image_repository/manager.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/src/snowflake/cli/_plugins/sql/manager.py#L15'>src/snowflake/cli/_plugins/sql/manager.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/src/snowflake/cli/_plugins/stage/diff.py#L15'>src/snowflake/cli/_plugins/stage/diff.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/src/snowflake/cli/_plugins/streamlit/manager.py#L15'>src/snowflake/cli/_plugins/streamlit/manager.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/src/snowflake/cli/api/cli_global_context.py#L15'>src/snowflake/cli/api/cli_global_context.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/src/snowflake/cli/api/connections.py#L15'>src/snowflake/cli/api/connections.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/src/snowflake/cli/api/entities/common.py#L1'>src/snowflake/cli/api/entities/common.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/src/snowflake/cli/api/output/types.py#L15'>src/snowflake/cli/api/output/types.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/src/snowflake/cli/api/sql_execution.py#L15'>src/snowflake/cli/api/sql_execution.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/test_external_plugins/snowpark_hello_single_command/src/snowflakecli/test_plugins/snowpark_hello/manager.py#L14'>test_external_plugins/snowpark_hello_single_command/src/snowflakecli/test_plugins/snowpark_hello/manager.py:14:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/tests/nativeapp/utils.py#L15'>tests/nativeapp/utils.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/tests/test_data/projects/glob_patterns/main.py#L1'>tests/test_data/projects/glob_patterns/main.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/tests/test_data/projects/glob_patterns/src/app.py#L1'>tests/test_data/projects/glob_patterns/src/app.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/tests/test_data/projects/glob_patterns_zip/main.py#L1'>tests/test_data/projects/glob_patterns_zip/main.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/tests/test_data/projects/glob_patterns_zip/src/app.py#L1'>tests/test_data/projects/glob_patterns_zip/src/app.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/tests/test_main.py#L17'>tests/test_main.py:17:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/tests_integration/nativeapp/test_debug_mode.py#L15'>tests_integration/nativeapp/test_debug_mode.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/tests_integration/nativeapp/test_telemetry_errors.py#L14'>tests_integration/nativeapp/test_telemetry_errors.py:14:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/tests_integration/notebook/test_notebooks.py#L15'>tests_integration/notebook/test_notebooks.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/tests_integration/test_config.py#L15'>tests_integration/test_config.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/tests_integration/test_data/projects/snowpark_artifact_repository/app.py#L1'>tests_integration/test_data/projects/snowpark_artifact_repository/app.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/tests_integration/testing_utils/sql_utils.py#L15'>tests_integration/testing_utils/sql_utils.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/feca5e6aae9e7a6c4f7f37645109fb4f50d9a99c/tests_integration/tests_using_container_services/spcs/testing_utils/image_repository_utils.py#L1'>tests_integration/tests_using_container_services/spcs/testing_utils/image_repository_utils.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/e72cfd7d9e8b73fb5bdad9c0a0bcc83fa0cad6a3/airflow-core/docs/conf.py#L21'>airflow-core/docs/conf.py:21:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/apache/airflow/blob/e72cfd7d9e8b73fb5bdad9c0a0bcc83fa0cad6a3/chart/docs/conf.py#L21'>chart/docs/conf.py:21:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/apache/airflow/blob/e72cfd7d9e8b73fb5bdad9c0a0bcc83fa0cad6a3/docker-stack-docs/conf.py#L21'>docker-stack-docs/conf.py:21:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/apache/airflow/blob/e72cfd7d9e8b73fb5bdad9c0a0bcc83fa0cad6a3/providers-summary-docs/conf.py#L21'>providers-summary-docs/conf.py:21:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/2982e13c91093875b13dcb0b4f9ac8c523ddbc71/journalist_gui/test_gui.py#L1'>journalist_gui/test_gui.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/4245726cd449a5dabe3d5c385b305c54733d23f2/tests/projects/utils.py#L50'>tests/projects/utils.py:50:5:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/45b35ebc26d82e9bd3890430376e47884cc20062/testing/_py/test_local.py#L2'>testing/_py/test_local.py:2:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| I001 | 36 | 22 | 14 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:97 on 2025-09-05 20:03_

Would it still be helpful to link to the FAQ in some of these rules, just not in a `## Preview` section?

---

_Review comment by @ntBre on `docs/faq.md`:313 on 2025-09-05 20:04_

```suggestion
Ruff will require that the full relative path `foo/bar` exists as a directory, or that `foo/bar.py` or `foo/bar.pyi` exist as files. Finally, for imports of the form `from foo import bar`, Ruff will only use `foo` when determining whether a module is first-party or third-party.
```

---

_@ntBre approved on 2025-09-05 20:07_

Thanks! I was slightly surprised to see a net increase in diagnostics, but I totally defer to your experience here. It looks like the same set from the original PR anyway.

---

_Comment by @dylwil3 on 2025-09-05 20:43_

> Thanks! I was slightly surprised to see a net increase in diagnostics, but I totally defer to your experience here. It looks like the same set from the original PR anyway.

Yeah these are expected, mostly because folks presumably sorted using the "old" method already.

---

_Merged by @dylwil3 on 2025-09-05 20:56_

---

_Closed by @dylwil3 on 2025-09-05 20:56_

---

_Branch deleted on 2025-09-05 20:56_

---

_Label `rule` added by @ntBre on 2025-09-08 17:32_

---

_Label `breaking` added by @ntBre on 2025-09-08 17:32_

---
