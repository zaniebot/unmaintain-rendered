```yaml
number: 13896
title: Change default for Python version from 3.8 to 3.9
type: pull_request
state: merged
author: takaya0
labels:
  - breaking
  - configuration
assignees: []
merged: true
base: ruff-0.8
head: takaya0/default-python-version-to-3.9
created_at: 2024-10-23T16:14:52Z
updated_at: 2024-11-18T13:29:27Z
url: https://github.com/astral-sh/ruff/pull/13896
synced_at: 2026-01-10T20:50:57Z
```

# Change default for Python version from 3.8 to 3.9

---

_Pull request opened by @takaya0 on 2024-10-23 16:14_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Update default Python version from 3.8 to 3.9.
(for solving #13786 ) 

## Test Plan

<!-- How was it tested? -->


---

_Renamed from "default python version to 3.8" to "default python version to 3.9" by @takaya0 on 2024-10-23 16:15_

---

_Label `breaking` added by @AlexWaygood on 2024-10-23 16:16_

---

_Marked ready for review by @takaya0 on 2024-11-10 11:47_

---

_Review requested from @MichaReiser by @takaya0 on 2024-11-10 11:47_

---

_Comment by @github-actions[bot] on 2024-11-10 12:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+216 -0 violations, +0 -0 fixes in 2 projects; 1 project error; 51 projects unchanged)

<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+216 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/app.py#L257'>lnbits/app.py:257:6:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/app.py#L9'>lnbits/app.py:9:1:</a> UP035 `typing.List` is deprecated, use `list` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/commands.py#L235'>lnbits/commands.py:235:23:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/commands.py#L465'>lnbits/commands.py:465:6:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/commands.py#L494'>lnbits/commands.py:494:6:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/commands.py#L6'>lnbits/commands.py:6:1:</a> UP035 `typing.List` is deprecated, use `list` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/commands.py#L6'>lnbits/commands.py:6:1:</a> UP035 `typing.Tuple` is deprecated, use `tuple` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L1018'>lnbits/core/crud.py:1018:6:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L1235'>lnbits/core/crud.py:1235:43:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L1285'>lnbits/core/crud.py:1285:6:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L433'>lnbits/core/crud.py:433:6:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L460'>lnbits/core/crud.py:460:6:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L485'>lnbits/core/crud.py:485:6:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L4'>lnbits/core/crud.py:4:1:</a> UP035 `typing.Dict` is deprecated, use `dict` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L4'>lnbits/core/crud.py:4:1:</a> UP035 `typing.List` is deprecated, use `list` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L654'>lnbits/core/crud.py:654:75:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L772'>lnbits/core/crud.py:772:13:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L773'>lnbits/core/crud.py:773:13:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L893'>lnbits/core/crud.py:893:21:</a> UP006 Use `dict` instead of `Dict` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L953'>lnbits/core/crud.py:953:17:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L954'>lnbits/core/crud.py:954:20:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/services.py#L121'>lnbits/core/services.py:121:21:</a> UP006 Use `dict` instead of `Dict` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/services.py#L125'>lnbits/core/services.py:125:6:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/services.py#L192'>lnbits/core/services.py:192:21:</a> UP006 Use `dict` instead of `Dict` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/services.py#L234'>lnbits/core/services.py:234:29:</a> UP006 Use `dict` instead of `Dict` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/services.py#L517'>lnbits/core/services.py:517:21:</a> UP006 Use `dict` instead of `Dict` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/services.py#L78'>lnbits/core/services.py:78:21:</a> UP006 Use `dict` instead of `Dict` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/services.py#L7'>lnbits/core/services.py:7:1:</a> UP035 `typing.Dict` is deprecated, use `dict` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/services.py#L7'>lnbits/core/services.py:7:1:</a> UP035 `typing.List` is deprecated, use `list` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/services.py#L7'>lnbits/core/services.py:7:1:</a> UP035 `typing.Tuple` is deprecated, use `tuple` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/services.py#L80'>lnbits/core/services.py:80:26:</a> UP006 Use `dict` instead of `Dict` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/services.py#L80'>lnbits/core/services.py:80:6:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/services.py#L856'>lnbits/core/services.py:856:34:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/tasks.py#L21'>lnbits/core/tasks.py:21:24:</a> UP006 Use `dict` instead of `Dict` for type annotation
... 120 additional changes omitted for rule UP006
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/tasks.py#L2'>lnbits/core/tasks.py:2:1:</a> UP035 `typing.Dict` is deprecated, use `dict` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/api.py#L5'>lnbits/core/views/api.py:5:1:</a> UP035 `typing.Dict` is deprecated, use `dict` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/api.py#L5'>lnbits/core/views/api.py:5:1:</a> UP035 `typing.List` is deprecated, use `list` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/extension_api.py#L3'>lnbits/core/views/extension_api.py:3:1:</a> UP035 `typing.List` is deprecated, use `list` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/generic.py#L3'>lnbits/core/views/generic.py:3:1:</a> UP035 `typing.List` is deprecated, use `list` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/node_api.py#L2'>lnbits/core/views/node_api.py:2:1:</a> UP035 `typing.List` is deprecated, use `list` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/payment_api.py#L6'>lnbits/core/views/payment_api.py:6:1:</a> UP035 `typing.List` is deprecated, use `list` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/user_api.py#L2'>lnbits/core/views/user_api.py:2:1:</a> UP035 `typing.List` is deprecated, use `list` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/decorators.py#L2'>lnbits/decorators.py:2:1:</a> UP035 `typing.Type` is deprecated, use `type` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/extension_manager.py#L9'>lnbits/extension_manager.py:9:1:</a> UP035 `typing.List` is deprecated, use `list` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/extension_manager.py#L9'>lnbits/extension_manager.py:9:1:</a> UP035 `typing.Tuple` is deprecated, use `tuple` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/helpers.py#L5'>lnbits/helpers.py:5:1:</a> UP035 `typing.List` is deprecated, use `list` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/helpers.py#L5'>lnbits/helpers.py:5:1:</a> UP035 `typing.Type` is deprecated, use `type` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/middleware.py#L2'>lnbits/middleware.py:2:1:</a> UP035 `typing.List` is deprecated, use `list` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/middleware.py#L2'>lnbits/middleware.py:2:1:</a> UP035 `typing.Tuple` is deprecated, use `tuple` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/tasks.py#L7'>lnbits/tasks.py:7:1:</a> UP035 [*] Import from `collections.abc` instead: `Coroutine`
... 166 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 8, column 1
  |
