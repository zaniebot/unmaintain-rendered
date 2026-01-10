```yaml
number: 10215
title: "[`pep8_naming`] Add fixes `N804` and `N805`"
type: pull_request
state: merged
author: GtrMo
labels:
  - fixes
assignees: []
merged: true
base: main
head: fix_invalid_first_argument
created_at: 2024-03-03T18:58:58Z
updated_at: 2024-03-04T09:42:58Z
url: https://github.com/astral-sh/ruff/pull/10215
synced_at: 2026-01-10T22:47:01Z
```

# [`pep8_naming`] Add fixes `N804` and `N805`

---

_Pull request opened by @GtrMo on 2024-03-03 18:58_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR fixes for `invalid-first-argument` rules.
The fixes rename the first argument of methods and class methods to the valid one. References to this argument are also renamed.
Fixes are skipped when another argument is named as the valid first argument.
The fix is marked as unsafe due

The functions for the `N804` and `N805` rules are now merged, as they only differ by the name of the valid first argument.
The rules were moved from the AST iteration to the deferred scopes to be in the function scope while creating the fix.

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2024-03-03 19:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+12 -12 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/models/connection.py#L352'>airflow/models/connection.py:352:18:</a> ANN101 Missing type annotation for `cls` in method
- <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/models/connection.py#L352'>airflow/models/connection.py:352:18:</a> ANN101 Missing type annotation for `cls` in method
+ <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/models/connection.py#L384'>airflow/models/connection.py:384:15:</a> ANN101 Missing type annotation for `cls` in method
- <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/models/connection.py#L384'>airflow/models/connection.py:384:15:</a> ANN101 Missing type annotation for `cls` in method
+ <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/models/taskinstance.py#L1454'>airflow/models/taskinstance.py:1454:20:</a> ANN101 Missing type annotation for `cls` in method
- <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/models/taskinstance.py#L1454'>airflow/models/taskinstance.py:1454:20:</a> ANN101 Missing type annotation for `cls` in method
+ <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/models/variable.py#L96'>airflow/models/variable.py:96:13:</a> ANN101 Missing type annotation for `cls` in method
- <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/airflow/models/variable.py#L96'>airflow/models/variable.py:96:13:</a> ANN101 Missing type annotation for `cls` in method
+ <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/tests/providers/dbt/cloud/hooks/test_dbt.py#L519'>tests/providers/dbt/cloud/hooks/test_dbt.py:519:38:</a> ANN101 Missing type annotation for `hook` in method
- <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/tests/providers/dbt/cloud/hooks/test_dbt.py#L519'>tests/providers/dbt/cloud/hooks/test_dbt.py:519:38:</a> ANN101 Missing type annotation for `hook` in method
+ <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/tests/providers/odbc/hooks/test_odbc.py#L70'>tests/providers/odbc/hooks/test_odbc.py:70:31:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/1b4b73e20d2f0826151000368e939238f7d1d137/tests/providers/odbc/hooks/test_odbc.py#L70'>tests/providers/odbc/hooks/test_odbc.py:70:31:</a> ANN101 Missing type annotation for `self` in method
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/util.py#L53'>src/bokeh/colors/util.py:53:17:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/util.py#L53'>src/bokeh/colors/util.py:53:17:</a> ANN101 Missing type annotation for `self` in method
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/util.py#L56'>src/bokeh/colors/util.py:56:21:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/util.py#L56'>src/bokeh/colors/util.py:56:21:</a> ANN101 Missing type annotation for `self` in method
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/util.py#L68'>src/bokeh/colors/util.py:68:18:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/util.py#L68'>src/bokeh/colors/util.py:68:18:</a> ANN101 Missing type annotation for `self` in method
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/util.py#L72'>src/bokeh/colors/util.py:72:21:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/util.py#L72'>src/bokeh/colors/util.py:72:21:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_transform.py#L65'>tests/unit/bokeh/test_transform.py:65:20:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_transform.py#L65'>tests/unit/bokeh/test_transform.py:65:20:</a> N805 First argument of a method should be named `self`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_transform.py#L72'>tests/unit/bokeh/test_transform.py:72:27:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_transform.py#L72'>tests/unit/bokeh/test_transform.py:72:27:</a> N805 First argument of a method should be named `self`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ANN101 | 20 | 10 | 10 | 0 | 0 |
| N805 | 4 | 2 | 2 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -3 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_transform.py#L65'>tests/unit/bokeh/test_transform.py:65:20:</a> A002 Argument `object` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_transform.py#L65'>tests/unit/bokeh/test_transform.py:65:20:</a> A002 Argument `object` is shadowing a Python builtin
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_transform.py#L72'>tests/unit/bokeh/test_transform.py:72:27:</a> A002 Argument `object` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_transform.py#L72'>tests/unit/bokeh/test_transform.py:72:27:</a> A002 Argument `object` is shadowing a Python builtin
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/aaa6f1653816e16c29b595d2e38133f2066be08a/zerver/models/realms.py#L1'>zerver/models/realms.py:1:1:</a> D100 Missing docstring in public module
- <a href='https://github.com/zulip/zulip/blob/aaa6f1653816e16c29b595d2e38133f2066be08a/zerver/models/realms.py#L1'>zerver/models/realms.py:1:1:</a> D100 Missing docstring in public module
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| A002 | 4 | 2 | 2 | 0 | 0 |
| D100 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_@charliermarsh reviewed on 2024-03-03 20:02_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_first_argument_name.rs`:260 on 2024-03-03 20:02_

Can you see if `Renamer::rename` would be useful here?

---

_@GtrMo reviewed on 2024-03-04 00:10_

---

_Review comment by @GtrMo on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_first_argument_name.rs`:260 on 2024-03-04 00:10_

`Renamer::rename` would rename all `first_parameter.name` bindings. I wanted to only renaming references to the argument binding, like this:
https://github.com/astral-sh/ruff/blob/90651a281076d91ca06721c59cbc8b469b03ee02/crates/ruff_linter/src/rules/pep8_naming/snapshots/ruff_linter__rules__pep8_naming__tests__N805_N805.py.snap#L241-L245

But I didn't know the `Renamer` existed, and from what I've tested Pyright lsp also renames with the same logic as the `Renamer`. I've updated the PR to use it.

---

_@charliermarsh approved on 2024-03-04 02:08_

This is nice work, great job figuring this out.

---

_Label `fixes` added by @charliermarsh on 2024-03-04 02:16_

---

_Merged by @charliermarsh on 2024-03-04 02:22_

---

_Closed by @charliermarsh on 2024-03-04 02:22_

---

_Branch deleted on 2024-03-04 09:42_

---
