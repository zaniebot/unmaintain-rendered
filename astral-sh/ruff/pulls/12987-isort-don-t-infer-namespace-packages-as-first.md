```yaml
number: 12987
title: "isort: Don't infer namespace packages as first-party (only subpackages of namespace packages)"
type: pull_request
state: closed
author: AlexWaygood
labels: []
assignees: []
draft: true
base: main
head: alex/isort-namespace-pkgs
created_at: 2024-08-19T12:20:28Z
updated_at: 2025-04-01T12:22:27Z
url: https://github.com/astral-sh/ruff/pull/12987
synced_at: 2026-01-10T19:40:36Z
```

# isort: Don't infer namespace packages as first-party (only subpackages of namespace packages)

---

_Pull request opened by @AlexWaygood on 2024-08-19 12:20_

Fixes #12984 (will add docs/PR description/tests after I've seen the ecosystem report)

---

_Comment by @github-actions[bot] on 2024-08-19 12:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+18 -9 violations, +0 -0 fixes in 6 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+12 -7 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/scripts/cleanup.py#L14'>scripts/cleanup.py:14:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/_plugins/connection/util.py#L15'>src/snowflake/cli/_plugins/connection/util.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/_plugins/git/manager.py#L15'>src/snowflake/cli/_plugins/git/manager.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/_plugins/nativeapp/project_model.py#L15'>src/snowflake/cli/_plugins/nativeapp/project_model.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/_plugins/snowpark/common.py#L15'>src/snowflake/cli/_plugins/snowpark/common.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/_plugins/snowpark/manager.py#L15'>src/snowflake/cli/_plugins/snowpark/manager.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/_plugins/spcs/compute_pool/manager.py#L15'>src/snowflake/cli/_plugins/spcs/compute_pool/manager.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/_plugins/spcs/image_repository/manager.py#L15'>src/snowflake/cli/_plugins/spcs/image_repository/manager.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/_plugins/stage/diff.py#L15'>src/snowflake/cli/_plugins/stage/diff.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/_plugins/streamlit/manager.py#L15'>src/snowflake/cli/_plugins/streamlit/manager.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/api/cli_global_context.py#L15'>src/snowflake/cli/api/cli_global_context.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/api/sql_execution.py#L15'>src/snowflake/cli/api/sql_execution.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/test_external_plugins/snowpark_hello_single_command/src/snowflakecli/test_plugins/snowpark_hello/manager.py#L15'>test_external_plugins/snowpark_hello_single_command/src/snowflakecli/test_plugins/snowpark_hello/manager.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/tests_integration/notebook/test_notebooks.py#L15'>tests_integration/notebook/test_notebooks.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/tests_integration/spcs/testing_utils/compute_pool_utils.py#L15'>tests_integration/spcs/testing_utils/compute_pool_utils.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/tests_integration/spcs/testing_utils/spcs_services_utils.py#L15'>tests_integration/spcs/testing_utils/spcs_services_utils.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/tests_integration/test_config.py#L15'>tests_integration/test_config.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/tests_integration/test_stage.py#L15'>tests_integration/test_stage.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/tests_integration/testing_utils/sql_utils.py#L15'>tests_integration/testing_utils/sql_utils.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/2545918b2272202f5427a2dcf98e700709705b32/dev/perf/dags/perf_dag_1.py#L22'>dev/perf/dags/perf_dag_1.py:22:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/dbdccc593e1aeab149dbd6019709394ec1e6dc9c/journalist_gui/test_gui.py#L1'>journalist_gui/test_gui.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/65dd64f4e2beae704ac2f898a592870513b0b017/latch_cli/services/init/example_conda/__init__.py#L10'>latch_cli/services/init/example_conda/__init__.py:10:35:</a> F401 `latch.resources.tasks.small_task` imported but unused
- <a href='https://github.com/latchbio/latch/blob/65dd64f4e2beae704ac2f898a592870513b0b017/latch_cli/services/init/example_conda/__init__.py#L10'>latch_cli/services/init/example_conda/__init__.py:10:35:</a> F401 `latch.resources.tasks.small_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/65dd64f4e2beae704ac2f898a592870513b0b017/latch_cli/services/init/example_r/__init__.py#L10'>latch_cli/services/init/example_r/__init__.py:10:35:</a> F401 `latch.resources.tasks.small_task` imported but unused
- <a href='https://github.com/latchbio/latch/blob/65dd64f4e2beae704ac2f898a592870513b0b017/latch_cli/services/init/example_r/__init__.py#L10'>latch_cli/services/init/example_r/__init__.py:10:35:</a> F401 `latch.resources.tasks.small_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/cd90644d99a4465ac324633b0ae15400cb0a6489/tests/projects/utils.py#L50'>tests/projects/utils.py:50:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/33db65c112e5d76e89dd45024e272df214d2ea9c/testing/_py/test_local.py#L2'>testing/_py/test_local.py:2:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| I001 | 23 | 16 | 7 | 0 | 0 |
| F401 | 4 | 2 | 2 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+18 -9 violations, +0 -0 fixes in 6 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+12 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/scripts/cleanup.py#L14'>scripts/cleanup.py:14:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/_plugins/connection/util.py#L15'>src/snowflake/cli/_plugins/connection/util.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/_plugins/git/manager.py#L15'>src/snowflake/cli/_plugins/git/manager.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/_plugins/nativeapp/project_model.py#L15'>src/snowflake/cli/_plugins/nativeapp/project_model.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/_plugins/snowpark/common.py#L15'>src/snowflake/cli/_plugins/snowpark/common.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/_plugins/snowpark/manager.py#L15'>src/snowflake/cli/_plugins/snowpark/manager.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/_plugins/spcs/compute_pool/manager.py#L15'>src/snowflake/cli/_plugins/spcs/compute_pool/manager.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/_plugins/spcs/image_repository/manager.py#L15'>src/snowflake/cli/_plugins/spcs/image_repository/manager.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/_plugins/stage/diff.py#L15'>src/snowflake/cli/_plugins/stage/diff.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/_plugins/streamlit/manager.py#L15'>src/snowflake/cli/_plugins/streamlit/manager.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/api/cli_global_context.py#L15'>src/snowflake/cli/api/cli_global_context.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/src/snowflake/cli/api/sql_execution.py#L15'>src/snowflake/cli/api/sql_execution.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/test_external_plugins/snowpark_hello_single_command/src/snowflakecli/test_plugins/snowpark_hello/manager.py#L15'>test_external_plugins/snowpark_hello_single_command/src/snowflakecli/test_plugins/snowpark_hello/manager.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/tests_integration/notebook/test_notebooks.py#L15'>tests_integration/notebook/test_notebooks.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/tests_integration/spcs/testing_utils/compute_pool_utils.py#L15'>tests_integration/spcs/testing_utils/compute_pool_utils.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/tests_integration/spcs/testing_utils/spcs_services_utils.py#L15'>tests_integration/spcs/testing_utils/spcs_services_utils.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/tests_integration/test_config.py#L15'>tests_integration/test_config.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/tests_integration/test_stage.py#L15'>tests_integration/test_stage.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/903b2b16699ecd414f769de59f81fe961da8cf29/tests_integration/testing_utils/sql_utils.py#L15'>tests_integration/testing_utils/sql_utils.py:15:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/2545918b2272202f5427a2dcf98e700709705b32/dev/perf/dags/perf_dag_1.py#L22'>dev/perf/dags/perf_dag_1.py:22:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/dbdccc593e1aeab149dbd6019709394ec1e6dc9c/journalist_gui/test_gui.py#L1'>journalist_gui/test_gui.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/65dd64f4e2beae704ac2f898a592870513b0b017/latch_cli/services/init/example_conda/__init__.py#L10'>latch_cli/services/init/example_conda/__init__.py:10:35:</a> F401 [*] `latch.resources.tasks.small_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/65dd64f4e2beae704ac2f898a592870513b0b017/latch_cli/services/init/example_conda/__init__.py#L10'>latch_cli/services/init/example_conda/__init__.py:10:35:</a> F401 `latch.resources.tasks.small_task` imported but unused
- <a href='https://github.com/latchbio/latch/blob/65dd64f4e2beae704ac2f898a592870513b0b017/latch_cli/services/init/example_r/__init__.py#L10'>latch_cli/services/init/example_r/__init__.py:10:35:</a> F401 [*] `latch.resources.tasks.small_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/65dd64f4e2beae704ac2f898a592870513b0b017/latch_cli/services/init/example_r/__init__.py#L10'>latch_cli/services/init/example_r/__init__.py:10:35:</a> F401 `latch.resources.tasks.small_task` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/cd90644d99a4465ac324633b0ae15400cb0a6489/tests/projects/utils.py#L50'>tests/projects/utils.py:50:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/33db65c112e5d76e89dd45024e272df214d2ea9c/testing/_py/test_local.py#L2'>testing/_py/test_local.py:2:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| I001 | 23 | 16 | 7 | 0 | 0 |
| F401 | 4 | 2 | 2 | 0 | 0 |

</p>
</details>




---

_Comment by @codspeed-hq[bot] on 2024-08-20 09:39_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex/isort-namespace-pkgs)

### Merging #12987 will **degrade performances by 7.42%**

<sub>Comparing <code>alex/isort-namespace-pkgs</code> (352edbb) with <code>main</code> (0bd258a)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex/isort-namespace-pkgs)._

### Benchmarks breakdown

|     | Benchmark | `main` | `alex/isort-namespace-pkgs` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/globals.py]` | 726.2 µs | 784.4 µs | -7.42% |


