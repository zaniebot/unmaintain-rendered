```yaml
number: 8742
title: Remove erroneous bad-dunder-name reference
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dunder
created_at: 2023-11-17T17:21:14Z
updated_at: 2023-11-17T17:33:33Z
url: https://github.com/astral-sh/ruff/pull/8742
synced_at: 2026-01-12T15:55:26Z
```

# Remove erroneous bad-dunder-name reference

---

_@charliermarsh_

Closes #8731.

---

_Label `bug` added by @charliermarsh on 2023-11-17 17:21_

---

_Merged by @charliermarsh on 2023-11-17 17:26_

---

_Closed by @charliermarsh on 2023-11-17 17:26_

---

_Branch deleted on 2023-11-17 17:26_

---

_Comment by @github-actions[bot] on 2023-11-17 17:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+147 -147 violations, +0 -0 fixes in 41 projects)

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
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/aaed909344b12aa4691a9e23ea9f9c98d641d853/airflow/auth/managers/fab/models/__init__.py#L81'>airflow/auth/managers/fab/models/__init__.py:81:9:</a> PLW3201 Bad or misspelled dunder method name `__neq__`
- <a href='https://github.com/apache/airflow/blob/aaed909344b12aa4691a9e23ea9f9c98d641d853/airflow/auth/managers/fab/models/__init__.py#L81'>airflow/auth/managers/fab/models/__init__.py:81:9:</a> PLW3201 Bad or misspelled dunder method name `__neq__`. (bad-dunder-name)
+ <a href='https://github.com/apache/airflow/blob/aaed909344b12aa4691a9e23ea9f9c98d641d853/airflow/providers/databricks/operators/databricks_repos.py#L105'>airflow/providers/databricks/operators/databricks_repos.py:105:9:</a> PLW3201 Bad or misspelled dunder method name `__detect_repo_provider__`
- <a href='https://github.com/apache/airflow/blob/aaed909344b12aa4691a9e23ea9f9c98d641d853/airflow/providers/databricks/operators/databricks_repos.py#L105'>airflow/providers/databricks/operators/databricks_repos.py:105:9:</a> PLW3201 Bad or misspelled dunder method name `__detect_repo_provider__`. (bad-dunder-name)
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
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/io/notebook.py#L129'>src/bokeh/io/notebook.py:129:9:</a> PLW3201 Bad or misspelled dunder method name `_repr_html_`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/io/notebook.py#L129'>src/bokeh/io/notebook.py:129:9:</a> PLW3201 Bad or misspelled dunder method name `_repr_html_`. (bad-dunder-name)
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/model/model.py#L595'>src/bokeh/model/model.py:595:9:</a> PLW3201 Bad or misspelled dunder method name `_repr_html_`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/model/model.py#L595'>src/bokeh/model/model.py:595:9:</a> PLW3201 Bad or misspelled dunder method name `_repr_html_`. (bad-dunder-name)
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/tests/unit/bokeh/core/test_serialization.py#L806'>tests/unit/bokeh/core/test_serialization.py:806:17:</a> PLW3201 Bad or misspelled dunder method name `__array__`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/tests/unit/bokeh/core/test_serialization.py#L806'>tests/unit/bokeh/core/test_serialization.py:806:17:</a> PLW3201 Bad or misspelled dunder method name `__array__`. (bad-dunder-name)
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
+ <a href='https://github.com/ibis-project/ibis/blob/a6318f1681628b0dba399041682a9e069271f48f/ibis/common/egraph.py#L496'>ibis/common/egraph.py:496:9:</a> PLW3201 Bad or misspelled dunder method name `__argnames__`
... 105 additional changes omitted for project
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
+ <a href='https://github.com/pandas-dev/pandas/blob/54f193a70815c01e26d6dd0616616d56a74699ce/pandas/core/arrays/categorical.py#L1629'>pandas/core/arrays/categorical.py:1629:9:</a> PLW3201 Bad or misspelled dunder method name `__array__`
... 107 additional changes omitted for project
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
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW3201 | 294 | 147 | 147 | 0 | 0 |

</p>
</details>




---