8 | [lint]
  | ^^^^^^
Unknown rule selector: `UP027`
```

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP006 | 145 | 145 | 0 | 0 | 0 |
| UP035 | 71 | 71 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+8555 -8359 violations, +0 -66 fixes in 10 projects; 1 project error; 43 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6176 -6179 violations, +0 -58 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/api/__init__.py#L32'>airflow/api/__init__.py:32:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/api/__init__.py#L32'>airflow/api/__init__.py:32:5:</a> DOC501 Raised exception `AirflowException` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/api/__init__.py#L43'>airflow/api/__init__.py:43:15:</a> DOC501 Raised exception `AirflowException` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/api/__init__.py#L44'>airflow/api/__init__.py:44:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/api/auth/backend/deny_all.py#L38'>airflow/api/auth/backend/deny_all.py:38:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/api/auth/backend/deny_all.py#L44'>airflow/api/auth/backend/deny_all.py:44:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/api/client/__init__.py#L27'>airflow/api/client/__init__.py:27:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/api/client/__init__.py#L38'>airflow/api/client/__init__.py:38:5:</a> DOC201 `return` is not documented in docstring
... 9019 additional changes omitted for rule DOC201
- <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/api/common/delete_dag.py#L43'>airflow/api/common/delete_dag.py:43:5:</a> DOC501 Raised exception `AirflowException` missing from docstring
- <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/api/common/delete_dag.py#L43'>airflow/api/common/delete_dag.py:43:5:</a> DOC501 Raised exception `DagNotFound` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/api/common/delete_dag.py#L61'>airflow/api/common/delete_dag.py:61:15:</a> DOC501 Raised exception `AirflowException` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/api/common/delete_dag.py#L64'>airflow/api/common/delete_dag.py:64:15:</a> DOC501 Raised exception `DagNotFound` missing from docstring
... 2949 additional changes omitted for rule DOC501
- <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/api/common/mark_tasks.py#L186'>airflow/api/common/mark_tasks.py:186:5:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/api/common/mark_tasks.py#L190'>airflow/api/common/mark_tasks.py:190:13:</a> DOC402 `yield` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/api_fastapi/common/db/common.py#L33'>airflow/api_fastapi/common/db/common.py:33:5:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/api_fastapi/common/db/common.py#L47'>airflow/api_fastapi/common/db/common.py:47:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/assets/__init__.py#L128'>airflow/assets/__init__.py:128:8:</a> PLR1714 Consider merging multiple comparisons: `value in ("self", "context")`. Use a `set` if the elements are hashable.
- <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/assets/__init__.py#L128'>airflow/assets/__init__.py:128:8:</a> PLR1714 Consider merging multiple comparisons: `value in {"self", "context"}`.
- <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/assets/__init__.py#L238'>airflow/assets/__init__.py:238:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/assets/__init__.py#L243'>airflow/assets/__init__.py:243:9:</a> DOC402 `yield` is not documented in docstring
... 329 additional changes omitted for rule DOC402
+ <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:25:</a> PYI041 Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:25:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/decorators/__init__.pyi#L256'>airflow/decorators/__init__.pyi:256:25:</a> PYI041 Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/decorators/__init__.pyi#L256'>airflow/decorators/__init__.pyi:256:25:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/jobs/job.py#L308'>airflow/jobs/job.py:308:39:</a> PYI041 Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/jobs/job.py#L308'>airflow/jobs/job.py:308:39:</a> PYI041 [*] Use `float` instead of `int | float`
... 53 additional changes omitted for rule PYI041
- <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/models/dag.py#L1038'>airflow/models/dag.py:1038:36:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/models/dagrun.py#L1317'>airflow/models/dagrun.py:1317:23:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
- <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/models/dagrun.py#L1439'>airflow/models/dagrun.py:1439:23:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
+ <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/www/decorators.py#L55'>airflow/www/decorators.py:55:27:</a> PLR1714 Consider merging multiple comparisons: `k in ("val", "value")`. Use a `set` if the elements are hashable.
- <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/www/decorators.py#L55'>airflow/www/decorators.py:55:27:</a> PLR1714 Consider merging multiple comparisons: `k in {"val", "value"}`.
+ <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/www/views.py#L4340'>airflow/www/views.py:4340:21:</a> PLR1714 Consider merging multiple comparisons: `parsed_url.scheme in ("http", "https")`. Use a `set` if the elements are hashable.
- <a href='https://github.com/apache/airflow/blob/f3bad1e9e173565e035e5c3d2a47eb8a0cb0de97/airflow/www/views.py#L4340'>airflow/www/views.py:4340:21:</a> PLR1714 Consider merging multiple comparisons: `parsed_url.scheme in {"http", "https"}`.
... 35 additional changes omitted for rule PLR1714
... 12380 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1170 -1172 violations, +0 -8 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/RELEASING/changelog.py#L104'>RELEASING/changelog.py:104:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/RELEASING/changelog.py#L107'>RELEASING/changelog.py:107:9:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/RELEASING/changelog.py#L113'>RELEASING/changelog.py:113:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/RELEASING/changelog.py#L52'>RELEASING/changelog.py:52:9:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/RELEASING/changelog.py#L54'>RELEASING/changelog.py:54:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/RELEASING/changelog.py#L87'>RELEASING/changelog.py:87:9:</a> DOC201 `return` is not documented in docstring
... 1824 additional changes omitted for rule DOC201
- <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/scripts/benchmark_migration.py#L43'>scripts/benchmark_migration.py:43:5:</a> DOC501 Raised exception `Exception` missing from docstring
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/scripts/benchmark_migration.py#L51'>scripts/benchmark_migration.py:51:11:</a> DOC501 Raised exception `Exception` missing from docstring
- <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/scripts/cancel_github_workflows.py#L162'>scripts/cancel_github_workflows.py:162:5:</a> DOC501 Raised exception `ClickException` missing from docstring
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/scripts/cancel_github_workflows.py#L164'>scripts/cancel_github_workflows.py:164:15:</a> DOC501 Raised exception `ClickException` missing from docstring
... 2340 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/integration/publish/publish_app_integ_base.py#L60'>tests/integration/publish/publish_app_integ_base.py:60:16:</a> PLR1714 Consider merging multiple comparisons: `f.suffix in (".yaml", ".json")`. Use a `set` if the elements are hashable.
- <a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/integration/publish/publish_app_integ_base.py#L60'>tests/integration/publish/publish_app_integ_base.py:60:16:</a> PLR1714 Consider merging multiple comparisons: `f.suffix in {".yaml", ".json"}`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+407 -406 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L107'>examples/advanced/extensions/parallel_plot/parallel_plot.py:107:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L15'>examples/advanced/extensions/parallel_plot/parallel_plot.py:15:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L53'>examples/basic/data/server_sent_events_source.py:53:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L60'>examples/basic/data/server_sent_events_source.py:60:13:</a> DOC402 `yield` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/js_on_event.py#L16'>examples/interaction/js_callbacks/js_on_event.py:16:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/js_on_event.py#L21'>examples/interaction/js_callbacks/js_on_event.py:21:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/daylight.py#L83'>examples/models/daylight.py:83:12:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/gauges.py#L33'>examples/models/gauges.py:33:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/gauges.py#L34'>examples/models/gauges.py:34:5:</a> DOC201 `return` is not documented in docstring
... 395 additional changes omitted for rule DOC201
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L63'>src/bokeh/__init__.py:63:5:</a> DOC202 Docstring should not have a returns section because the function doesn't return anything
... 803 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/types/metadata.py#L500'>src/latch/types/metadata.py:500:45:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch_cli/services/stop_pod.py#L22'>src/latch_cli/services/stop_pod.py:22:8:</a> PLR1714 Consider merging multiple comparisons: `res.status_code in (403, 404)`. Use a `set` if the elements are hashable.
- <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch_cli/services/stop_pod.py#L22'>src/latch_cli/services/stop_pod.py:22:8:</a> PLR1714 Consider merging multiple comparisons: `res.status_code in {403, 404}`.
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch_cli/snakemake/single_task_snakemake.py#L362'>src/latch_cli/snakemake/single_task_snakemake.py:362:8:</a> PLR1714 Consider merging multiple comparisons: `parsed.scheme not in ("", "docker")`. Use a `set` if the elements are hashable.
- <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch_cli/snakemake/single_task_snakemake.py#L362'>src/latch_cli/snakemake/single_task_snakemake.py:362:8:</a> PLR1714 Consider merging multiple comparisons: `parsed.scheme not in {"", "docker"}`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+216 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/app.py#L257'>lnbits/app.py:257:6:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/app.py#L9'>lnbits/app.py:9:1:</a> UP035 `typing.List` is deprecated, use `list` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/commands.py#L235'>lnbits/commands.py:235:23:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/commands.py#L465'>lnbits/commands.py:465:6:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/commands.py#L494'>lnbits/commands.py:494:6:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/commands.py#L6'>lnbits/commands.py:6:1:</a> UP035 `typing.List` is deprecated, use `list` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/commands.py#L6'>lnbits/commands.py:6:1:</a> UP035 `typing.Tuple` is deprecated, use `tuple` instead
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L1018'>lnbits/core/crud.py:1018:6:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L1235'>lnbits/core/crud.py:1235:43:</a> UP006 Use `list` instead of `List` for type annotation
... 140 additional changes omitted for rule UP006
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L4'>lnbits/core/crud.py:4:1:</a> UP035 `typing.Dict` is deprecated, use `dict` instead
... 206 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/core/groupby/groupby.py#L4069'>pandas/core/groupby/groupby.py:4069:39:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/core/groupby/indexing.py#L299'>pandas/core/groupby/indexing.py:299:39:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/io/html.py#L1027'>pandas/io/html.py:1027:28:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/io/html.py#L223'>pandas/io/html.py:223:32:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+2 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/2cc71de3c38bb2911fb38333cf805217fde620ea/stdlib/ast.pyi#L1480'>stdlib/ast.pyi:1480:16:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
- <a href='https://github.com/python/typeshed/blob/2cc71de3c38bb2911fb38333cf805217fde620ea/stdlib/ast.pyi#L1481'>stdlib/ast.pyi:1481:35:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
- <a href='https://github.com/python/typeshed/blob/2cc71de3c38bb2911fb38333cf805217fde620ea/stdlib/ast.pyi#L1484'>stdlib/ast.pyi:1484:45:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
+ <a href='https://github.com/python/typeshed/blob/2cc71de3c38bb2911fb38333cf805217fde620ea/stdlib/random.pyi#L45'>stdlib/random.pyi:45:31:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/2cc71de3c38bb2911fb38333cf805217fde620ea/stdlib/random.pyi#L52'>stdlib/random.pyi:52:27:</a> PYI041 Use `float` instead of `int | float`
- <a href='https://github.com/python/typeshed/blob/2cc71de3c38bb2911fb38333cf805217fde620ea/stubs/pyxdg/xdg/Menu.pyi#L97'>stubs/pyxdg/xdg/Menu.pyi:97:11:</a> RUF038 `Literal[True, False, ...]` can be replaced with `Literal[...] | bool`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+581 -589 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/analytics/lib/fixtures.py#L19'>analytics/lib/fixtures.py:19:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/analytics/lib/fixtures.py#L19'>analytics/lib/fixtures.py:19:5:</a> DOC501 Raised exception `AssertionError` missing from docstring
+ <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/analytics/lib/fixtures.py#L56'>analytics/lib/fixtures.py:56:15:</a> DOC501 Raised exception `AssertionError` missing from docstring
+ <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/analytics/lib/fixtures.py#L77'>analytics/lib/fixtures.py:77:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/confirmation/models.py#L125'>confirmation/models.py:125:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/confirmation/models.py#L279'>confirmation/models.py:279:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/confirmation/models.py#L283'>confirmation/models.py:283:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/confirmation/models.py#L298'>confirmation/models.py:298:5:</a> DOC201 `return` is not documented in docstring
... 857 additional changes omitted for rule DOC201
- <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/confirmation/models.py#L298'>confirmation/models.py:298:5:</a> DOC501 Raised exception `InvalidError` missing from docstring
+ <a href='https://github.com/zulip/zulip/blob/a8abcf5210deb1894147077a19545686418198d1/confirmation/models.py#L304'>confirmation/models.py:304:15:</a> DOC501 Raised exception `InvalidError` missing from docstring
... 1160 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 8, column 1
  |
8 | [lint]
  | ^^^^^^
Unknown rule selector: `UP027`
```

