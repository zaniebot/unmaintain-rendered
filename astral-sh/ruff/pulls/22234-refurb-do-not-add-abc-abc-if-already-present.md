```yaml
number: 22234
title: "[`refurb`] Do not add `abc.ABC` if already present (`FURB180`)"
type: pull_request
state: open
author: akawd
labels:
  - fixes
assignees: []
base: main
head: issue/17162/do-not-add-abc-if-already-added
created_at: 2025-12-28T11:38:46Z
updated_at: 2026-01-13T20:08:46Z
url: https://github.com/astral-sh/ruff/pull/22234
synced_at: 2026-01-13T20:37:03Z
```

# [`refurb`] Do not add `abc.ABC` if already present (`FURB180`)

---

_@akawd_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

According to the `FURB180` rule, `abc.ABC` should be used instead of `metaclass=abc.ABCMeta`. This issue can be fixed automatically, but if the class being checked already contains `abc.ABC`, it would be added a second time.

Fixes: #17162

## Test Plan

I ran `cargo test refurb`.  
For checking the "fix" I used this python file:
<details>

<summary>Python file (input)</summary>

```python
import abc
from abc import abstractmethod, ABCMeta, ABC
from abc import ABC as ABCAnotherName

class ParentClass:
    ...

from abc import ABC, ABCMeta

class Foo(metaclass=ABCMeta):
    ...
    
class Foo2(abc.ABC, ParentClass, metaclass=ABCMeta):
    ...
    
class Foo3(ParentClass, ABC, metaclass=ABCMeta):
    ...
    
class Foo4(ABCAnotherName, metaclass=ABCMeta):
    ...
```

</details>

The output after applying the fix is so:
<details>
<summary>Python file (output)</summary>

```python
import abc
from abc import abstractmethod, ABCMeta, ABC
from abc import ABC as ABCAnotherName

class ParentClass:
    ...

from abc import ABC, ABCMeta

class Foo(ABC):
    ...
    
class Foo2(abc.ABC, ParentClass, ):
    ...
    
class Foo3(ParentClass, ABC, ):
    ...
    
class Foo4(ABCAnotherName, ):
    ...
```

</details>

Note: Trailing commas may remain after the automatic fix. It is unclear whether this is an issue.


---

_Review requested from @ntBre by @ntBre on 2025-12-29 14:18_

---

_Label `fixes` added by @ntBre on 2026-01-01 14:24_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:136 on 2026-01-01 14:26_

nit: is there an `Edit::range_deletion`? It looks like we could use that with `keyword.range()` here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:100 on 2026-01-01 14:27_

