```yaml
number: 8727
title: "[`refurb`] Implement `math-constant` (`FURB152`)"
type: pull_request
state: merged
author: siiptuo
labels:
  - rule
assignees: []
merged: true
base: main
head: FURB152
created_at: 2023-11-16T21:33:11Z
updated_at: 2023-11-17T17:44:27Z
url: https://github.com/astral-sh/ruff/pull/8727
synced_at: 2026-01-12T15:55:26Z
```

# [`refurb`] Implement `math-constant` (`FURB152`)

---

_@siiptuo_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Implements [FURB152](https://github.com/dosisod/refurb/blob/master/docs/checks.md#furb152-use-math-constant) that checks for literals that are similar to constants in `math` module, for example:

```python
A = 3.141592 * r ** 2
```

Use instead:
```python
A = math.pi * r ** 2
```

Related to #1348.

## Test Plan

<!-- How was it tested? -->

Tested with few test cases.

---

_@zanieb reviewed on 2023-11-16 21:41_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/refurb/rules/math_constant.rs`:62 on 2023-11-16 21:41_

Why is this an unsafe fix?

---

_@zanieb approved on 2023-11-16 21:43_

Sweet! Thanks. Just the one question.

---

_Label `rule` added by @zanieb on 2023-11-16 21:43_

---

_Review comment by @siiptuo on `crates/ruff_linter/src/rules/refurb/rules/math_constant.rs`:62 on 2023-11-16 21:47_

Most likely it changes result of calculations. For example, `math.pi` is more precise than `3.14`.

---

_@siiptuo reviewed on 2023-11-16 21:47_

---

_Comment by @github-actions[bot] on 2023-11-16 21:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+159 -147 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2c85e39f415e9d96867dc317592582b685888ed9/disnake/enums.py#L163'>disnake/enums.py:163:9:</a> PLW3201 Bad or misspelled dunder method name `__members__`
- <a href='https://github.com/DisnakeDev/disnake/blob/2c85e39f415e9d96867dc317592582b685888ed9/disnake/enums.py#L163'>disnake/enums.py:163:9:</a> PLW3201 Bad or misspelled dunder method name `__members__`. (bad-dunder-name)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+13 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/aaed909344b12aa4691a9e23ea9f9c98d641d853/airflow/auth/managers/fab/models/__init__.py#L81'>airflow/auth/managers/fab/models/__init__.py:81:9:</a> PLW3201 Bad or misspelled dunder method name `__neq__`
- <a href='https://github.com/apache/airflow/blob/aaed909344b12aa4691a9e23ea9f9c98d641d853/airflow/auth/managers/fab/models/__init__.py#L81'>airflow/auth/managers/fab/models/__init__.py:81:9:</a> PLW3201 Bad or misspelled dunder method name `__neq__`. (bad-dunder-name)
+ <a href='https://github.com/apache/airflow/blob/aaed909344b12aa4691a9e23ea9f9c98d641d853/airflow/providers/databricks/operators/databricks_repos.py#L105'>airflow/providers/databricks/operators/databricks_repos.py:105:9:</a> PLW3201 Bad or misspelled dunder method name `__detect_repo_provider__`
- <a href='https://github.com/apache/airflow/blob/aaed909344b12aa4691a9e23ea9f9c98d641d853/airflow/providers/databricks/operators/databricks_repos.py#L105'>airflow/providers/databricks/operators/databricks_repos.py:105:9:</a> PLW3201 Bad or misspelled dunder method name `__detect_repo_provider__`. (bad-dunder-name)
+ <a href='https://github.com/apache/airflow/blob/aaed909344b12aa4691a9e23ea9f9c98d641d853/tests/core/test_otel_logger.py#L268'>tests/core/test_otel_logger.py:268:64:</a> FURB152 [*] Replace `3.14` with `math.pi`
+ <a href='https://github.com/apache/airflow/blob/aaed909344b12aa4691a9e23ea9f9c98d641d853/tests/core/test_otel_logger.py#L274'>tests/core/test_otel_logger.py:274:34:</a> FURB152 [*] Replace `3.14` with `math.pi`
+ <a href='https://github.com/apache/airflow/blob/aaed909344b12aa4691a9e23ea9f9c98d641d853/tests/core/test_otel_logger.py#L280'>tests/core/test_otel_logger.py:280:64:</a> FURB152 [*] Replace `3.14` with `math.pi`
+ <a href='https://github.com/apache/airflow/blob/aaed909344b12aa4691a9e23ea9f9c98d641d853/tests/core/test_otel_logger.py#L286'>tests/core/test_otel_logger.py:286:34:</a> FURB152 [*] Replace `3.14` with `math.pi`
+ <a href='https://github.com/apache/airflow/blob/aaed909344b12aa4691a9e23ea9f9c98d641d853/tests/core/test_otel_logger.py#L290'>tests/core/test_otel_logger.py:290:64:</a> FURB152 [*] Replace `3.14` with `math.pi`
+ <a href='https://github.com/apache/airflow/blob/aaed909344b12aa4691a9e23ea9f9c98d641d853/tests/core/test_otel_logger.py#L298'>tests/core/test_otel_logger.py:298:34:</a> FURB152 [*] Replace `3.14` with `math.pi`
... 6 additional changes omitted for rule FURB152
... 5 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/3a1b7f64e8d8fe526296326c6d90f5c3080bc577/samcli/commands/local/lib/debug_context.py#L30'>samcli/commands/local/lib/debug_context.py:30:9:</a> PLW3201 Bad or misspelled dunder method name `__nonzero__`
- <a href='https://github.com/aws/aws-sam-cli/blob/3a1b7f64e8d8fe526296326c6d90f5c3080bc577/samcli/commands/local/lib/debug_context.py#L30'>samcli/commands/local/lib/debug_context.py:30:9:</a> PLW3201 Bad or misspelled dunder method name `__nonzero__`. (bad-dunder-name)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/io/notebook.py#L129'>src/bokeh/io/notebook.py:129:9:</a> PLW3201 Bad or misspelled dunder method name `_repr_html_`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/io/notebook.py#L129'>src/bokeh/io/notebook.py:129:9:</a> PLW3201 Bad or misspelled dunder method name `_repr_html_`. (bad-dunder-name)
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/model/model.py#L595'>src/bokeh/model/model.py:595:9:</a> PLW3201 Bad or misspelled dunder method name `_repr_html_`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/model/model.py#L595'>src/bokeh/model/model.py:595:9:</a> PLW3201 Bad or misspelled dunder method name `_repr_html_`. (bad-dunder-name)
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/tests/unit/bokeh/core/test_properties.py#L224'>tests/unit/bokeh/core/test_properties.py:224:23:</a> FURB152 [*] Replace `3.14` with `math.pi`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/tests/unit/bokeh/core/test_serialization.py#L806'>tests/unit/bokeh/core/test_serialization.py:806:17:</a> PLW3201 Bad or misspelled dunder method name `__array__`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/tests/unit/bokeh/core/test_serialization.py#L806'>tests/unit/bokeh/core/test_serialization.py:806:17:</a> PLW3201 Bad or misspelled dunder method name `__array__`. (bad-dunder-name)
... 1 additional changes omitted for rule PLW3201
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+7 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/a6da4faf938cae1c7249e486a7769419b93dd136/securedrop/pretty_bad_protocol/_meta.py#L227'>securedrop/pretty_bad_protocol/_meta.py:227:9:</a> PLW3201 Bad or misspelled dunder method name `__remove_path__`
- <a href='https://github.com/freedomofpress/securedrop/blob/a6da4faf938cae1c7249e486a7769419b93dd136/securedrop/pretty_bad_protocol/_meta.py#L227'>securedrop/pretty_bad_protocol/_meta.py:227:9:</a> PLW3201 Bad or misspelled dunder method name `__remove_path__`. (bad-dunder-name)
+ <a href='https://github.com/freedomofpress/securedrop/blob/a6da4faf938cae1c7249e486a7769419b93dd136/securedrop/pretty_bad_protocol/_parsers.py#L1007'>securedrop/pretty_bad_protocol/_parsers.py:1007:9:</a> PLW3201 Bad or misspelled dunder method name `__nonzero__`
- <a href='https://github.com/freedomofpress/securedrop/blob/a6da4faf938cae1c7249e486a7769419b93dd136/securedrop/pretty_bad_protocol/_parsers.py#L1007'>securedrop/pretty_bad_protocol/_parsers.py:1007:9:</a> PLW3201 Bad or misspelled dunder method name `__nonzero__`. (bad-dunder-name)
+ <a href='https://github.com/freedomofpress/securedrop/blob/a6da4faf938cae1c7249e486a7769419b93dd136/securedrop/pretty_bad_protocol/_parsers.py#L1115'>securedrop/pretty_bad_protocol/_parsers.py:1115:9:</a> PLW3201 Bad or misspelled dunder method name `__nonzero__`
- <a href='https://github.com/freedomofpress/securedrop/blob/a6da4faf938cae1c7249e486a7769419b93dd136/securedrop/pretty_bad_protocol/_parsers.py#L1115'>securedrop/pretty_bad_protocol/_parsers.py:1115:9:</a> PLW3201 Bad or misspelled dunder method name `__nonzero__`. (bad-dunder-name)
+ <a href='https://github.com/freedomofpress/securedrop/blob/a6da4faf938cae1c7249e486a7769419b93dd136/securedrop/pretty_bad_protocol/_parsers.py#L1313'>securedrop/pretty_bad_protocol/_parsers.py:1313:9:</a> PLW3201 Bad or misspelled dunder method name `__nonzero__`
- <a href='https://github.com/freedomofpress/securedrop/blob/a6da4faf938cae1c7249e486a7769419b93dd136/securedrop/pretty_bad_protocol/_parsers.py#L1313'>securedrop/pretty_bad_protocol/_parsers.py:1313:9:</a> PLW3201 Bad or misspelled dunder method name `__nonzero__`. (bad-dunder-name)
+ <a href='https://github.com/freedomofpress/securedrop/blob/a6da4faf938cae1c7249e486a7769419b93dd136/securedrop/pretty_bad_protocol/_parsers.py#L1417'>securedrop/pretty_bad_protocol/_parsers.py:1417:9:</a> PLW3201 Bad or misspelled dunder method name `__nonzero__`
- <a href='https://github.com/freedomofpress/securedrop/blob/a6da4faf938cae1c7249e486a7769419b93dd136/securedrop/pretty_bad_protocol/_parsers.py#L1417'>securedrop/pretty_bad_protocol/_parsers.py:1417:9:</a> PLW3201 Bad or misspelled dunder method name `__nonzero__`. (bad-dunder-name)
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+63 -63 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/backends/base/__init__.py#L234'>ibis/backends/base/__init__.py:234:9:</a> PLW3201 Bad or misspelled dunder method name `_ipython_key_completions_`
- <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/backends/base/__init__.py#L234'>ibis/backends/base/__init__.py:234:9:</a> PLW3201 Bad or misspelled dunder method name `_ipython_key_completions_`. (bad-dunder-name)
+ <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/backends/base/__init__.py#L769'>ibis/backends/base/__init__.py:769:9:</a> PLW3201 Bad or misspelled dunder method name `__rich_repr__`
- <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/backends/base/__init__.py#L769'>ibis/backends/base/__init__.py:769:9:</a> PLW3201 Bad or misspelled dunder method name `__rich_repr__`. (bad-dunder-name)
+ <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/backends/base/sql/compiler/query_builder.py#L241'>ibis/backends/base/sql/compiler/query_builder.py:241:9:</a> PLW3201 Bad or misspelled dunder method name `__equals__`
- <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/backends/base/sql/compiler/query_builder.py#L241'>ibis/backends/base/sql/compiler/query_builder.py:241:9:</a> PLW3201 Bad or misspelled dunder method name `__equals__`. (bad-dunder-name)
+ <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/common/bases.py#L105'>ibis/common/bases.py:105:9:</a> PLW3201 Bad or misspelled dunder method name `__create__`
- <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/common/bases.py#L105'>ibis/common/bases.py:105:9:</a> PLW3201 Bad or misspelled dunder method name `__create__`. (bad-dunder-name)
+ <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/common/bases.py#L123'>ibis/common/bases.py:123:9:</a> PLW3201 Bad or misspelled dunder method name `__prohibit_inheritance__`
- <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/common/bases.py#L123'>ibis/common/bases.py:123:9:</a> PLW3201 Bad or misspelled dunder method name `__prohibit_inheritance__`. (bad-dunder-name)
+ <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/common/bases.py#L156'>ibis/common/bases.py:156:9:</a> PLW3201 Bad or misspelled dunder method name `__equals__`
- <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/common/bases.py#L156'>ibis/common/bases.py:156:9:</a> PLW3201 Bad or misspelled dunder method name `__equals__`. (bad-dunder-name)
+ <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/common/bases.py#L159'>ibis/common/bases.py:159:9:</a> PLW3201 Bad or misspelled dunder method name `__cached_equals__`
- <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/common/bases.py#L159'>ibis/common/bases.py:159:9:</a> PLW3201 Bad or misspelled dunder method name `__cached_equals__`. (bad-dunder-name)
+ <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/common/bases.py#L219'>ibis/common/bases.py:219:9:</a> PLW3201 Bad or misspelled dunder method name `__rich_repr__`
- <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/common/bases.py#L219'>ibis/common/bases.py:219:9:</a> PLW3201 Bad or misspelled dunder method name `__rich_repr__`. (bad-dunder-name)
+ <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/common/deferred.py#L236'>ibis/common/deferred.py:236:9:</a> PLW3201 Bad or misspelled dunder method name `__create__`
- <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/common/deferred.py#L236'>ibis/common/deferred.py:236:9:</a> PLW3201 Bad or misspelled dunder method name `__create__`. (bad-dunder-name)
+ <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/common/deferred.py#L48'>ibis/common/deferred.py:48:9:</a> PLW3201 Bad or misspelled dunder method name `__coerce__`
- <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/common/deferred.py#L48'>ibis/common/deferred.py:48:9:</a> PLW3201 Bad or misspelled dunder method name `__coerce__`. (bad-dunder-name)
... 106 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+64 -64 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/arrow/array.py#L593'>pandas/core/arrays/arrow/array.py:593:9:</a> PLW3201 Bad or misspelled dunder method name `__arrow_array__`
- <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/arrow/array.py#L593'>pandas/core/arrays/arrow/array.py:593:9:</a> PLW3201 Bad or misspelled dunder method name `__arrow_array__`. (bad-dunder-name)
+ <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/arrow/array.py#L597'>pandas/core/arrays/arrow/array.py:597:9:</a> PLW3201 Bad or misspelled dunder method name `__array__`
- <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/arrow/array.py#L597'>pandas/core/arrays/arrow/array.py:597:9:</a> PLW3201 Bad or misspelled dunder method name `__array__`. (bad-dunder-name)
+ <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/arrow/extension_types.py#L148'>pandas/core/arrays/arrow/extension_types.py:148:13:</a> PLW3201 Bad or misspelled dunder method name `__arrow_ext_serialize__`
- <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/arrow/extension_types.py#L148'>pandas/core/arrays/arrow/extension_types.py:148:13:</a> PLW3201 Bad or misspelled dunder method name `__arrow_ext_serialize__`. (bad-dunder-name)
+ <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/arrow/extension_types.py#L152'>pandas/core/arrays/arrow/extension_types.py:152:13:</a> PLW3201 Bad or misspelled dunder method name `__arrow_ext_deserialize__`
- <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/arrow/extension_types.py#L152'>pandas/core/arrays/arrow/extension_types.py:152:13:</a> PLW3201 Bad or misspelled dunder method name `__arrow_ext_deserialize__`. (bad-dunder-name)
+ <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/arrow/extension_types.py#L32'>pandas/core/arrays/arrow/extension_types.py:32:9:</a> PLW3201 Bad or misspelled dunder method name `__arrow_ext_serialize__`
- <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/arrow/extension_types.py#L32'>pandas/core/arrays/arrow/extension_types.py:32:9:</a> PLW3201 Bad or misspelled dunder method name `__arrow_ext_serialize__`. (bad-dunder-name)
+ <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/arrow/extension_types.py#L37'>pandas/core/arrays/arrow/extension_types.py:37:9:</a> PLW3201 Bad or misspelled dunder method name `__arrow_ext_deserialize__`
- <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/arrow/extension_types.py#L37'>pandas/core/arrays/arrow/extension_types.py:37:9:</a> PLW3201 Bad or misspelled dunder method name `__arrow_ext_deserialize__`. (bad-dunder-name)
+ <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/arrow/extension_types.py#L83'>pandas/core/arrays/arrow/extension_types.py:83:9:</a> PLW3201 Bad or misspelled dunder method name `__arrow_ext_serialize__`
- <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/arrow/extension_types.py#L83'>pandas/core/arrays/arrow/extension_types.py:83:9:</a> PLW3201 Bad or misspelled dunder method name `__arrow_ext_serialize__`. (bad-dunder-name)
+ <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/arrow/extension_types.py#L88'>pandas/core/arrays/arrow/extension_types.py:88:9:</a> PLW3201 Bad or misspelled dunder method name `__arrow_ext_deserialize__`
- <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/arrow/extension_types.py#L88'>pandas/core/arrays/arrow/extension_types.py:88:9:</a> PLW3201 Bad or misspelled dunder method name `__arrow_ext_deserialize__`. (bad-dunder-name)
+ <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/base.py#L2259'>pandas/core/arrays/base.py:2259:9:</a> PLW3201 Bad or misspelled dunder method name `__array_ufunc__`
- <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/base.py#L2259'>pandas/core/arrays/base.py:2259:9:</a> PLW3201 Bad or misspelled dunder method name `__array_ufunc__`. (bad-dunder-name)
+ <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/boolean.py#L102'>pandas/core/arrays/boolean.py:102:9:</a> PLW3201 Bad or misspelled dunder method name `__from_arrow__`
- <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/boolean.py#L102'>pandas/core/arrays/boolean.py:102:9:</a> PLW3201 Bad or misspelled dunder method name `__from_arrow__`. (bad-dunder-name)
... 108 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/sphinx-doc/sphinx/blob/35965903177c6ed9a6afb62ccd33243a746a3fc0/sphinx/ext/autodoc/mock.py#L52'>sphinx/ext/autodoc/mock.py:52:9:</a> PLW3201 Bad or misspelled dunder method name `__mro_entries__`
- <a href='https://github.com/sphinx-doc/sphinx/blob/35965903177c6ed9a6afb62ccd33243a746a3fc0/sphinx/ext/autodoc/mock.py#L52'>sphinx/ext/autodoc/mock.py:52:9:</a> PLW3201 Bad or misspelled dunder method name `__mro_entries__`. (bad-dunder-name)
+ <a href='https://github.com/sphinx-doc/sphinx/blob/35965903177c6ed9a6afb62ccd33243a746a3fc0/tests/test_ext_napoleon.py#L49'>tests/test_ext_napoleon.py:49:9:</a> PLW3201 Bad or misspelled dunder method name `__special_doc__`
- <a href='https://github.com/sphinx-doc/sphinx/blob/35965903177c6ed9a6afb62ccd33243a746a3fc0/tests/test_ext_napoleon.py#L49'>tests/test_ext_napoleon.py:49:9:</a> PLW3201 Bad or misspelled dunder method name `__special_doc__`. (bad-dunder-name)
+ <a href='https://github.com/sphinx-doc/sphinx/blob/35965903177c6ed9a6afb62ccd33243a746a3fc0/tests/test_ext_napoleon.py#L53'>tests/test_ext_napoleon.py:53:9:</a> PLW3201 Bad or misspelled dunder method name `__special_undoc__`
- <a href='https://github.com/sphinx-doc/sphinx/blob/35965903177c6ed9a6afb62ccd33243a746a3fc0/tests/test_ext_napoleon.py#L53'>tests/test_ext_napoleon.py:53:9:</a> PLW3201 Bad or misspelled dunder method name `__special_undoc__`. (bad-dunder-name)
+ <a href='https://github.com/sphinx-doc/sphinx/blob/35965903177c6ed9a6afb62ccd33243a746a3fc0/tests/test_ext_napoleon.py#L57'>tests/test_ext_napoleon.py:57:9:</a> PLW3201 Bad or misspelled dunder method name `__decorated_func__`
- <a href='https://github.com/sphinx-doc/sphinx/blob/35965903177c6ed9a6afb62ccd33243a746a3fc0/tests/test_ext_napoleon.py#L57'>tests/test_ext_napoleon.py:57:9:</a> PLW3201 Bad or misspelled dunder method name `__decorated_func__`. (bad-dunder-name)
+ <a href='https://github.com/sphinx-doc/sphinx/blob/35965903177c6ed9a6afb62ccd33243a746a3fc0/tests/test_ext_napoleon.py#L70'>tests/test_ext_napoleon.py:70:9:</a> PLW3201 Bad or misspelled dunder method name `__special_doc__`
- <a href='https://github.com/sphinx-doc/sphinx/blob/35965903177c6ed9a6afb62ccd33243a746a3fc0/tests/test_ext_napoleon.py#L70'>tests/test_ext_napoleon.py:70:9:</a> PLW3201 Bad or misspelled dunder method name `__special_doc__`. (bad-dunder-name)
... 2 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed projected reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW3201 | 294 | 147 | 147 | 0 | 0 |
| FURB152 | 12 | 12 | 0 | 0 | 0 |

</p>
</details>




---

_@zanieb reviewed on 2023-11-16 21:49_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/refurb/rules/math_constant.rs`:62 on 2023-11-16 21:49_

I'm unsure if that means we need to mark it as unsafe, I could go either way though. @charliermarsh?

If we do leave it as unsafe, we should explain why in the rule docs.

---

_@charliermarsh reviewed on 2023-11-16 22:22_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/refurb/rules/math_constant.rs`:62 on 2023-11-16 22:22_

Hah, this is an interesting case. I guess I'd lean towards unsafe since it _could_ break tests, etc., if you had hard-coded expectations that differed slightly?

---

_@zanieb reviewed on 2023-11-17 05:11_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/refurb/rules/math_constant.rs`:62 on 2023-11-17 05:11_

I guess but tests written to check floating point math already should include tolerances. Idk I'm struggling to think of a case where this would change runtime behavior in a meaningful way.

---

_@zanieb reviewed on 2023-11-17 14:12_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/refurb/rules/math_constant.rs`:62 on 2023-11-17 14:12_

Since it's in preview, it may be fine to just use safe and see if we get feedback.

---

_Renamed from "Implement FURB152" to "[`refurb`] Implement `math-constant` (`FURB152`)" by @charliermarsh on 2023-11-17 17:32_

---

_Merged by @charliermarsh on 2023-11-17 17:37_

---

_Closed by @charliermarsh on 2023-11-17 17:37_

---