</p>
</details>
<details><summary>Changes by rule (14 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DOC201 | 12115 | 6057 | 6058 | 0 | 0 |
| DOC501 | 3924 | 1962 | 1962 | 0 | 0 |
| DOC402 | 408 | 204 | 204 | 0 | 0 |
| DOC202 | 158 | 79 | 79 | 0 | 0 |
| UP006 | 145 | 145 | 0 | 0 | 0 |
| UP035 | 71 | 71 | 0 | 0 | 0 |
| PYI041 | 68 | 2 | 0 | 0 | 66 |
| PLR1714 | 62 | 31 | 31 | 0 | 0 |
| PYI061 | 14 | 0 | 14 | 0 | 0 |
| RUF038 | 7 | 0 | 7 | 0 | 0 |
| DOC502 | 4 | 2 | 2 | 0 | 0 |
| DOC403 | 2 | 1 | 1 | 0 | 0 |
| DTZ901 | 1 | 1 | 0 | 0 | 0 |
| RUF100 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+111 -85 lines in 16 files in 4 projects; 1 project error; 49 projects unchanged)

<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+34 -30 lines across 4 files)</summary>
<p>

<a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/integration/pipeline/test_init_command.py#L98'>tests/integration/pipeline/test_init_command.py~L98</a>
```diff
 
         self.assertEqual(init_process_execute.process.returncode, 0)
 
-        with open(EXPECTED_JENKINS_FILE_PATH, "r") as expected, open(
-            os.path.join(".aws-sam", "pipeline", "generated-files", "Jenkinsfile"), "r"
-        ) as output:
+        with (
+            open(EXPECTED_JENKINS_FILE_PATH, "r") as expected,
+            open(os.path.join(".aws-sam", "pipeline", "generated-files", "Jenkinsfile"), "r") as output,
+        ):
             self.assertEqual(expected.read(), output.read())
 
         # also check the Jenkinsfile is not overridden
```
<a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/unit/commands/samconfig/test_samconfig.py#L1066'>tests/unit/commands/samconfig/test_samconfig.py~L1066</a>
```diff
         }
 
         # NOTE: Because we don't load the full Click BaseCommand here, this is mounted as top-level command
-        with samconfig_parameters(
-            ["start-lambda"], self.scratch_dir, **config_values
-        ) as config_path, tempfile.NamedTemporaryFile() as key_file, tempfile.NamedTemporaryFile() as cert_file:
+        with (
+            samconfig_parameters(["start-lambda"], self.scratch_dir, **config_values) as config_path,
+            tempfile.NamedTemporaryFile() as key_file,
+            tempfile.NamedTemporaryFile() as cert_file,
+        ):
             from samcli.commands.local.start_lambda.cli import cli
 
             LOG.debug(Path(config_path).read_text())
```
<a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/unit/commands/samconfig/test_samconfig.py#L1171'>tests/unit/commands/samconfig/test_samconfig.py~L1171</a>
```diff
         }
 
         # NOTE: Because we don't load the full Click BaseCommand here, this is mounted as top-level command
-        with samconfig_parameters(
-            ["start-lambda"], self.scratch_dir, **config_values
-        ) as config_path, tempfile.NamedTemporaryFile() as key_file, tempfile.NamedTemporaryFile() as cert_file:
+        with (
+            samconfig_parameters(["start-lambda"], self.scratch_dir, **config_values) as config_path,
+            tempfile.NamedTemporaryFile() as key_file,
+            tempfile.NamedTemporaryFile() as cert_file,
+        ):
             from samcli.commands.local.start_lambda.cli import cli
 
             LOG.debug(Path(config_path).read_text())
```
<a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/unit/lib/build_module/test_build_strategy.py#L747'>tests/unit/lib/build_module/test_build_strategy.py~L747</a>
```diff
     def test_will_call_incremental_build_strategy(self, mocked_read, mocked_write, runtime):
         build_definition = FunctionBuildDefinition(runtime, "codeuri", None, "package_type", X86_64, {}, "handler")
         self.build_graph.put_function_build_definition(build_definition, Mock(full_path="function_full_path"))
-        with patch.object(
-            self.build_strategy, "_incremental_build_strategy"
-        ) as patched_incremental_build_strategy, patch.object(
-            self.build_strategy, "_cached_build_strategy"
-        ) as patched_cached_build_strategy:
+        with (
+            patch.object(self.build_strategy, "_incremental_build_strategy") as patched_incremental_build_strategy,
+            patch.object(self.build_strategy, "_cached_build_strategy") as patched_cached_build_strategy,
+        ):
             self.build_strategy.build()
 
             patched_incremental_build_strategy.build_single_function_definition.assert_called_with(build_definition)
```
<a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/unit/lib/build_module/test_build_strategy.py#L767'>tests/unit/lib/build_module/test_build_strategy.py~L767</a>
```diff
     def test_will_call_cached_build_strategy(self, mocked_read, mocked_write, runtime):
         build_definition = FunctionBuildDefinition(runtime, "codeuri", None, "package_type", X86_64, {}, "handler")
         self.build_graph.put_function_build_definition(build_definition, Mock(full_path="function_full_path"))
-        with patch.object(
-            self.build_strategy, "_incremental_build_strategy"
-        ) as patched_incremental_build_strategy, patch.object(
-            self.build_strategy, "_cached_build_strategy"
-        ) as patched_cached_build_strategy:
+        with (
+            patch.object(self.build_strategy, "_incremental_build_strategy") as patched_incremental_build_strategy,
+            patch.object(self.build_strategy, "_cached_build_strategy") as patched_cached_build_strategy,
+        ):
             self.build_strategy.build()
 
             patched_cached_build_strategy.build_single_function_definition.assert_called_with(build_definition)
```
<a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/unit/lib/build_module/test_build_strategy.py#L841'>tests/unit/lib/build_module/test_build_strategy.py~L841</a>
```diff
 
         build_definition = FunctionBuildDefinition(runtime, "codeuri", None, "package_type", X86_64, {}, "handler")
         self.build_graph.put_function_build_definition(build_definition, Mock(full_path="function_full_path"))
-        with patch.object(
-            build_strategy, "_incremental_build_strategy"
-        ) as patched_incremental_build_strategy, patch.object(
-            build_strategy, "_cached_build_strategy"
-        ) as patched_cached_build_strategy:
+        with (
+            patch.object(build_strategy, "_incremental_build_strategy") as patched_incremental_build_strategy,
+            patch.object(build_strategy, "_cached_build_strategy") as patched_cached_build_strategy,
+        ):
             build_strategy.build()
 
             if not use_container:
```
<a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/unit/lib/remote_invoke/test_remote_invoke_executors.py#L79'>tests/unit/lib/remote_invoke/test_remote_invoke_executors.py~L79</a>
```diff
         given_output_format = "text"
         test_execution_info = RemoteInvokeExecutionInfo(given_payload, None, given_parameters, given_output_format)
 
-        with patch.object(self.boto_action_executor, "_execute_action") as patched_execute_action, patch.object(
-            self.boto_action_executor, "_execute_action_file"
-        ) as patched_execute_action_file:
+        with (
+            patch.object(self.boto_action_executor, "_execute_action") as patched_execute_action,
+            patch.object(self.boto_action_executor, "_execute_action_file") as patched_execute_action_file,
+        ):
             given_result = Mock()
             patched_execute_action.return_value = given_result
 
```
<a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/unit/lib/remote_invoke/test_remote_invoke_executors.py#L96'>tests/unit/lib/remote_invoke/test_remote_invoke_executors.py~L96</a>
```diff
         given_output_format = "json"
         test_execution_info = RemoteInvokeExecutionInfo(None, given_payload_file, given_parameters, given_output_format)
 
-        with patch.object(self.boto_action_executor, "_execute_action") as patched_execute_action, patch.object(
-            self.boto_action_executor, "_execute_action_file"
-        ) as patched_execute_action_file:
+        with (
+            patch.object(self.boto_action_executor, "_execute_action") as patched_execute_action,
+            patch.object(self.boto_action_executor, "_execute_action_file") as patched_execute_action_file,
+        ):
             given_result = Mock()
             patched_execute_action_file.return_value = given_result
 
```

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+32 -23 lines across 5 files)</summary>
<p>

