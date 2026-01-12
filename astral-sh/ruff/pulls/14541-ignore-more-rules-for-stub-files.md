```yaml
number: 14541
title: Ignore more rules for stub files
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: main
head: pyi-ignores
created_at: 2024-11-22T21:07:47Z
updated_at: 2024-11-23T07:43:19Z
url: https://github.com/astral-sh/ruff/pull/14541
synced_at: 2026-01-12T15:55:48Z
```

# Ignore more rules for stub files

---

_@dylwil3_

This PR causes the following rules to ignore stub files, on the grounds that it is not under the author's control to appease these lints:

- `PLR0904` https://docs.astral.sh/ruff/rules/too-many-public-methods/
- `PLR0913` https://docs.astral.sh/ruff/rules/too-many-arguments/
- `PLR0917` https://docs.astral.sh/ruff/rules/too-many-positional-arguments/
- `PLW3201` https://docs.astral.sh/ruff/rules/bad-dunder-method-name/
- `SLOT` https://docs.astral.sh/ruff/rules/#flake8-slots-slot
- `FBT` https://docs.astral.sh/ruff/rules/#flake8-boolean-trap-fbt (except for FBT003 since that involves a function call.)

Progress towards #14535


---

_Label `rule` added by @MichaReiser on 2024-11-22 21:13_

---

_@MichaReiser approved on 2024-11-22 21:13_

---

_Comment by @github-actions[bot] on 2024-11-22 21:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -19 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/3c58e01266f884544fdebc70f92b63848c610d2d/airflow/decorators/__init__.pyi#L185'>airflow/decorators/__init__.pyi:185:9:</a> PLR0913 Too many arguments in function definition (9 > 5)
- <a href='https://github.com/apache/airflow/blob/3c58e01266f884544fdebc70f92b63848c610d2d/airflow/decorators/__init__.pyi#L379'>airflow/decorators/__init__.pyi:379:9:</a> PLR0913 Too many arguments in function definition (47 > 5)
- <a href='https://github.com/apache/airflow/blob/3c58e01266f884544fdebc70f92b63848c610d2d/airflow/decorators/__init__.pyi#L537'>airflow/decorators/__init__.pyi:537:9:</a> PLR0913 Too many arguments in function definition (59 > 5)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -16 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/IPython/core/history.pyi#L5'>src/typings/IPython/core/history.pyi:5:38:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/IPython/core/history.pyi#L6'>src/typings/IPython/core/history.pyi:6:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/IPython/core/history.pyi#L8'>src/typings/IPython/core/history.pyi:8:38:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/IPython/core/history.pyi#L9'>src/typings/IPython/core/history.pyi:9:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/bs4.pyi#L21'>src/typings/bs4.pyi:21:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/json5.pyi#L14'>src/typings/json5.pyi:14:5:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/json5.pyi#L15'>src/typings/json5.pyi:15:5:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/json5.pyi#L16'>src/typings/json5.pyi:16:5:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/json5.pyi#L17'>src/typings/json5.pyi:17:5:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/json5.pyi#L4'>src/typings/json5.pyi:4:5:</a> PLR0913 Too many arguments in function definition (13 > 5)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/json5.pyi#L6'>src/typings/json5.pyi:6:5:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/json5.pyi#L7'>src/typings/json5.pyi:7:5:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/json5.pyi#L8'>src/typings/json5.pyi:8:5:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/json5.pyi#L9'>src/typings/json5.pyi:9:5:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/support/expected_conditions.pyi#L103'>src/typings/selenium/webdriver/support/expected_conditions.pyi:103:43:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/support/expected_conditions.pyi#L99'>src/typings/selenium/webdriver/support/expected_conditions.pyi:99:45:</a> FBT001 Boolean-typed positional argument in function definition
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FBT001 | 15 | 0 | 15 | 0 | 0 |
| PLR0913 | 4 | 0 | 4 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -23 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/3c58e01266f884544fdebc70f92b63848c610d2d/airflow/decorators/__init__.pyi#L185'>airflow/decorators/__init__.pyi:185:9:</a> PLR0913 Too many arguments in function definition (9 > 5)
- <a href='https://github.com/apache/airflow/blob/3c58e01266f884544fdebc70f92b63848c610d2d/airflow/decorators/__init__.pyi#L379'>airflow/decorators/__init__.pyi:379:9:</a> PLR0913 Too many arguments in function definition (47 > 5)
- <a href='https://github.com/apache/airflow/blob/3c58e01266f884544fdebc70f92b63848c610d2d/airflow/decorators/__init__.pyi#L537'>airflow/decorators/__init__.pyi:537:9:</a> PLR0913 Too many arguments in function definition (59 > 5)
- <a href='https://github.com/apache/airflow/blob/3c58e01266f884544fdebc70f92b63848c610d2d/airflow/decorators/__init__.pyi#L67'>airflow/decorators/__init__.pyi:67:1:</a> PLR0904 Too many public methods (26 > 20)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -19 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/IPython/core/history.pyi#L5'>src/typings/IPython/core/history.pyi:5:38:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/IPython/core/history.pyi#L6'>src/typings/IPython/core/history.pyi:6:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/IPython/core/history.pyi#L8'>src/typings/IPython/core/history.pyi:8:38:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/IPython/core/history.pyi#L9'>src/typings/IPython/core/history.pyi:9:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/bs4.pyi#L21'>src/typings/bs4.pyi:21:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/json5.pyi#L14'>src/typings/json5.pyi:14:5:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/json5.pyi#L15'>src/typings/json5.pyi:15:5:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/json5.pyi#L16'>src/typings/json5.pyi:16:5:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/json5.pyi#L17'>src/typings/json5.pyi:17:5:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/json5.pyi#L4'>src/typings/json5.pyi:4:5:</a> PLR0913 Too many arguments in function definition (13 > 5)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/json5.pyi#L4'>src/typings/json5.pyi:4:5:</a> PLR0917 Too many positional arguments (13/5)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/json5.pyi#L6'>src/typings/json5.pyi:6:5:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/json5.pyi#L7'>src/typings/json5.pyi:7:5:</a> FBT001 Boolean-typed positional argument in function definition
... 6 additional changes omitted for rule FBT001
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/remote/webelement.pyi#L21'>src/typings/selenium/webdriver/remote/webelement.pyi:21:1:</a> PLR0904 Too many public methods (22 > 20)
</pre>

</p>
</details>
<details><summary>Changes by rule (4 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FBT001 | 16 | 0 | 16 | 0 | 0 |
| PLR0913 | 4 | 0 | 4 | 0 | 0 |
| PLR0904 | 2 | 0 | 2 | 0 | 0 |
| PLR0917 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_@Avasam reviewed on 2024-11-22 22:07_

---

_Review comment by @Avasam on `crates/ruff_linter/src/rules/flake8_boolean_trap/rules/boolean_positional_value_in_call.rs`:58 on 2024-11-22 22:07_

This one I'm not sure about. The other two FBT rules are about definitions, which a stub author doesn't have control over if faithfully representing the runtime implementation.

But this rule is about calls, which shouldn't be found in stubs in the first place (maybe in decorators? In which case it could be argued to apply)

---

_Merged by @dylwil3 on 2024-11-23 07:41_

---

_Closed by @dylwil3 on 2024-11-23 07:41_

---