We have an [`any_qualified_base_class`](https://github.com/astral-sh/ruff/blob/b2b9d91859f2abc017e35d22f01696f141f6f5b1/crates/ruff_python_semantic/src/analyze/class.rs#L16) helper function that I think we could use here.

`is_has_abc` also reads a bit strangely to me, maybe just `has_abc` would be better.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:120 on 2026-01-01 14:29_

I think we should be able to use a slice here to avoid allocating vecs:


```suggestion
            let rest = if is_has_abc {
                &[]
            } else {
                &[
                    Edit::insertion(format!("{binding}, "), class_def.keywords()[0].start()),
                    import_edit,
                ]
            };
```

You might have to annotate `rest` as `&[Edit]`, though.

---

_@ntBre requested changes on 2026-01-01 14:34_

Thanks! This looks good to me overall, I just had a couple of small suggestions. We should also add your manual tests from the PR summary as regression tests. At the bottom of this file would be a good place for them:

https://github.com/astral-sh/ruff/blob/b2b9d91859f2abc017e35d22f01696f141f6f5b1/crates/ruff_linter/resources/test/fixtures/refurb/FURB180.py#L58

---

_Renamed from "[refurb] Do not add abc.ABC if already present" to "[`refurb`] Do not add `abc.ABC` if already present (`FURB180`)" by @ntBre on 2026-01-01 14:35_

---

_Comment by @astral-sh-bot[bot] on 2026-01-01 14:35_


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
- <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L1475'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py:1475:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L1475'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py:1475:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/tests/unit/serialization/test_dag_serialization.py#L682'>airflow-core/tests/unit/serialization/test_dag_serialization.py:682:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/tests/unit/serialization/test_dag_serialization.py#L682'>airflow-core/tests/unit/serialization/test_dag_serialization.py:682:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/devel-common/src/docs/utils/conf_constants.py#L208'>devel-common/src/docs/utils/conf_constants.py:208:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/devel-common/src/docs/utils/conf_constants.py#L208'>devel-common/src/docs/utils/conf_constants.py:208:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L136'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:136:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L136'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:136:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/providers/amazon/tests/unit/amazon/aws/executors/batch/test_batch_executor.py#L740'>providers/amazon/tests/unit/amazon/aws/executors/batch/test_batch_executor.py:740:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/providers/amazon/tests/unit/amazon/aws/executors/batch/test_batch_executor.py#L740'>providers/amazon/tests/unit/amazon/aws/executors/batch/test_batch_executor.py:740:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py#L1329'>providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py:1329:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py#L1329'>providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py:1329:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/providers/standard/tests/unit/standard/operators/test_hitl.py#L332'>providers/standard/tests/unit/standard/operators/test_hitl.py:332:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/providers/standard/tests/unit/standard/operators/test_hitl.py#L332'>providers/standard/tests/unit/standard/operators/test_hitl.py:332:17:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/scripts/ci/prek/check_shared_distributions_usage.py#L409'>scripts/ci/prek/check_shared_distributions_usage.py:409:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/scripts/ci/prek/check_shared_distributions_usage.py#L409'>scripts/ci/prek/check_shared_distributions_usage.py:409:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/RELEASING/changelog.py#L227'>RELEASING/changelog.py:227:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/RELEASING/changelog.py#L227'>RELEASING/changelog.py:227:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset/jinja_context.py#L546'>superset/jinja_context.py:546:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset/jinja_context.py#L546'>superset/jinja_context.py:546:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/tests/integration_tests/reports/api_tests.py#L301'>tests/integration_tests/reports/api_tests.py:301:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/tests/integration_tests/reports/api_tests.py#L301'>tests/integration_tests/reports/api_tests.py:301:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
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
ℹ️ ecosystem check **detected linter changes**. (+20 -2038 violations, +10 -0 fixes in 25 projects; 30 projects unchanged)

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
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+8 -397 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/__init__.py#L130'>airflow-core/src/airflow/__init__.py:130:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/__init__.py#L137'>airflow-core/src/airflow/__init__.py:137:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/__init__.py#L38'>airflow-core/src/airflow/__init__.py:38:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/__init__.py#L46'>airflow-core/src/airflow/__init__.py:46:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/__init__.py#L80'>airflow-core/src/airflow/__init__.py:80:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/src/airflow/__init__.py#L84'>airflow-core/src/airflow/__init__.py:84:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
... 384 additional changes omitted for rule RUF067
- <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L1475'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py:1475:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L1475'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py:1475:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/tests/unit/serialization/test_dag_serialization.py#L682'>airflow-core/tests/unit/serialization/test_dag_serialization.py:682:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/84110f44b73ac9ee8383d9066001e0ee17c9b5e8/airflow-core/tests/unit/serialization/test_dag_serialization.py#L682'>airflow-core/tests/unit/serialization/test_dag_serialization.py:682:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
... 401 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+3 -73 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/RELEASING/changelog.py#L227'>RELEASING/changelog.py:227:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/RELEASING/changelog.py#L227'>RELEASING/changelog.py:227:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset-core/src/superset_core/mcp/__init__.py#L100'>superset-core/src/superset_core/mcp/__init__.py:100:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset-core/src/superset_core/mcp/__init__.py#L41'>superset-core/src/superset_core/mcp/__init__.py:41:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset-core/src/superset_core/mcp/__init__.py#L44'>superset-core/src/superset_core/mcp/__init__.py:44:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset/__init__.py#L37'>superset/__init__.py:37:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset/__init__.py#L38'>superset/__init__.py:38:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset/__init__.py#L39'>superset/__init__.py:39:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
... 65 additional changes omitted for rule RUF067
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset/jinja_context.py#L546'>superset/jinja_context.py:546:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset/jinja_context.py#L546'>superset/jinja_context.py:546:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
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

<details><summary>Changes by rule (4 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF067 | 2017 | 0 | 2017 | 0 | 0 |
| PLC0206 | 40 | 20 | 20 | 0 | 0 |
| FURB192 | 10 | 0 | 0 | 10 | 0 |
| RUF100 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>





---

_@akawd reviewed on 2026-01-02 08:51_

---

_Review comment by @akawd on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:136 on 2026-01-02 08:51_

Changed to `Edit::range_deletion`.

---

_@akawd reviewed on 2026-01-02 08:52_

---

_Review comment by @akawd on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:100 on 2026-01-02 08:52_

Switched to using [any_qualified_base_class](https://github.com/astral-sh/ruff/blob/b2b9d91859f2abc017e35d22f01696f141f6f5b1/crates/ruff_python_semantic/src/analyze/class.rs#L16).

---

_@akawd reviewed on 2026-01-02 08:54_

---

_Review comment by @akawd on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:120 on 2026-01-02 08:54_

Thank you for this advice, done.
P.S.: I had to use `iter().cloned()` because of https://github.com/akawd/ruff/blob/issue/17162/do-not-add-abc-if-already-added/crates/ruff_diagnostics/src/fix.rs#L130

---

_Comment by @akawd on 2026-01-02 09:00_

According to @"... add your manual tests from the PR summary as regression tests." - I added the following changes to `FURB180.py`:

```python

from abc import abstractmethod, ABCMeta, ABC
from abc import ABC as ABCAlternativeName

...
class A7(ABC):
    @abstractmethod
    def foo(self): pass

class A8(ABCAlternativeName):
    @abstractmethod
    def foo(self): pass  
...
```

I hope this is what was expected.

---

_Review requested from @ntBre by @akawd on 2026-01-02 09:06_

---

_@ntBre reviewed on 2026-01-02 14:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:120 on 2026-01-02 14:57_

Ah right, my mistake. Let's just go back to the vecs. I forgot the slices would change the type of the iterator.

---

_Comment by @ntBre on 2026-01-02 14:58_

Thank you! That was close to what I had in mind, but I think your tests in the summary were a bit more helpful than the ones added to FURB180.py. I just pushed two commits updating the tests. A couple of things I tried to fix:
- I avoided changing any lines earlier in the file, this is nice for review because it doesn't shift the line numbers in the existing snapshots
- I added `metaclass=ABCMeta` to the new test cases so that FURB180 triggers
- I added `class A11` showing off the parent class traversal that `any_qualified_base_class` gets us

After making these changes, it showed that there's an issue with the fix:

```diff
- class A11(MyMetaClass, metaclass=ABCMeta):  # FURB180
+ class A11(MyMetaClass, ):  # FURB180
```

We're leaving a trailing comma after removing the metaclass argument. I think we might want to try the [`remove_argument`](https://github.com/astral-sh/ruff/blob/3cbe1e613c2dfc4b536130e635cb714bc7c0b375/crates/ruff_linter/src/fix/edits.rs#L206) helper function here. Sorry, I should have recommended that sooner!

---

_@akawd reviewed on 2026-01-02 18:54_

---

_Review comment by @akawd on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:120 on 2026-01-02 18:54_

Anyway it was an interesting idea (but not for this particular case, looks like). Reverted.
https://github.com/astral-sh/ruff/pull/22234/commits/aa524c82a18e12d890726554031b65745019679b
P.S.: And sorry for the mess with tests, I did not think the line numbers are important.

---

_Converted to draft by @akawd on 2026-01-02 19:24_

---

_Comment by @akawd on 2026-01-02 19:26_

According to: "We're leaving a trailing comma after removing the metaclass argument".
Marked as draft to cover this issue.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:120 on 2026-01-02 20:02_

Thank you! And no worries, the line numbers aren't a huge deal, just a little nicer for review :) Otherwise the formatting you had would be much nicer in normal circumstances.

---

_@ntBre reviewed on 2026-01-02 20:02_

---

_Comment by @akawd on 2026-01-03 07:23_

> We're leaving a trailing comma after removing the metaclass argument. I think we might want to try the [`remove_argument`](https://github.com/astral-sh/ruff/blob/3cbe1e613c2dfc4b536130e635cb714bc7c0b375/crates/ruff_linter/src/fix/edits.rs#L206) helper function here. 

Fixed the issue above here: https://github.com/astral-sh/ruff/pull/22234/commits/740da9e2808d8fda3616f8467dd706cca42ff7c0

Example.

<details>

<summary>Before fix:</summary>

```python
import abc
from abc import abstractmethod, ABCMeta, ABC
from abc import ABC as ABCAnotherName

class ParentClass:
    ...

class Foo(metaclass=ABCMeta):
    ...
    
class Foo2(abc.ABC, ParentClass, metaclass=ABCMeta):
    ...
    
class Foo3(ParentClass, ABC, metaclass=ABCMeta):
    ...
    
class Foo4(ABCAnotherName, metaclass=ABCMeta):
    ...
```

</details>

<details>

<summary>After fix:</summary>

```python
import abc
from abc import abstractmethod, ABCMeta, ABC
from abc import ABC as ABCAnotherName

class ParentClass:
    ...

class Foo(ABCAnotherName):
    ...
    
class Foo2(abc.ABC, ParentClass):
    ...
    
class Foo3(ParentClass, ABC):
    ...
    
class Foo4(ABCAnotherName):
    ...
```

</details>

---

_Marked ready for review by @akawd on 2026-01-03 07:24_

---

_Comment by @akawd on 2026-01-03 07:27_

And since there are several commits here, do I need to squash them?

---

_Comment by @akawd on 2026-01-13 18:43_

Hello, @ntBre . Just a friendly reminder and request to take a look at this if possible.

---

_Comment by @ntBre on 2026-01-13 20:08_

Thank you! I was on PTO last week, but I'm back now :) I'll take a look soon.

> And since there are several commits here, do I need to squash them?

Don't worry about squashing, we'll squash-merge at the end, so it's easier to keep all the commits for review in the meantime.

---