<a href='https://github.com/langchain-ai/langchain/blob/76e210a34964aa264bc49aa2b583d725694caf8d/libs/community/tests/unit_tests/document_loaders/test_mongodb.py#L50'>libs/community/tests/unit_tests/document_loaders/test_mongodb.py~L50</a>
```diff
     mock_collection.find = mock_find
     mock_collection.count_documents = mock_count_documents
 
-    with patch(
-        "motor.motor_asyncio.AsyncIOMotorClient", return_value=MagicMock()
-    ), patch(
-        "langchain_community.document_loaders.mongodb.MongodbLoader.aload",
-        new=mock_async_load,
+    with (
+        patch("motor.motor_asyncio.AsyncIOMotorClient", return_value=MagicMock()),
+        patch(
+            "langchain_community.document_loaders.mongodb.MongodbLoader.aload",
+            new=mock_async_load,
+        ),
     ):
         loader = MongodbLoader(
             "mongodb://localhost:27017",
```
<a href='https://github.com/langchain-ai/langchain/blob/76e210a34964aa264bc49aa2b583d725694caf8d/libs/community/tests/unit_tests/tools/audio/test_tools.py#L44'>libs/community/tests/unit_tests/tools/audio/test_tools.py~L44</a>
```diff
 def test_huggingface_tts_run_with_requests_mock() -> None:
     os.environ["HUGGINGFACE_API_KEY"] = "foo"
 
-    with tempfile.TemporaryDirectory() as tmp_dir, patch(
-        "uuid.uuid4"
-    ) as mock_uuid, patch("requests.post") as mock_inference, patch(
-        "builtins.open", mock_open()
-    ) as mock_file:
+    with (
+        tempfile.TemporaryDirectory() as tmp_dir,
+        patch("uuid.uuid4") as mock_uuid,
+        patch("requests.post") as mock_inference,
+        patch("builtins.open", mock_open()) as mock_file,
+    ):
         input_query = "Dummy input"
 
         mock_uuid_value = uuid.UUID("00000000-0000-0000-0000-000000000000")
```
<a href='https://github.com/langchain-ai/langchain/blob/76e210a34964aa264bc49aa2b583d725694caf8d/libs/community/tests/unit_tests/vectorstores/test_azure_search.py#L220'>libs/community/tests/unit_tests/vectorstores/test_azure_search.py~L220</a>
```diff
     ]
     ids_provided = [i.metadata.get("id") for i in documents]
 
-    with patch.object(
-        SearchClient, "upload_documents", mock_upload_documents
-    ), patch.object(SearchIndexClient, "get_index", mock_default_index):
+    with (
+        patch.object(SearchClient, "upload_documents", mock_upload_documents),
+        patch.object(SearchIndexClient, "get_index", mock_default_index),
+    ):
         vector_store = create_vector_store()
         ids_used_at_upload = vector_store.add_documents(documents, ids=ids_provided)
         assert len(ids_provided) == len(ids_used_at_upload)
```
<a href='https://github.com/langchain-ai/langchain/blob/76e210a34964aa264bc49aa2b583d725694caf8d/libs/langchain/tests/unit_tests/smith/evaluation/test_runner_utils.py#L316'>libs/langchain/tests/unit_tests/smith/evaluation/test_runner_utils.py~L316</a>
```diff
         proj.id = "123"
         return proj
 
-    with mock.patch.object(
-        Client, "read_dataset", new=mock_read_dataset
-    ), mock.patch.object(Client, "list_examples", new=mock_list_examples), mock.patch(
-        "langchain.smith.evaluation.runner_utils._arun_llm_or_chain",
-        new=mock_arun_chain,
-    ), mock.patch.object(Client, "create_project", new=mock_create_project):
+    with (
+        mock.patch.object(Client, "read_dataset", new=mock_read_dataset),
+        mock.patch.object(Client, "list_examples", new=mock_list_examples),
+        mock.patch(
+            "langchain.smith.evaluation.runner_utils._arun_llm_or_chain",
+            new=mock_arun_chain,
+        ),
+        mock.patch.object(Client, "create_project", new=mock_create_project),
+    ):
         client = Client(api_url="http://localhost:1984", api_key="123")
         chain = mock.MagicMock()
         chain.input_keys = ["foothing"]
```
<a href='https://github.com/langchain-ai/langchain/blob/76e210a34964aa264bc49aa2b583d725694caf8d/libs/partners/huggingface/tests/unit_tests/test_chat_models.py#L231'>libs/partners/huggingface/tests/unit_tests/test_chat_models.py~L231</a>
```diff
 
 def test_bind_tools(chat_hugging_face: Any) -> None:
     tools = [MagicMock(spec=BaseTool)]
-    with patch(
-        "langchain_huggingface.chat_models.huggingface.convert_to_openai_tool",
-        side_effect=lambda x: x,
-    ), patch("langchain_core.runnables.base.Runnable.bind") as mock_super_bind:
+    with (
+        patch(
+            "langchain_huggingface.chat_models.huggingface.convert_to_openai_tool",
+            side_effect=lambda x: x,
+        ),
+        patch("langchain_core.runnables.base.Runnable.bind") as mock_super_bind,
+    ):
         chat_hugging_face.bind_tools(tools, tool_choice="auto")
         mock_super_bind.assert_called_once()
         _, kwargs = mock_super_bind.call_args
```