---

_Comment by @smartYSC on 2024-11-06 10:16_

Hey, thank you for preparing this PR! I built and tested it locally and it solves the problem I discussed in https://github.com/astral-sh/ruff/discussions/12038. Any chance to get this merged?

---

_Comment by @AlexWaygood on 2024-11-06 18:08_

I'll try to find some time to work on this soon and hopefully fix the perf regression!

---

_Comment by @smartYSC on 2025-01-03 06:55_

Hi Alex, do you have any update on this?

Unfortunately I am not a Rust developer, otherwise I would be glad to help out!

---

_Comment by @smartYSC on 2025-02-26 10:04_

Gentle reminder. This issue is blocking us from using `ruff` in our company :-(

---

_Comment by @ollie-bell on 2025-02-26 10:17_

> Gentle reminder. This issue is blocking us from using `ruff` in our company :-(

can you not use `[tool.ruff.lint.isort.known-third-party]`?

---

_Comment by @smartYSC on 2025-02-26 10:49_

> can you not use `[tool.ruff.lint.isort.known-third-party]`?

As discussed in https://github.com/astral-sh/ruff/discussions/12038 this sadly does not give the right output.


---

_Comment by @smartYSC on 2025-02-26 12:21_

@ollie-bell to elaborate this a bit further: `ruff` fails to detect the namespace package correctly (no matter what config options you throw at it).

If I have a namespace package `foo` and the repo I lint only contains `foo.bar` it will never sort `foo.baz` correctly. It only looks at the first part before the . (`foo`) and decides too early what type of package it is (ignoring the fact, that `bar` can be found locally and `baz` cannot).

---

_Comment by @strubbly on 2025-02-27 09:11_

Jut to add that we are in the same boat.  We are trying to move to `ruff` but this issue prevents it since we can't get it to do the same as `isort` has always done for us.  So we can't use it - it would reformat all our files.

---

_Comment by @ntBre on 2025-02-27 15:32_

Thanks for the updates everyone! We've been talking about this internally and are planning to have someone look into this again soon. I'm hoping to have a look this week if @dylwil3  doesn't beat me to it!

---

_Assigned to @dylwil3 by @dylwil3 on 2025-03-01 16:28_

---

_Comment by @MichaReiser on 2025-04-01 12:21_

I'll close this now that #16565 is up and @dylwil3's commited to land it :)

---

_Closed by @MichaReiser on 2025-04-01 12:21_

---

_Branch deleted on 2025-04-01 12:22_

---
