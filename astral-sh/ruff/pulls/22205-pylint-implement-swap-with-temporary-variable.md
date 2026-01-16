```yaml
number: 22205
title: "[`pylint`] Implement `swap-with-temporary-variable` (`PLR1712`)"
type: pull_request
state: open
author: Bnyro
labels:
  - rule
  - preview
assignees: []
base: main
head: temporary-variable-swap
created_at: 2025-12-26T11:15:04Z
updated_at: 2026-01-16T08:59:29Z
url: https://github.com/astral-sh/ruff/pull/22205
synced_at: 2026-01-16T09:55:09Z
```

# [`pylint`] Implement `swap-with-temporary-variable` (`PLR1712`)

---

_@Bnyro_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
This PR implements the `consider-swap-variables` rule from pylint. Basically it tries to find code parts that swap two variables with each other using a temporary variable.

Example code:
```py
temp = x
x = y
y = temp
```

can be simplified to

```py
x, y = y, x
```

related:
- https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/consider-swap-variables.html
- #970 

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
I've added new snapshots tests.

PS: Since this is my first contribution here and I'm not too familiar with the codebase, suggestions are very welcome! The implementation might also not be 100% memory-optimized yet, since we use `clone` a few times.


---

_Label `rule` added by @ntBre on 2025-12-29 13:53_

---

_Label `preview` added by @ntBre on 2025-12-29 13:53_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/consider_swap_variables.rs`:41 on 2025-12-29 13:56_

I think we might need a different name here based on our [naming convention](https://docs.astral.sh/ruff/contributing/#rule-naming-convention). Something like `UnnecessaryTemporaryVariable` would work since it sounds nice after "allow," but I'm definitely open to better suggestions.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/consider_swap_variables.rs`:40 on 2025-12-29 13:57_

Let's go ahead and put the next patch version here:


```suggestion
#[violation_metadata(preview_since = "0.14.11")]
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/consider_swap_variables.rs`:55 on 2025-12-29 13:58_

nit: You can implement `AlwaysFixableViolation` instead for this:


```suggestion
impl AlwaysFixableViolation for ConsiderSwapVariables {
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/consider_swap_variables.rs`:60 on 2025-12-29 14:01_

This is a pretty long message. I think I would move the part after the colon into the `fix_title`. Something like this:

```rust
#[derive_message_formats]
fn message(&self) -> String {
    "Consider swapping `{first_var}` and {`second_var}` with tuple unpacking".to_string()
}

fn fix_title(&self) -> String {
    "Use `{first_var}, {second_var} = {second_var}, {first_var}` instead".to_string()
}
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/consider_swap_variables.rs`:84 on 2025-12-29 14:02_

