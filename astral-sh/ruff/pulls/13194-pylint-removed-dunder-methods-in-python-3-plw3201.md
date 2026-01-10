```yaml
number: 13194
title: "[pylint] removed dunder methods in Python 3 (PLW3201)"
type: pull_request
state: merged
author: vieira127
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: deprecated-dunder-method
created_at: 2024-09-01T14:42:07Z
updated_at: 2024-09-04T06:23:08Z
url: https://github.com/astral-sh/ruff/pull/13194
synced_at: 2026-01-10T21:38:32Z
```

# [pylint] removed dunder methods in Python 3 (PLW3201)

---

_Pull request opened by @vieira127 on 2024-09-01 14:42_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Add message for deprecated dunder methods not supported in Python 3

This PR addresses issue #12883 by adding custom messages in `PLW3201` for dunder methods that were deprecated in Python 2 and are no longer valid in Python 3. The message is displayed for the dunder methods listed in #12607 and additional  methods that are no longer part of Python 3 documentation:
- `__iop__`
- `__rop__`
- `__op__`
<!-- What's the purpose of the change? What does it do, and why? -->

fixes #12883

## Test Plan

<!-- How was it tested? -->
New snapshot added and `cargo test` 


---

_Comment by @github-actions[bot] on 2024-09-01 14:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+115 -115 violations, +0 -0 fixes in 9 projects; 45 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/DisnakeDev/disnake/blob/c7123d6e3d4c2caab9149e7870f7f37a10f6c924/disnake/enums.py#L168'>disnake/enums.py:168:9:</a> PLW3201 Bad or misspelled dunder method name `__members__`
+ <a href='https://github.com/DisnakeDev/disnake/blob/c7123d6e3d4c2caab9149e7870f7f37a10f6c924/disnake/enums.py#L168'>disnake/enums.py:168:9:</a> PLW3201 Dunder method `__members__` has no special meaning in Python 3
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/c3cabc130594d304aca7a7e1da8fd99f4d957b20/airflow/decorators/base.py#L390'>airflow/decorators/base.py:390:9:</a> PLW3201 Bad or misspelled dunder method name `__wrapped__`
+ <a href='https://github.com/apache/airflow/blob/c3cabc130594d304aca7a7e1da8fd99f4d957b20/airflow/decorators/base.py#L390'>airflow/decorators/base.py:390:9:</a> PLW3201 Dunder method `__wrapped__` has no special meaning in Python 3
- <a href='https://github.com/apache/airflow/blob/c3cabc130594d304aca7a7e1da8fd99f4d957b20/airflow/decorators/base.py#L594'>airflow/decorators/base.py:594:9:</a> PLW3201 Bad or misspelled dunder method name `__wrapped__`
+ <a href='https://github.com/apache/airflow/blob/c3cabc130594d304aca7a7e1da8fd99f4d957b20/airflow/decorators/base.py#L594'>airflow/decorators/base.py:594:9:</a> PLW3201 Dunder method `__wrapped__` has no special meaning in Python 3
- <a href='https://github.com/apache/airflow/blob/c3cabc130594d304aca7a7e1da8fd99f4d957b20/airflow/providers/databricks/operators/databricks_repos.py#L106'>airflow/providers/databricks/operators/databricks_repos.py:106:9:</a> PLW3201 Bad or misspelled dunder method name `__detect_repo_provider__`
+ <a href='https://github.com/apache/airflow/blob/c3cabc130594d304aca7a7e1da8fd99f4d957b20/airflow/providers/databricks/operators/databricks_repos.py#L106'>airflow/providers/databricks/operators/databricks_repos.py:106:9:</a> PLW3201 Dunder method `__detect_repo_provider__` has no special meaning in Python 3
- <a href='https://github.com/apache/airflow/blob/c3cabc130594d304aca7a7e1da8fd99f4d957b20/airflow/providers/fab/auth_manager/models/__init__.py#L94'>airflow/providers/fab/auth_manager/models/__init__.py:94:9:</a> PLW3201 Bad or misspelled dunder method name `__neq__`
+ <a href='https://github.com/apache/airflow/blob/c3cabc130594d304aca7a7e1da8fd99f4d957b20/airflow/providers/fab/auth_manager/models/__init__.py#L94'>airflow/providers/fab/auth_manager/models/__init__.py:94:9:</a> PLW3201 Dunder method `__neq__` has no special meaning in Python 3
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/72a520fba4c021e0e6eca5caffe562f8683884e4/superset/migrations/shared/security_converge.py#L68'>superset/migrations/shared/security_converge.py:68:9:</a> PLW3201 Bad or misspelled dunder method name `__neq__`
+ <a href='https://github.com/apache/superset/blob/72a520fba4c021e0e6eca5caffe562f8683884e4/superset/migrations/shared/security_converge.py#L68'>superset/migrations/shared/security_converge.py:68:9:</a> PLW3201 Dunder method `__neq__` has no special meaning in Python 3
- <a href='https://github.com/apache/superset/blob/72a520fba4c021e0e6eca5caffe562f8683884e4/superset/migrations/versions/2020-09-24_12-04_3fbbc6e8d654_fix_data_access_permissions_for_virtual_.py#L65'>superset/migrations/versions/2020-09-24_12-04_3fbbc6e8d654_fix_data_access_permissions_for_virtual_.py:65:9:</a> PLW3201 Bad or misspelled dunder method name `__neq__`
+ <a href='https://github.com/apache/superset/blob/72a520fba4c021e0e6eca5caffe562f8683884e4/superset/migrations/versions/2020-09-24_12-04_3fbbc6e8d654_fix_data_access_permissions_for_virtual_.py#L65'>superset/migrations/versions/2020-09-24_12-04_3fbbc6e8d654_fix_data_access_permissions_for_virtual_.py:65:9:</a> PLW3201 Dunder method `__neq__` has no special meaning in Python 3
- <a href='https://github.com/apache/superset/blob/72a520fba4c021e0e6eca5caffe562f8683884e4/tests/integration_tests/fixtures/pyodbcRow.py#L21'>tests/integration_tests/fixtures/pyodbcRow.py:21:9:</a> PLW3201 Bad or misspelled dunder method name `__name__`
+ <a href='https://github.com/apache/superset/blob/72a520fba4c021e0e6eca5caffe562f8683884e4/tests/integration_tests/fixtures/pyodbcRow.py#L21'>tests/integration_tests/fixtures/pyodbcRow.py:21:9:</a> PLW3201 Dunder method `__name__` has no special meaning in Python 3
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/7d0800acdf287c249b2ff8aa60bcf12865e0c2d5/samcli/commands/local/lib/debug_context.py#L30'>samcli/commands/local/lib/debug_context.py:30:9:</a> PLW3201 Bad or misspelled dunder method name `__nonzero__`
+ <a href='https://github.com/aws/aws-sam-cli/blob/7d0800acdf287c249b2ff8aa60bcf12865e0c2d5/samcli/commands/local/lib/debug_context.py#L30'>samcli/commands/local/lib/debug_context.py:30:9:</a> PLW3201 Dunder method `__nonzero__` has no special meaning in Python 3
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/notebook.py#L129'>src/bokeh/io/notebook.py:129:9:</a> PLW3201 Bad or misspelled dunder method name `_repr_html_`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/notebook.py#L129'>src/bokeh/io/notebook.py:129:9:</a> PLW3201 Dunder method `_repr_html_` has no special meaning in Python 3
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/model.py#L602'>src/bokeh/model/model.py:602:9:</a> PLW3201 Bad or misspelled dunder method name `_repr_html_`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/model.py#L602'>src/bokeh/model/model.py:602:9:</a> PLW3201 Dunder method `_repr_html_` has no special meaning in Python 3
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/test_serialization.py#L806'>tests/unit/bokeh/core/test_serialization.py:806:17:</a> PLW3201 Bad or misspelled dunder method name `__array__`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/test_serialization.py#L806'>tests/unit/bokeh/core/test_serialization.py:806:17:</a> PLW3201 Dunder method `__array__` has no special meaning in Python 3
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+7 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/dbdccc593e1aeab149dbd6019709394ec1e6dc9c/securedrop/pretty_bad_protocol/_meta.py#L227'>securedrop/pretty_bad_protocol/_meta.py:227:9:</a> PLW3201 Bad or misspelled dunder method name `__remove_path__`
+ <a href='https://github.com/freedomofpress/securedrop/blob/dbdccc593e1aeab149dbd6019709394ec1e6dc9c/securedrop/pretty_bad_protocol/_meta.py#L227'>securedrop/pretty_bad_protocol/_meta.py:227:9:</a> PLW3201 Dunder method `__remove_path__` has no special meaning in Python 3
- <a href='https://github.com/freedomofpress/securedrop/blob/dbdccc593e1aeab149dbd6019709394ec1e6dc9c/securedrop/pretty_bad_protocol/_parsers.py#L1007'>securedrop/pretty_bad_protocol/_parsers.py:1007:9:</a> PLW3201 Bad or misspelled dunder method name `__nonzero__`
+ <a href='https://github.com/freedomofpress/securedrop/blob/dbdccc593e1aeab149dbd6019709394ec1e6dc9c/securedrop/pretty_bad_protocol/_parsers.py#L1007'>securedrop/pretty_bad_protocol/_parsers.py:1007:9:</a> PLW3201 Dunder method `__nonzero__` has no special meaning in Python 3
- <a href='https://github.com/freedomofpress/securedrop/blob/dbdccc593e1aeab149dbd6019709394ec1e6dc9c/securedrop/pretty_bad_protocol/_parsers.py#L1115'>securedrop/pretty_bad_protocol/_parsers.py:1115:9:</a> PLW3201 Bad or misspelled dunder method name `__nonzero__`
+ <a href='https://github.com/freedomofpress/securedrop/blob/dbdccc593e1aeab149dbd6019709394ec1e6dc9c/securedrop/pretty_bad_protocol/_parsers.py#L1115'>securedrop/pretty_bad_protocol/_parsers.py:1115:9:</a> PLW3201 Dunder method `__nonzero__` has no special meaning in Python 3
- <a href='https://github.com/freedomofpress/securedrop/blob/dbdccc593e1aeab149dbd6019709394ec1e6dc9c/securedrop/pretty_bad_protocol/_parsers.py#L1313'>securedrop/pretty_bad_protocol/_parsers.py:1313:9:</a> PLW3201 Bad or misspelled dunder method name `__nonzero__`
+ <a href='https://github.com/freedomofpress/securedrop/blob/dbdccc593e1aeab149dbd6019709394ec1e6dc9c/securedrop/pretty_bad_protocol/_parsers.py#L1313'>securedrop/pretty_bad_protocol/_parsers.py:1313:9:</a> PLW3201 Dunder method `__nonzero__` has no special meaning in Python 3
- <a href='https://github.com/freedomofpress/securedrop/blob/dbdccc593e1aeab149dbd6019709394ec1e6dc9c/securedrop/pretty_bad_protocol/_parsers.py#L1417'>securedrop/pretty_bad_protocol/_parsers.py:1417:9:</a> PLW3201 Bad or misspelled dunder method name `__nonzero__`
+ <a href='https://github.com/freedomofpress/securedrop/blob/dbdccc593e1aeab149dbd6019709394ec1e6dc9c/securedrop/pretty_bad_protocol/_parsers.py#L1417'>securedrop/pretty_bad_protocol/_parsers.py:1417:9:</a> PLW3201 Dunder method `__nonzero__` has no special meaning in Python 3
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+91 -91 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/backends/__init__.py#L86'>ibis/backends/__init__.py:86:9:</a> PLW3201 Bad or misspelled dunder method name `_ipython_key_completions_`
+ <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/backends/__init__.py#L86'>ibis/backends/__init__.py:86:9:</a> PLW3201 Dunder method `_ipython_key_completions_` has no special meaning in Python 3
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/backends/__init__.py#L878'>ibis/backends/__init__.py:878:9:</a> PLW3201 Bad or misspelled dunder method name `__rich_repr__`
+ <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/backends/__init__.py#L878'>ibis/backends/__init__.py:878:9:</a> PLW3201 Dunder method `__rich_repr__` has no special meaning in Python 3
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/backends/snowflake/converter.py#L34'>ibis/backends/snowflake/converter.py:34:9:</a> PLW3201 Bad or misspelled dunder method name `__arrow_ext_serialize__`
+ <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/backends/snowflake/converter.py#L34'>ibis/backends/snowflake/converter.py:34:9:</a> PLW3201 Dunder method `__arrow_ext_serialize__` has no special meaning in Python 3
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/backends/snowflake/converter.py#L38'>ibis/backends/snowflake/converter.py:38:9:</a> PLW3201 Bad or misspelled dunder method name `__arrow_ext_deserialize__`
+ <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/backends/snowflake/converter.py#L38'>ibis/backends/snowflake/converter.py:38:9:</a> PLW3201 Dunder method `__arrow_ext_deserialize__` has no special meaning in Python 3
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/backends/snowflake/converter.py#L41'>ibis/backends/snowflake/converter.py:41:9:</a> PLW3201 Bad or misspelled dunder method name `__arrow_ext_class__`
+ <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/backends/snowflake/converter.py#L41'>ibis/backends/snowflake/converter.py:41:9:</a> PLW3201 Dunder method `__arrow_ext_class__` has no special meaning in Python 3
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/backends/snowflake/converter.py#L44'>ibis/backends/snowflake/converter.py:44:9:</a> PLW3201 Bad or misspelled dunder method name `__arrow_ext_scalar_class__`
+ <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/backends/snowflake/converter.py#L44'>ibis/backends/snowflake/converter.py:44:9:</a> PLW3201 Dunder method `__arrow_ext_scalar_class__` has no special meaning in Python 3
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/backends/sql/compilers/base.py#L1196'>ibis/backends/sql/compilers/base.py:1196:9:</a> PLW3201 Bad or misspelled dunder method name `__sql_name__`
+ <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/backends/sql/compilers/base.py#L1196'>ibis/backends/sql/compilers/base.py:1196:9:</a> PLW3201 Dunder method `__sql_name__` has no special meaning in Python 3
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/backends/sql/compilers/pyspark.py#L348'>ibis/backends/sql/compilers/pyspark.py:348:9:</a> PLW3201 Bad or misspelled dunder method name `__sql_name__`
+ <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/backends/sql/compilers/pyspark.py#L348'>ibis/backends/sql/compilers/pyspark.py:348:9:</a> PLW3201 Dunder method `__sql_name__` has no special meaning in Python 3
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/bases.py#L104'>ibis/common/bases.py:104:9:</a> PLW3201 Bad or misspelled dunder method name `__create__`
+ <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/bases.py#L104'>ibis/common/bases.py:104:9:</a> PLW3201 Dunder method `__create__` has no special meaning in Python 3
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/bases.py#L122'>ibis/common/bases.py:122:9:</a> PLW3201 Bad or misspelled dunder method name `__prohibit_inheritance__`
+ <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/bases.py#L122'>ibis/common/bases.py:122:9:</a> PLW3201 Dunder method `__prohibit_inheritance__` has no special meaning in Python 3
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/bases.py#L147'>ibis/common/bases.py:147:9:</a> PLW3201 Bad or misspelled dunder method name `__equals__`
+ <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/bases.py#L147'>ibis/common/bases.py:147:9:</a> PLW3201 Dunder method `__equals__` has no special meaning in Python 3
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/bases.py#L213'>ibis/common/bases.py:213:9:</a> PLW3201 Bad or misspelled dunder method name `__rich_repr__`
+ <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/bases.py#L213'>ibis/common/bases.py:213:9:</a> PLW3201 Dunder method `__rich_repr__` has no special meaning in Python 3
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/deferred.py#L247'>ibis/common/deferred.py:247:9:</a> PLW3201 Bad or misspelled dunder method name `__create__`
+ <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/deferred.py#L247'>ibis/common/deferred.py:247:9:</a> PLW3201 Dunder method `__create__` has no special meaning in Python 3
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/deferred.py#L49'>ibis/common/deferred.py:49:9:</a> PLW3201 Bad or misspelled dunder method name `__coerce__`
+ <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/deferred.py#L49'>ibis/common/deferred.py:49:9:</a> PLW3201 Dunder method `__coerce__` has no special meaning in Python 3
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/egraph.py#L497'>ibis/common/egraph.py:497:9:</a> PLW3201 Bad or misspelled dunder method name `__argnames__`
+ <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/egraph.py#L497'>ibis/common/egraph.py:497:9:</a> PLW3201 Dunder method `__argnames__` has no special meaning in Python 3
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/egraph.py#L502'>ibis/common/egraph.py:502:9:</a> PLW3201 Bad or misspelled dunder method name `__args__`
+ <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/egraph.py#L502'>ibis/common/egraph.py:502:9:</a> PLW3201 Dunder method `__args__` has no special meaning in Python 3
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/graph.py#L212'>ibis/common/graph.py:212:9:</a> PLW3201 Bad or misspelled dunder method name `__recreate__`
+ <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/graph.py#L212'>ibis/common/graph.py:212:9:</a> PLW3201 Dunder method `__recreate__` has no special meaning in Python 3
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/graph.py#L218'>ibis/common/graph.py:218:9:</a> PLW3201 Bad or misspelled dunder method name `__args__`
+ <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/graph.py#L218'>ibis/common/graph.py:218:9:</a> PLW3201 Dunder method `__args__` has no special meaning in Python 3
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/graph.py#L223'>ibis/common/graph.py:223:9:</a> PLW3201 Bad or misspelled dunder method name `__argnames__`
+ <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/graph.py#L223'>ibis/common/graph.py:223:9:</a> PLW3201 Dunder method `__argnames__` has no special meaning in Python 3
- <a href='https://github.com/ibis-project/ibis/blob/7a6af8db91e6909dd8073a6aaa332141c02be163/ibis/common/graph.py#L227'>ibis/common/graph.py:227:9:</a> PLW3201 Bad or misspelled dunder method name `__children__`
... 143 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/wntrblm/nox">wntrblm/nox</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/wntrblm/nox/blob/e0868b6c092c80837f86463a54d34379851d43d7/nox/sessions.py#L1054'>nox/sessions.py:1054:9:</a> PLW3201 Bad or misspelled dunder method name `__nonzero__`
+ <a href='https://github.com/wntrblm/nox/blob/e0868b6c092c80837f86463a54d34379851d43d7/nox/sessions.py#L1054'>nox/sessions.py:1054:9:</a> PLW3201 Dunder method `__nonzero__` has no special meaning in Python 3
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pytest-dev/pytest/blob/7928dade67fdd6851efade25f3462ff6f0ad2d49/src/_pytest/_py/path.py#L337'>src/_pytest/_py/path.py:337:9:</a> PLW3201 Bad or misspelled dunder method name `__div__`
+ <a href='https://github.com/pytest-dev/pytest/blob/7928dade67fdd6851efade25f3462ff6f0ad2d49/src/_pytest/_py/path.py#L337'>src/_pytest/_py/path.py:337:9:</a> PLW3201 Dunder method `__div__` has no special meaning in Python 3
- <a href='https://github.com/pytest-dev/pytest/blob/7928dade67fdd6851efade25f3462ff6f0ad2d49/testing/python/approx.py#L780'>testing/python/approx.py:780:17:</a> PLW3201 Bad or misspelled dunder method name `__array__`
+ <a href='https://github.com/pytest-dev/pytest/blob/7928dade67fdd6851efade25f3462ff6f0ad2d49/testing/python/approx.py#L780'>testing/python/approx.py:780:17:</a> PLW3201 Dunder method `__array__` has no special meaning in Python 3
- <a href='https://github.com/pytest-dev/pytest/blob/7928dade67fdd6851efade25f3462ff6f0ad2d49/testing/python/approx.py#L800'>testing/python/approx.py:800:17:</a> PLW3201 Bad or misspelled dunder method name `__array__`
+ <a href='https://github.com/pytest-dev/pytest/blob/7928dade67fdd6851efade25f3462ff6f0ad2d49/testing/python/approx.py#L800'>testing/python/approx.py:800:17:</a> PLW3201 Dunder method `__array__` has no special meaning in Python 3
- <a href='https://github.com/pytest-dev/pytest/blob/7928dade67fdd6851efade25f3462ff6f0ad2d49/testing/python/approx.py#L807'>testing/python/approx.py:807:17:</a> PLW3201 Bad or misspelled dunder method name `__array__`
+ <a href='https://github.com/pytest-dev/pytest/blob/7928dade67fdd6851efade25f3462ff6f0ad2d49/testing/python/approx.py#L807'>testing/python/approx.py:807:17:</a> PLW3201 Dunder method `__array__` has no special meaning in Python 3
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW3201 | 230 | 115 | 115 | 0 | 0 |