</p>
</details>
<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (+38 -27 lines across 5 files)</summary>
<p>

<a href='https://github.com/prefecthq/prefect/blob/2bf9d05e96a47710d59c242d54df25dd1d58b4d5/src/integrations/prefect-dbt/prefect_dbt/cloud/jobs.py#L752'>src/integrations/prefect-dbt/prefect_dbt/cloud/jobs.py~L752</a>
```diff
         run_status = DbtCloudJobRunStatus(run_data.get("status"))
         if run_status == DbtCloudJobRunStatus.SUCCESS:
             try:
-                async with self._dbt_cloud_credentials.get_administrative_client() as client:  # noqa
+                async with (
+                    self._dbt_cloud_credentials.get_administrative_client() as client
+                ):  # noqa
                     response = await client.list_run_artifacts(
                         run_id=self.run_id, step=step
                     )
```
<a href='https://github.com/prefecthq/prefect/blob/2bf9d05e96a47710d59c242d54df25dd1d58b4d5/tests/runner/test_webserver.py#L151'>tests/runner/test_webserver.py~L151</a>
```diff
             webserver = await build_server(runner)
             client = TestClient(webserver)
 
-            with mock.patch(
-                "prefect.runner.server.get_client", new=mock_get_client
-            ), mock.patch.object(runner, "execute_in_background"):
+            with (
+                mock.patch("prefect.runner.server.get_client", new=mock_get_client),
+                mock.patch.object(runner, "execute_in_background"),
+            ):
                 with client:
                     response = client.post(f"/deployment/{deployment_id}/run")
                 assert response.status_code == 201, response.json()
```
<a href='https://github.com/prefecthq/prefect/blob/2bf9d05e96a47710d59c242d54df25dd1d58b4d5/tests/server/orchestration/api/test_task_run_subscriptions.py#L326'>tests/server/orchestration/api/test_task_run_subscriptions.py~L326</a>
```diff
             )
             await queue.put(task_run)
 
-        with patch("asyncio.sleep", return_value=None), pytest.raises(
-            asyncio.TimeoutError
+        with (
+            patch("asyncio.sleep", return_value=None),
+            pytest.raises(asyncio.TimeoutError),
         ):
             extra_task_run = ServerTaskRun(
                 id=uuid4(),
```
<a href='https://github.com/prefecthq/prefect/blob/2bf9d05e96a47710d59c242d54df25dd1d58b4d5/tests/server/orchestration/api/test_task_run_subscriptions.py#L356'>tests/server/orchestration/api/test_task_run_subscriptions.py~L356</a>
```diff
         )
         await queue.retry(task_run)
 
-        with patch("asyncio.sleep", return_value=None), pytest.raises(
-            asyncio.TimeoutError
+        with (
+            patch("asyncio.sleep", return_value=None),
+            pytest.raises(asyncio.TimeoutError),
         ):
             extra_task_run = ServerTaskRun(
                 id=uuid4(),
```
<a href='https://github.com/prefecthq/prefect/blob/2bf9d05e96a47710d59c242d54df25dd1d58b4d5/tests/test_task_worker.py#L106'>tests/test_task_worker.py~L106</a>
```diff
 async def test_handle_sigterm(mock_create_subscription):
     task_worker = TaskWorker(...)
 
-    with patch("sys.exit") as mock_exit, patch.object(
-        task_worker, "stop", new_callable=AsyncMock
-    ) as mock_stop:
+    with (
+        patch("sys.exit") as mock_exit,
+        patch.object(task_worker, "stop", new_callable=AsyncMock) as mock_stop,
+    ):
         await task_worker.start()
 
         mock_create_subscription.assert_called_once()
```
<a href='https://github.com/prefecthq/prefect/blob/2bf9d05e96a47710d59c242d54df25dd1d58b4d5/tests/test_task_worker.py#L120'>tests/test_task_worker.py~L120</a>
```diff
 
 
 async def test_task_worker_client_id_is_set():
-    with patch("socket.gethostname", return_value="foo"), patch(
-        "os.getpid", return_value=42
+    with (
+        patch("socket.gethostname", return_value="foo"),
+        patch("os.getpid", return_value=42),
     ):
         task_worker = TaskWorker(...)
         task_worker._client = MagicMock(api_url="http://localhost:4200")
```
<a href='https://github.com/prefecthq/prefect/blob/2bf9d05e96a47710d59c242d54df25dd1d58b4d5/tests/workers/test_base_worker.py#L1905'>tests/workers/test_base_worker.py~L1905</a>
```diff
     ):
         async with WorkerTestImpl(work_pool_name=work_pool.name) as worker:
             await worker.start(run_once=True)
-            with mock.patch(
-                "prefect.workers.base.load_prefect_collections"
-            ) as mock_load_prefect_collections, mock.patch(
-                "prefect.client.orchestration.PrefectHttpxAsyncClient.post"
-            ) as mock_send_worker_heartbeat_post, mock.patch(
-                "prefect.workers.base.distributions"
-            ) as mock_distributions:
+            with (
+                mock.patch(
+                    "prefect.workers.base.load_prefect_collections"
+                ) as mock_load_prefect_collections,
+                mock.patch(
+                    "prefect.client.orchestration.PrefectHttpxAsyncClient.post"
+                ) as mock_send_worker_heartbeat_post,
+                mock.patch("prefect.workers.base.distributions") as mock_distributions,
+            ):
                 mock_load_prefect_collections.return_value = {
                     "prefect_aws": "1.0.0",
                 }
```
<a href='https://github.com/prefecthq/prefect/blob/2bf9d05e96a47710d59c242d54df25dd1d58b4d5/tests/workers/test_base_worker.py#L1963'>tests/workers/test_base_worker.py~L1963</a>
```diff
 
         async with CustomWorker(work_pool_name=work_pool.name) as worker:
             await worker.start(run_once=True)
-            with mock.patch(
-                "prefect.workers.base.load_prefect_collections"
-            ) as mock_load_prefect_collections, mock.patch(
-                "prefect.client.orchestration.PrefectHttpxAsyncClient.post"
-            ) as mock_send_worker_heartbeat_post, mock.patch(
-                "prefect.workers.base.distributions"
-            ) as mock_distributions:
+            with (
+                mock.patch(
+                    "prefect.workers.base.load_prefect_collections"
+                ) as mock_load_prefect_collections,
+                mock.patch(
+                    "prefect.client.orchestration.PrefectHttpxAsyncClient.post"
+                ) as mock_send_worker_heartbeat_post,
+                mock.patch("prefect.workers.base.distributions") as mock_distributions,
+            ):
                 mock_load_prefect_collections.return_value = {
                     "prefect_aws": "1.0.0",
                 }
```