I think this would be a good use for [`tuple_windows`](https://docs.rs/itertools/latest/itertools/trait.Itertools.html#method.tuple_windows) from the itertools crate.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/consider_swap_variables.rs`:149 on 2025-12-29 14:05_

If `start` and `end` are always from the same statement, I think we can just use a single `TextRange` here and `stmt.range()` to retrieve it. Then you can use `Edit::range_replacement` above.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/consider_swap_variables.rs`:125 on 2025-12-29 14:09_

I haven't played with this myself, but we can almost always get away without cloning. Have you tried that here?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/consider_swap_variables.rs`:147 on 2025-12-29 14:09_

I think we can use `let-else` here to avoid `expect`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/consider_swap_variables.rs`:15 on 2025-12-29 14:11_

I don't think we need the quotes here either:


```suggestion
/// Variables can be swapped by using tuple unpacking instead of using a temporary variable. That also makes the intention of the swapping logic more clear.
```

and as a small nit, can you wrap this to 80-100 columns? Anywhere in that range is good :)

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/consider_swap_variables.rs`:93 on 2025-12-29 14:12_

I think we can avoid cloning here too, but you may need a lifetime on the violation struct.

---

_@ntBre requested changes on 2025-12-29 14:14_

Thank you! This looks good to me overall, just a few comments.

---

_Comment by @astral-sh-bot[bot] on 2025-12-29 14:20_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+20 -21 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+8 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L1475'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py:1475:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L1475'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py:1475:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/airflow-core/tests/unit/serialization/test_dag_serialization.py#L682'>airflow-core/tests/unit/serialization/test_dag_serialization.py:682:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/airflow-core/tests/unit/serialization/test_dag_serialization.py#L682'>airflow-core/tests/unit/serialization/test_dag_serialization.py:682:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/devel-common/src/docs/utils/conf_constants.py#L208'>devel-common/src/docs/utils/conf_constants.py:208:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/devel-common/src/docs/utils/conf_constants.py#L208'>devel-common/src/docs/utils/conf_constants.py:208:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L136'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:136:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L136'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:136:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/providers/amazon/tests/unit/amazon/aws/executors/batch/test_batch_executor.py#L740'>providers/amazon/tests/unit/amazon/aws/executors/batch/test_batch_executor.py:740:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/providers/amazon/tests/unit/amazon/aws/executors/batch/test_batch_executor.py#L740'>providers/amazon/tests/unit/amazon/aws/executors/batch/test_batch_executor.py:740:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py#L1329'>providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py:1329:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py#L1329'>providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py:1329:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/providers/standard/tests/unit/standard/operators/test_hitl.py#L332'>providers/standard/tests/unit/standard/operators/test_hitl.py:332:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/providers/standard/tests/unit/standard/operators/test_hitl.py#L332'>providers/standard/tests/unit/standard/operators/test_hitl.py:332:17:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/scripts/ci/prek/check_shared_distributions_usage.py#L409'>scripts/ci/prek/check_shared_distributions_usage.py:409:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/scripts/ci/prek/check_shared_distributions_usage.py#L409'>scripts/ci/prek/check_shared_distributions_usage.py:409:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/e5489bd30f590040631a8a1604091e3ca785f459/RELEASING/changelog.py#L227'>RELEASING/changelog.py:227:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/superset/blob/e5489bd30f590040631a8a1604091e3ca785f459/RELEASING/changelog.py#L227'>RELEASING/changelog.py:227:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/superset/blob/e5489bd30f590040631a8a1604091e3ca785f459/superset/jinja_context.py#L546'>superset/jinja_context.py:546:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/superset/blob/e5489bd30f590040631a8a1604091e3ca785f459/superset/jinja_context.py#L546'>superset/jinja_context.py:546:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/superset/blob/e5489bd30f590040631a8a1604091e3ca785f459/tests/integration_tests/reports/api_tests.py#L301'>tests/integration_tests/reports/api_tests.py:301:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/superset/blob/e5489bd30f590040631a8a1604091e3ca785f459/tests/integration_tests/reports/api_tests.py#L301'>tests/integration_tests/reports/api_tests.py:301:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/eb1da4888be5b628916d6a02f55ae3babb4dfd4c/tests/integration/pipeline/test_bootstrap_command.py#L229'>tests/integration/pipeline/test_bootstrap_command.py:229:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/aws/aws-sam-cli/blob/eb1da4888be5b628916d6a02f55ae3babb4dfd4c/tests/integration/pipeline/test_bootstrap_command.py#L229'>tests/integration/pipeline/test_bootstrap_command.py:229:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L201'>src/bokeh/application/handlers/code.py:201:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L201'>src/bokeh/application/handlers/code.py:201:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/wrappers.py#L470'>src/bokeh/core/property/wrappers.py:470:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/wrappers.py#L470'>src/bokeh/core/property/wrappers.py:470:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L183'>src/latch/functions/operators.py:183:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L183'>src/latch/functions/operators.py:183:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L37'>src/latch/functions/operators.py:37:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L37'>src/latch/functions/operators.py:37:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L48'>src/latch/functions/operators.py:48:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L48'>src/latch/functions/operators.py:48:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L59'>src/latch/functions/operators.py:59:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L59'>src/latch/functions/operators.py:59:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L68'>src/latch/functions/operators.py:68:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L68'>src/latch/functions/operators.py:68:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L73'>src/latch/functions/operators.py:73:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L73'>src/latch/functions/operators.py:73:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/scikit-build/scikit-build-core/blob/302e0fde412a4545adcc81d75d6bb49ee583e81c/src/scikit_build_core/_compat/typing.py#L36'>src/scikit_build_core/_compat/typing.py:36:32:</a> RUF100 [*] Unused `noqa` directive (unused: `ARG001`)
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC0206 | 40 | 20 | 20 | 0 | 0 |
| RUF100 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+20 -2041 violations, +0 -0 fixes in 25 projects; 30 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -28 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/__init__.py#L13'>disnake/__init__.py:13:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/__init__.py#L15'>disnake/__init__.py:15:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/__init__.py#L16'>disnake/__init__.py:16:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/__init__.py#L83'>disnake/__init__.py:83:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/ext/mypy_plugin/__init__.py#L11'>disnake/ext/mypy_plugin/__init__.py:11:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/ext/mypy_plugin/__init__.py#L7'>disnake/ext/mypy_plugin/__init__.py:7:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/ext/tasks/__init__.py#L38'>disnake/ext/tasks/__init__.py:38:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/ext/tasks/__init__.py#L39'>disnake/ext/tasks/__init__.py:39:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/ext/tasks/__init__.py#L40'>disnake/ext/tasks/__init__.py:40:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/ext/tasks/__init__.py#L41'>disnake/ext/tasks/__init__.py:41:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
... 18 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -12 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/__init__.py#L10'>rasa/__init__.py:10:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/cli/__init__.py#L5'>rasa/cli/__init__.py:5:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/__init__.py#L5'>rasa/core/__init__.py:5:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/channels/__init__.py#L28'>rasa/core/channels/__init__.py:28:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/channels/__init__.py#L47'>rasa/core/channels/__init__.py:47:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/training/__init__.py#L10'>rasa/core/training/__init__.py:10:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/training/__init__.py#L33'>rasa/core/training/__init__.py:33:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/__init__.py#L5'>rasa/nlu/__init__.py:5:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/classifiers/__init__.py#L3'>rasa/nlu/classifiers/__init__.py:3:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/utils/__init__.py#L11'>rasa/nlu/utils/__init__.py:11:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+8 -402 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/airflow-core/src/airflow/__init__.py#L130'>airflow-core/src/airflow/__init__.py:130:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/airflow-core/src/airflow/__init__.py#L137'>airflow-core/src/airflow/__init__.py:137:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/airflow-core/src/airflow/__init__.py#L38'>airflow-core/src/airflow/__init__.py:38:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/airflow-core/src/airflow/__init__.py#L46'>airflow-core/src/airflow/__init__.py:46:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/airflow-core/src/airflow/__init__.py#L80'>airflow-core/src/airflow/__init__.py:80:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/airflow-core/src/airflow/__init__.py#L84'>airflow-core/src/airflow/__init__.py:84:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
... 389 additional changes omitted for rule RUF067
- <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L1475'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py:1475:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L1475'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py:1475:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/airflow-core/tests/unit/serialization/test_dag_serialization.py#L682'>airflow-core/tests/unit/serialization/test_dag_serialization.py:682:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/9bdd6536e5d12f57aed81b80a4a1d0799784a667/airflow-core/tests/unit/serialization/test_dag_serialization.py#L682'>airflow-core/tests/unit/serialization/test_dag_serialization.py:682:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
... 400 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+3 -73 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/e5489bd30f590040631a8a1604091e3ca785f459/RELEASING/changelog.py#L227'>RELEASING/changelog.py:227:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/superset/blob/e5489bd30f590040631a8a1604091e3ca785f459/RELEASING/changelog.py#L227'>RELEASING/changelog.py:227:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/superset/blob/e5489bd30f590040631a8a1604091e3ca785f459/superset-core/src/superset_core/mcp/__init__.py#L100'>superset-core/src/superset_core/mcp/__init__.py:100:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/superset/blob/e5489bd30f590040631a8a1604091e3ca785f459/superset-core/src/superset_core/mcp/__init__.py#L41'>superset-core/src/superset_core/mcp/__init__.py:41:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/superset/blob/e5489bd30f590040631a8a1604091e3ca785f459/superset-core/src/superset_core/mcp/__init__.py#L44'>superset-core/src/superset_core/mcp/__init__.py:44:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/superset/blob/e5489bd30f590040631a8a1604091e3ca785f459/superset/__init__.py#L37'>superset/__init__.py:37:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/superset/blob/e5489bd30f590040631a8a1604091e3ca785f459/superset/__init__.py#L38'>superset/__init__.py:38:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/superset/blob/e5489bd30f590040631a8a1604091e3ca785f459/superset/__init__.py#L39'>superset/__init__.py:39:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
... 65 additional changes omitted for rule RUF067
+ <a href='https://github.com/apache/superset/blob/e5489bd30f590040631a8a1604091e3ca785f459/superset/jinja_context.py#L546'>superset/jinja_context.py:546:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/superset/blob/e5489bd30f590040631a8a1604091e3ca785f459/superset/jinja_context.py#L546'>superset/jinja_context.py:546:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
... 66 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/eb1da4888be5b628916d6a02f55ae3babb4dfd4c/tests/integration/pipeline/test_bootstrap_command.py#L229'>tests/integration/pipeline/test_bootstrap_command.py:229:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/aws/aws-sam-cli/blob/eb1da4888be5b628916d6a02f55ae3babb4dfd4c/tests/integration/pipeline/test_bootstrap_command.py#L229'>tests/integration/pipeline/test_bootstrap_command.py:229:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -58 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L101'>src/bokeh/__init__.py:101:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L102'>src/bokeh/__init__.py:102:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L107'>src/bokeh/__init__.py:107:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L109'>src/bokeh/__init__.py:109:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L110'>src/bokeh/__init__.py:110:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L111'>src/bokeh/__init__.py:111:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
... 51 additional changes omitted for rule RUF067
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L201'>src/bokeh/application/handlers/code.py:201:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L201'>src/bokeh/application/handlers/code.py:201:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/wrappers.py#L470'>src/bokeh/core/property/wrappers.py:470:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/wrappers.py#L470'>src/bokeh/core/property/wrappers.py:470:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
... 50 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+0 -142 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/__init__.py#L31'>ibis/__init__.py:31:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/__init__.py#L42'>ibis/__init__.py:42:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/backends/__init__.py#L1517'>ibis/backends/__init__.py:1517:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/backends/__init__.py#L1530'>ibis/backends/__init__.py:1530:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/backends/__init__.py#L1550'>ibis/backends/__init__.py:1550:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/backends/__init__.py#L1557'>ibis/backends/__init__.py:1557:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/backends/__init__.py#L1567'>ibis/backends/__init__.py:1567:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/backends/__init__.py#L1601'>ibis/backends/__init__.py:1601:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/backends/__init__.py#L1648'>ibis/backends/__init__.py:1648:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/backends/__init__.py#L1673'>ibis/backends/__init__.py:1673:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
... 132 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+0 -274 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/cli/tests/unit_tests/migrate/cli_runner/cases/__init__.py#L14'>libs/cli/tests/unit_tests/migrate/cli_runner/cases/__init__.py:14:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/cli/tests/unit_tests/migrate/cli_runner/cases/__init__.py#L15'>libs/cli/tests/unit_tests/migrate/cli_runner/cases/__init__.py:15:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/cli/tests/unit_tests/migrate/cli_runner/cases/__init__.py#L6'>libs/cli/tests/unit_tests/migrate/cli_runner/cases/__init__.py:6:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/core/langchain_core/__init__.py#L19'>libs/core/langchain_core/__init__.py:19:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/core/langchain_core/__init__.py#L20'>libs/core/langchain_core/__init__.py:20:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/core/langchain_core/_api/__init__.py#L45'>libs/core/langchain_core/_api/__init__.py:45:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/core/langchain_core/callbacks/__init__.py#L86'>libs/core/langchain_core/callbacks/__init__.py:86:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/core/langchain_core/document_loaders/__init__.py#L21'>libs/core/langchain_core/document_loaders/__init__.py:21:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/core/langchain_core/documents/__init__.py#L39'>libs/core/langchain_core/documents/__init__.py:39:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/langchain-ai/langchain/blob/a7aad60989581d80283ed2a13e19b41706daaec3/libs/core/langchain_core/embeddings/__init__.py#L16'>libs/core/langchain_core/embeddings/__init__.py:16:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
... 264 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+6 -58 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L183'>src/latch/functions/operators.py:183:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L183'>src/latch/functions/operators.py:183:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L37'>src/latch/functions/operators.py:37:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L37'>src/latch/functions/operators.py:37:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L48'>src/latch/functions/operators.py:48:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L48'>src/latch/functions/operators.py:48:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
... 7 additional changes omitted for rule PLC0206
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/docker_utils/__init__.py#L19'>src/latch_cli/docker_utils/__init__.py:19:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/docker_utils/__init__.py#L27'>src/latch_cli/docker_utils/__init__.py:27:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/docker_utils/__init__.py#L38'>src/latch_cli/docker_utils/__init__.py:38:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/docker_utils/__init__.py#L401'>src/latch_cli/docker_utils/__init__.py:401:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
... 54 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF067 | 2020 | 0 | 2020 | 0 | 0 |
| PLC0206 | 40 | 20 | 20 | 0 | 0 |
| RUF100 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>





---

_@Bnyro reviewed on 2025-12-29 16:23_

---

_Review comment by @Bnyro on `crates/ruff_linter/src/rules/pylint/rules/consider_swap_variables.rs`:84 on 2025-12-29 16:23_

I think that would only work well if we didn't have to make sure that all three statements `stmt_a`, `stmt_b`, and `stmt_c` are a variable to variable assignment (e.g. `x = y`). If we used something like

```rs
for (stmt_a, stmt_b, stmt_c) in stmts.tuple_windows() {
...
}
```
We would still need to check if all of them are such var-to-var assignments by calling `var_to_var_assignment` and checking if all the `Option`s (one for `stmt_a`, one for `stmt_b`, one for `stmt_c`) are all `Some`.

So we would probably need to repeat the same call to `var_to_var_assignment` three times, i.e.
```rs
let Some(stmt_a) = var_to_var_assignment(stmt_a) else { return; }
let Some(stmt_b) = var_to_var_assignment(stmt_b) else { return; }
let Some(stmt_c) = var_to_var_assignment(stmt_c) else { return; }
```

---

_@Bnyro reviewed on 2025-12-29 16:35_

---

_Review comment by @Bnyro on `crates/ruff_linter/src/rules/pylint/rules/consider_swap_variables.rs`:41 on 2025-12-29 16:35_

Hmm, I'm bad with naming too.

`UnnecessaryTemporaryVariable` feels a bit too generic in my opinion, that name is probably going to be better suited for other rules in the future that can spot more generally applicable scenarios/issues than temporary variable swapping.

My idea would be `SwapWithTemporaryVariable`, but I'm not really happy with it either, because it doesn't really say why that's a bad thing.

---

_Comment by @Bnyro on 2025-12-29 16:37_

Thanks a lot for the review @ntBre!

I've applied all suggestions except for the 2 unresolved ones above where I'm not quite sure yet, I'd appreciate your thoughts on that :)

---

_@ntBre reviewed on 2025-12-29 16:55_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/consider_swap_variables.rs`:84 on 2025-12-29 16:55_

Ohhh, I see, you're checking that there are three statements _and_ that they're all of the right type. I was really just trying to avoid collecting a temporary `Vec`, but that does make it a bit more awkward. Maybe something like this?

```rust
    for stmt_sequence in stmts.iter().map(var_to_var_assignment).tuple_windows() {
        let (Some(stmt_a), Some(stmt_b), Some(stmt_c)) = stmt_sequence else {
            continue;
        };
```


---

_@ntBre reviewed on 2025-12-29 17:03_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/consider_swap_variables.rs`:41 on 2025-12-29 17:03_

I like `SwapWithTemporaryVariable`, that preserves a bit more of the original name too! I think it's okay not to say why it's bad in the name.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/swap_with_temporary_variable.rs`:63 on 2026-01-02 17:37_

```suggestion
        format!("Consider swapping `{first_var}` and `{second_var}` by using tuple unpacking")
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/swap_with_temporary_variable.rs`:145 on 2026-01-02 17:42_

Could we avoid cloning here too? `VarToVarAssignment` may need a lifetime bound, but I think it should be doable.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pylint/swap_with_temporary_variable.py`:5 on 2026-01-02 17:47_

Could you test a case where the `temp` variable is used later? I just thought of that tricky case, and I think our fix would cause an error currently.

I think we could use something like this to check that there are no references after the swap:

https://github.com/astral-sh/ruff/blob/26230b1ed3db02f5c2d5b9ed9d49c70062d66736/crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs#L253-L266

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/swap_with_temporary_variable.rs`:54 on 2026-01-02 17:51_

A couple of nits on the naming here:

```suggestion
struct SwapAssignment {
    target: Name,
    value: Name,
    range: TextRange,
}
```

Or something else is fine too, we just typically try to avoid abbreviations like `var`. Can you put this below the main rule implementation just above your `var_to_var_assignment` function? That function could also be a `from_stmt` method on this struct, which would fix the `var` abbreviations in that name too.

---

_@ntBre reviewed on 2026-01-02 17:53_

Thank you again, this is looking great! 

I had a couple more nits and one larger concern about removing the temporary variable, but even that shouldn't be too much trouble, I hope.

---

_Renamed from "[`pylint`]: Implement `consider-swap-variables` (PLR1712)" to "[`pylint`] Implement `swap-with-temporary-variable` (`PLR1712`)" by @ntBre on 2026-01-02 17:53_

---

_Review comment by @Bnyro on `crates/ruff_linter/src/rules/pylint/rules/swap_with_temporary_variable.rs`:145 on 2026-01-03 11:41_

I tried it, but calling `target.name_expr()` consumes `target` and hence it doesn't seem to be possible to get away without `clone` here (or I just don't know how), because the result of `name_expr()` is owned by our current scope and hence may not escape outside of it

---

_@Bnyro reviewed on 2026-01-03 11:41_

---

_@Bnyro reviewed on 2026-01-03 12:52_

---

_Review comment by @Bnyro on `crates/ruff_linter/resources/test/fixtures/pylint/swap_with_temporary_variable.py`:5 on 2026-01-03 12:52_

You're right, very good catch!

I've implemented this now, but it's not yet working because it looks like `checker.semantic().bindings` does not contain any `Assignment`s, although that doesn't make any sense?! I've spent some time debugging this, but I've really no idea what's going on, i.e. why 
```rs
            let temporary_variable_binding = checker
                .semantic()
                .bindings
                .iter()
                .find(|binding| stmt_a.range.contains_range(binding.range))
                .unwrap();

```
doesn't find the binding definition and panics.

---

_Review requested from @ntBre by @MichaReiser on 2026-01-16 08:59_

---