</p>
</details>




---

_Renamed from "[pylint] depracted dunder methods in Python 3 (PLW3201)" to "[pylint] deprecated dunder methods in Python 3 (PLW3201)" by @AlexWaygood on 2024-09-01 18:22_

---

_Comment by @dscorbett on 2024-09-01 20:48_

`__iop__`, `__rop__`, and `__op__` should not be included, because they didn’t exist in Python 2. They were only used in the documentation as placeholders for methods like `__iadd__`, `__radd__`, `__add__`, etc.

---

_Label `rule` added by @MichaReiser on 2024-09-02 08:31_

---

_Label `preview` added by @MichaReiser on 2024-09-02 08:31_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/helpers.rs`:312 on 2024-09-02 11:45_

```suggestion
pub(super) fn is_python2_only_dunder_method(method: &str) -> bool {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_dunder_method_name.rs`:14 on 2024-09-02 11:45_

```suggestion
    RemovedInPython3,
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_dunder_method_name.rs`:11 on 2024-09-02 11:46_

```suggestion
#[derive(Copy, Clone, PartialEq, Eq, Debug)]
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_dunder_method_name.rs`:18 on 2024-09-02 11:46_

```suggestion
/// Checks for misspelled, unknown and no longer supported dunder names in method definitions.
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_dunder_method_name.rs`:21 on 2024-09-02 11:47_

```suggestion
/// Misspelled or no longer supported dunder name methods may cause your code to not function
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/helpers.rs`:311 on 2024-09-02 11:51_

Calling `is_known_dunder_method` first and then `is_deprecated_dunder_method_in_python3` has the downside that it results in two string matches. We can avoid this by:

* Introduce a new `DunderMethodKind` with two variants: `Known`, `Removed`
* Add a `dunder_method_kind(name: &str) -> Option<DunderMethodKind>` 
* Change `is_known_dunder_method` to `dunder_method_kind(name) == DunderMethodKind::Known`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_dunder_method_name.rs`:65 on 2024-09-02 11:54_

```suggestion
                format!("Python 2 or older only dunder method name `{name}`")
```

---

_@MichaReiser approved on 2024-09-02 11:55_

---

_Comment by @MichaReiser on 2024-09-02 11:55_

Thanks, this is looking great

---

_Comment by @Skylion007 on 2024-09-02 16:34_

Shocked this isn't already covered by a pyupgrade rule

---

_Review requested from @MichaReiser by @vieira127 on 2024-09-02 19:21_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_dunder_method_name.rs`:64 on 2024-09-03 06:03_

@AlexWaygood any suggestion on how to improve the wording here? It's kind of hard to read

---

_@MichaReiser reviewed on 2024-09-03 06:03_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pylint/bad_dunder_method_name.py`:97 on 2024-09-03 06:04_


```suggestion
    # Removed with Python 3
```

---

_@MichaReiser reviewed on 2024-09-03 06:04_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLW3201_bad_dunder_method_name.py.snap`:63 on 2024-09-03 06:04_


```suggestion
97 |     # Removed with Python 3
```

---

_@MichaReiser reviewed on 2024-09-03 06:04_

---

_@MichaReiser approved on 2024-09-03 06:04_

---

_Renamed from "[pylint] deprecated dunder methods in Python 3 (PLW3201)" to "[pylint] removed dunder methods in Python 3 (PLW3201)" by @MichaReiser on 2024-09-03 06:05_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-09-03 06:06_

---

_Label `preview` removed by @MichaReiser on 2024-09-03 06:11_

---

_Label `preview` added by @MichaReiser on 2024-09-03 06:13_

---

_@vieira127 reviewed on 2024-09-03 07:31_

---

_Review comment by @vieira127 on `crates/ruff_linter/src/rules/pylint/rules/bad_dunder_method_name.rs`:64 on 2024-09-03 07:31_

I think removing the 'only' makes it read a bit smoother, but it might change the meaning slightly.

---

_@AlexWaygood reviewed on 2024-09-03 10:51_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/bad_dunder_method_name.rs`:64 on 2024-09-03 10:51_

Maybe something like this?

```suggestion
                format!("Dunder method `{name}` has no special meaning in Python 3")
```

Though actually, that's a pretty good description of what the problem is in both cases. I wonder if we need to distinguish between misspelled dunder methods and those that were removed in Python 3 at all, or if we could just improve the docs (like this PR already does) and use a better error message here that applies to both cases? In both cases -- whether you've misspelled a dunder or you're using one that had its special semantics removed in Python 3 -- it's not going to cause any kind of exception to define the dunder method. It's just probably not going to do what you want, because it doesn't have any special semantics attached to it anymore.

---

_@vieira127 reviewed on 2024-09-03 15:59_

---

_Review comment by @vieira127 on `crates/ruff_linter/src/rules/pylint/rules/bad_dunder_method_name.rs`:64 on 2024-09-03 15:59_

Way clearer!

---

_@vieira127 reviewed on 2024-09-03 16:27_

---

_Review comment by @vieira127 on `crates/ruff_linter/src/rules/pylint/rules/bad_dunder_method_name.rs`:64 on 2024-09-03 16:27_

@MichaReiser What do you think about a single message for both cases?

---

_@MichaReiser reviewed on 2024-09-03 16:39_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_dunder_method_name.rs`:64 on 2024-09-03 16:39_

A bit dissatisfying after all the hard work you put into this but I do like the suggested message. 

---

_@vieira127 reviewed on 2024-09-03 16:48_

---

_Review comment by @vieira127 on `crates/ruff_linter/src/rules/pylint/rules/bad_dunder_method_name.rs`:64 on 2024-09-03 16:48_

No problem, I also agree 😄 

---

_Review requested from @AlexWaygood by @vieira127 on 2024-09-03 18:12_

---

_Review requested from @MichaReiser by @vieira127 on 2024-09-03 18:12_

---

_@AlexWaygood approved on 2024-09-03 18:14_

LGTM

---

_@MichaReiser approved on 2024-09-04 06:22_

Thanks a lot!

---

_Merged by @MichaReiser on 2024-09-04 06:23_

---

_Closed by @MichaReiser on 2024-09-04 06:23_

---