</p>
</details>
<details><summary><a href="https://github.com/yandex/ch-backup">yandex/ch-backup</a> (+7 -5 lines across 2 files)</summary>
<p>

<a href='https://github.com/yandex/ch-backup/blob/2b085999d7e3b76f07a8a38ab99071b20d7ccdc9/tests/unit/test_backup_tables.py#L65'>tests/unit/test_backup_tables.py~L65</a>
```diff
     read_bytes_mock = Mock(return_value=creation_statement.encode())
 
     # Backup table
-    with patch("os.path.getmtime", side_effect=mtime), patch(
-        "ch_backup.logic.table.Path", read_bytes=read_bytes_mock
+    with (
+        patch("os.path.getmtime", side_effect=mtime),
+        patch("ch_backup.logic.table.Path", read_bytes=read_bytes_mock),
     ):
         table_backup.backup(
             context,
```
<a href='https://github.com/yandex/ch-backup/blob/2b085999d7e3b76f07a8a38ab99071b20d7ccdc9/tests/unit/test_pipeline.py#L164'>tests/unit/test_pipeline.py~L164</a>
```diff
         forward_file_path, backward_file_name, read_conf, encrypt_conf, write_conf
     )
 
-    with open(original_file_path, "rb") as orig_fobj, open(
-        backward_file_name, "rb"
-    ) as res_fobj:
+    with (
+        open(original_file_path, "rb") as orig_fobj,
+        open(backward_file_name, "rb") as res_fobj,
+    ):
         orig_contents = orig_fobj.read()
         res_contents = res_fobj.read()
 
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 8, column 1
  |
8 | [lint]
  | ^^^^^^
Unknown rule selector: `UP027`
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+110 -86 lines in 16 files in 4 projects; 1 project error; 49 projects unchanged)

<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+34 -30 lines across 4 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/integration/pipeline/test_init_command.py#L98'>tests/integration/pipeline/test_init_command.py~L98</a>
```diff
 
         self.assertEqual(init_process_execute.process.returncode, 0)
 
-        with open(EXPECTED_JENKINS_FILE_PATH, "r") as expected, open(
-            os.path.join(".aws-sam", "pipeline", "generated-files", "Jenkinsfile"), "r"
-        ) as output:
+        with (
+            open(EXPECTED_JENKINS_FILE_PATH, "r") as expected,
+            open(os.path.join(".aws-sam", "pipeline", "generated-files", "Jenkinsfile"), "r") as output,
+        ):
             self.assertEqual(expected.read(), output.read())
 
         # also check the Jenkinsfile is not overridden
```
<a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/unit/commands/samconfig/test_samconfig.py#L1066'>tests/unit/commands/samconfig/test_samconfig.py~L1066</a>
```diff
         }
 
         # NOTE: Because we don't load the full Click BaseCommand here, this is mounted as top-level command
-        with samconfig_parameters(
-            ["start-lambda"], self.scratch_dir, **config_values
-        ) as config_path, tempfile.NamedTemporaryFile() as key_file, tempfile.NamedTemporaryFile() as cert_file:
+        with (
+            samconfig_parameters(["start-lambda"], self.scratch_dir, **config_values) as config_path,
+            tempfile.NamedTemporaryFile() as key_file,
+            tempfile.NamedTemporaryFile() as cert_file,
+        ):
             from samcli.commands.local.start_lambda.cli import cli
 
             LOG.debug(Path(config_path).read_text())
```
<a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/unit/commands/samconfig/test_samconfig.py#L1171'>tests/unit/commands/samconfig/test_samconfig.py~L1171</a>
```diff
         }
 
         # NOTE: Because we don't load the full Click BaseCommand here, this is mounted as top-level command
-        with samconfig_parameters(
-            ["start-lambda"], self.scratch_dir, **config_values
-        ) as config_path, tempfile.NamedTemporaryFile() as key_file, tempfile.NamedTemporaryFile() as cert_file:
+        with (
+            samconfig_parameters(["start-lambda"], self.scratch_dir, **config_values) as config_path,
+            tempfile.NamedTemporaryFile() as key_file,
+            tempfile.NamedTemporaryFile() as cert_file,
+        ):
             from samcli.commands.local.start_lambda.cli import cli
 
             LOG.debug(Path(config_path).read_text())
```
<a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/unit/lib/build_module/test_build_strategy.py#L723'>tests/unit/lib/build_module/test_build_strategy.py~L723</a>
```diff
     def test_will_call_incremental_build_strategy(self, mocked_read, mocked_write, runtime):
         build_definition = FunctionBuildDefinition(runtime, "codeuri", None, "package_type", X86_64, {}, "handler")
         self.build_graph.put_function_build_definition(build_definition, Mock(full_path="function_full_path"))
-        with patch.object(
-            self.build_strategy, "_incremental_build_strategy"
-        ) as patched_incremental_build_strategy, patch.object(
-            self.build_strategy, "_cached_build_strategy"
-        ) as patched_cached_build_strategy:
+        with (
+            patch.object(self.build_strategy, "_incremental_build_strategy") as patched_incremental_build_strategy,
+            patch.object(self.build_strategy, "_cached_build_strategy") as patched_cached_build_strategy,
+        ):
             self.build_strategy.build()
 
             patched_incremental_build_strategy.build_single_function_definition.assert_called_with(build_definition)
```
<a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/unit/lib/build_module/test_build_strategy.py#L741'>tests/unit/lib/build_module/test_build_strategy.py~L741</a>
```diff
     def test_will_call_cached_build_strategy(self, mocked_read, mocked_write, runtime):
         build_definition = FunctionBuildDefinition(runtime, "codeuri", None, "package_type", X86_64, {}, "handler")
         self.build_graph.put_function_build_definition(build_definition, Mock(full_path="function_full_path"))
-        with patch.object(
-            self.build_strategy, "_incremental_build_strategy"
-        ) as patched_incremental_build_strategy, patch.object(
-            self.build_strategy, "_cached_build_strategy"
-        ) as patched_cached_build_strategy:
+        with (
+            patch.object(self.build_strategy, "_incremental_build_strategy") as patched_incremental_build_strategy,
+            patch.object(self.build_strategy, "_cached_build_strategy") as patched_cached_build_strategy,
+        ):
             self.build_strategy.build()
 
             patched_cached_build_strategy.build_single_function_definition.assert_called_with(build_definition)
```
<a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/unit/lib/build_module/test_build_strategy.py#L813'>tests/unit/lib/build_module/test_build_strategy.py~L813</a>
```diff
 
         build_definition = FunctionBuildDefinition(runtime, "codeuri", None, "package_type", X86_64, {}, "handler")
         self.build_graph.put_function_build_definition(build_definition, Mock(full_path="function_full_path"))
-        with patch.object(
-            build_strategy, "_incremental_build_strategy"
-        ) as patched_incremental_build_strategy, patch.object(
-            build_strategy, "_cached_build_strategy"
-        ) as patched_cached_build_strategy:
+        with (
+            patch.object(build_strategy, "_incremental_build_strategy") as patched_incremental_build_strategy,
+            patch.object(build_strategy, "_cached_build_strategy") as patched_cached_build_strategy,
+        ):
             build_strategy.build()
 
             if not use_container:
```
<a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/unit/lib/remote_invoke/test_remote_invoke_executors.py#L79'>tests/unit/lib/remote_invoke/test_remote_invoke_executors.py~L79</a>
```diff
         given_output_format = "text"
         test_execution_info = RemoteInvokeExecutionInfo(given_payload, None, given_parameters, given_output_format)
 
-        with patch.object(self.boto_action_executor, "_execute_action") as patched_execute_action, patch.object(
-            self.boto_action_executor, "_execute_action_file"
-        ) as patched_execute_action_file:
+        with (
+            patch.object(self.boto_action_executor, "_execute_action") as patched_execute_action,
+            patch.object(self.boto_action_executor, "_execute_action_file") as patched_execute_action_file,
+        ):
             given_result = Mock()
             patched_execute_action.return_value = given_result
 
```
<a href='https://github.com/aws/aws-sam-cli/blob/bd3773637c531cbec40bb81c3cac93825fa16810/tests/unit/lib/remote_invoke/test_remote_invoke_executors.py#L96'>tests/unit/lib/remote_invoke/test_remote_invoke_executors.py~L96</a>
```diff
         given_output_format = "json"
     ...*[Comment body truncated]*

---

_Added to milestone `v0.8` by @MichaReiser on 2024-11-11 09:59_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/outdated_version_block.rs`:240 on 2024-11-11 09:59_

I don't think this change here is necessary as it is only an exmaple.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__context_managers_38.py.snap`:1 on 2024-11-11 10:04_

I think we want to keep this test (and the context manager auto detect) to run with Python 3.8. 

You can do this by copying the input source and paste it into a new file in `ruff_python_formatter/resources/test/fixtures/ruff/statement/context_managers_38.py` and then create a `context_managers_38.options.json` next to it containing

```
[
  {
    "target_version": "py38"
  },
  {
    "target_version": "py39"
  }
]

```

---

_@MichaReiser reviewed on 2024-11-11 10:05_

Thanks. This looks good to me. 

I think we want to preserve two formatter tests for now and there's one change that I think we can revert.

We have to wait merging this until our next minor release

---

_Label `configuration` added by @MichaReiser on 2024-11-18 11:23_

---

_Review requested from @carljm by @MichaReiser on 2024-11-18 12:37_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-18 12:37_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-18 12:37_

---

_@MichaReiser approved on 2024-11-18 13:08_

I updated the black test import script to allow overriding the test options. I also removed black tests that no longer exist upstream (because they were moved/renamed). I removed them because I tried to override the settings for them and that didn't work... because they no longer exist.

---

_Comment by @MichaReiser on 2024-11-18 13:13_

What's important for the changelog is to point out that this can result in new violations and formatter changes.

---

_Renamed from "default python version to 3.9" to "Change default for Python version from 3.8 to 3.9" by @MichaReiser on 2024-11-18 13:29_

---

_Merged by @MichaReiser on 2024-11-18 13:29_

---

_Closed by @MichaReiser on 2024-11-18 13:29_

---
