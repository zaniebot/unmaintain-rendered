```yaml
number: 11262
title: "[`pyflakes`] Treat `if TYPE_CHECKING` blocks like stub files for F821"
type: pull_request
state: open
author: AlexWaygood
labels:
  - bug
  - linter
assignees: []
draft: true
base: main
head: type-checking-f821
created_at: 2024-05-03T12:45:09Z
updated_at: 2024-05-11T14:54:55Z
url: https://github.com/astral-sh/ruff/pull/11262
synced_at: 2026-01-12T15:55:37Z
```

# [`pyflakes`] Treat `if TYPE_CHECKING` blocks like stub files for F821

---

_@AlexWaygood_

## Summary

Fixes #11245.

`if TYPE_CHECKING` blocks, like stub files, are never executed at runtime -- so I think it makes sense to give them stub-file semantics when it comes to whether forward references are or aren't permitted. Currently we allow forward references in stub files on the r.h.s. of type alias definitions and in class bases; this PR implements the same logic for `if TYPE_CHECKING` blocks.

## Test Plan

`cargo test` / `cargo insta review`. Two fixtures have been extended.


---

_Review requested from @carljm by @AlexWaygood on 2024-05-03 12:45_

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-05-03 12:45_

---

_Renamed from "[`pyflakes`] Treat `if `TYPE_CHECKING` blocks like stub files for F821" to "[`pyflakes`] Treat `if TYPE_CHECKING` blocks like stub files for F821" by @AlexWaygood on 2024-05-03 12:45_

---

_Label `bug` added by @AlexWaygood on 2024-05-03 12:45_

---

_Label `linter` added by @AlexWaygood on 2024-05-03 12:45_

---

_Comment by @AlexWaygood on 2024-05-03 12:48_

Type definitions created in `if TYPE_CHECKING` blocks obviously won't be available at runtime. If you want to use them elsewhere in the file, you'll either need to quote the definition or (if you only need to use it in an annotation context) put `from __future__ import annotations` at the top of the file. Possibly we could have an additional lint checking that the user does that, but I think it's out of scope of this PR.

---

_Comment by @github-actions[bot] on 2024-05-03 12:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+47 -0 violations, +0 -0 fixes in 7 projects; 37 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+28 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/base_executor.py#L53'>airflow/executors/base_executor.py:53:19:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/base_executor.py#L59'>airflow/executors/base_executor.py:59:30:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/base_executor.py#L59'>airflow/executors/base_executor.py:59:54:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/base_executor.py#L63'>airflow/executors/base_executor.py:63:28:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/base_executor.py#L63'>airflow/executors/base_executor.py:63:34:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/base_executor.py#L66'>airflow/executors/base_executor.py:66:17:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/base_executor.py#L66'>airflow/executors/base_executor.py:66:53:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/base_executor.py#L66'>airflow/executors/base_executor.py:66:68:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/local_executor.py#L56'>airflow/executors/local_executor.py:56:24:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/local_executor.py#L56'>airflow/executors/local_executor.py:56:30:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/local_executor.py#L56'>airflow/executors/local_executor.py:56:57:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/models/mappedoperator.py#L90'>airflow/models/mappedoperator.py:90:39:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/models/mappedoperator.py#L90'>airflow/models/mappedoperator.py:90:76:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/celery/executors/celery_executor.py#L98'>airflow/providers/celery/executors/celery_executor.py:98:28:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/celery/executors/celery_executor.py#L98'>airflow/providers/celery/executors/celery_executor.py:98:64:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/celery/executors/celery_executor_utils.py#L62'>airflow/providers/celery/executors/celery_executor_utils.py:62:28:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/celery/executors/celery_executor_utils.py#L62'>airflow/providers/celery/executors/celery_executor_utils.py:62:64:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py#L28'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py:28:25:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py#L28'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py:28:66:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py#L31'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py:31:29:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py#L31'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py:31:52:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py#L31'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py:31:61:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py#L34'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py:34:27:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py#L34'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py:34:43:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py#L34'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py:34:52:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py#L34'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py:34:84:</a> UP006 Use `dict` instead of `Dict` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/microsoft/azure/hooks/cosmos.py#L47'>airflow/providers/microsoft/azure/hooks/cosmos.py:47:24:</a> UP007 Use `X | Y` for type annotations
... 1 additional changes omitted for rule UP007
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/microsoft/azure/hooks/cosmos.py#L47'>airflow/providers/microsoft/azure/hooks/cosmos.py:47:35:</a> UP006 Use `list` instead of `List` for type annotation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L98'>src/bokeh/core/has_props.py:98:25:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/events.py#L122'>src/bokeh/document/events.py:122:26:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L87'>src/bokeh/models/sources.py:87:37:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L89'>src/bokeh/models/sources.py:89:24:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L89'>src/bokeh/models/sources.py:89:48:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/contour.py#L42'>src/bokeh/plotting/contour.py:42:31:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/contour.py#L43'>src/bokeh/plotting/contour.py:43:40:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/transform.py#L71'>src/bokeh/transform.py:71:26:</a> UP007 Use `X | Y` for type annotations
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/5e86e61c29234cb4fe0cc79410a352e8c225b0b9/latch/ldata/_transfer/upload.py#L31'>latch/ldata/_transfer/upload.py:31:39:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/latchbio/latch/blob/5e86e61c29234cb4fe0cc79410a352e8c225b0b9/latch/ldata/_transfer/upload.py#L32'>latch/ldata/_transfer/upload.py:32:42:</a> UP007 Use `X | Y` for type annotations
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/98d57d9547203fa3b5676ef6960d639989295cf8/cibuildwheel/typing.py#L19'>cibuildwheel/typing.py:19:17:</a> UP007 Use `X | Y` for type annotations
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/python/mypy/blob/fb31409b392c5533b25173705d62ed385ee39cfb/mypyc/build.py#L58'>mypyc/build.py:58:32:</a> UP007 Use `X | Y` for type annotations
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/repositories/link_sources/base.py#L26'>src/poetry/repositories/link_sources/base.py:26:17:</a> UP006 Use `collections.defaultdict` instead of `DefaultDict` for type annotation
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/repositories/link_sources/base.py#L26'>src/poetry/repositories/link_sources/base.py:26:45:</a> UP006 Use `collections.defaultdict` instead of `DefaultDict` for type annotation
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/repositories/link_sources/base.py#L26'>src/poetry/repositories/link_sources/base.py:26:66:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/inspection/test_lazy_wheel.py#L31'>tests/inspection/test_lazy_wheel.py:31:25:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/inspection/test_lazy_wheel.py#L31'>tests/inspection/test_lazy_wheel.py:31:36:</a> UP006 Use `dict` instead of `Dict` for type annotation
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/inspection/test_lazy_wheel.py#L33'>tests/inspection/test_lazy_wheel.py:33:33:</a> UP006 Use `dict` instead of `Dict` for type annotation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6ed5a28fc85e621b03d984011d17def888ee0db/src/scikit_build_core/_compat/importlib/metadata.py#L20'>src/scikit_build_core/_compat/importlib/metadata.py:20:23:</a> UP006 Use `list` instead of `typing.List` for type annotation
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP007 | 27 | 27 | 0 | 0 | 0 |
| UP006 | 20 | 20 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+47 -0 violations, +0 -0 fixes in 7 projects; 37 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+28 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/base_executor.py#L53'>airflow/executors/base_executor.py:53:19:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/base_executor.py#L59'>airflow/executors/base_executor.py:59:30:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/base_executor.py#L59'>airflow/executors/base_executor.py:59:54:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/base_executor.py#L63'>airflow/executors/base_executor.py:63:28:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/base_executor.py#L63'>airflow/executors/base_executor.py:63:34:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/base_executor.py#L66'>airflow/executors/base_executor.py:66:17:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/base_executor.py#L66'>airflow/executors/base_executor.py:66:53:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/base_executor.py#L66'>airflow/executors/base_executor.py:66:68:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/local_executor.py#L56'>airflow/executors/local_executor.py:56:24:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/local_executor.py#L56'>airflow/executors/local_executor.py:56:30:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/executors/local_executor.py#L56'>airflow/executors/local_executor.py:56:57:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/models/mappedoperator.py#L90'>airflow/models/mappedoperator.py:90:39:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/models/mappedoperator.py#L90'>airflow/models/mappedoperator.py:90:76:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/celery/executors/celery_executor.py#L98'>airflow/providers/celery/executors/celery_executor.py:98:28:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/celery/executors/celery_executor.py#L98'>airflow/providers/celery/executors/celery_executor.py:98:64:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/celery/executors/celery_executor_utils.py#L62'>airflow/providers/celery/executors/celery_executor_utils.py:62:28:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/celery/executors/celery_executor_utils.py#L62'>airflow/providers/celery/executors/celery_executor_utils.py:62:64:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py#L28'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py:28:25:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py#L28'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py:28:66:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py#L31'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py:31:29:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py#L31'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py:31:52:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py#L31'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py:31:61:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py#L34'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py:34:27:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py#L34'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py:34:43:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py#L34'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py:34:52:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py#L34'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_types.py:34:84:</a> UP006 Use `dict` instead of `Dict` for type annotation
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/microsoft/azure/hooks/cosmos.py#L47'>airflow/providers/microsoft/azure/hooks/cosmos.py:47:24:</a> UP007 Use `X | Y` for type annotations
... 1 additional changes omitted for rule UP007
+ <a href='https://github.com/apache/airflow/blob/a61f393ec4361499fcef9f2854668db85b852ec0/airflow/providers/microsoft/azure/hooks/cosmos.py#L47'>airflow/providers/microsoft/azure/hooks/cosmos.py:47:35:</a> UP006 Use `list` instead of `List` for type annotation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L98'>src/bokeh/core/has_props.py:98:25:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/events.py#L122'>src/bokeh/document/events.py:122:26:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L87'>src/bokeh/models/sources.py:87:37:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L89'>src/bokeh/models/sources.py:89:24:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L89'>src/bokeh/models/sources.py:89:48:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/contour.py#L42'>src/bokeh/plotting/contour.py:42:31:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/contour.py#L43'>src/bokeh/plotting/contour.py:43:40:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/transform.py#L71'>src/bokeh/transform.py:71:26:</a> UP007 Use `X | Y` for type annotations
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/5e86e61c29234cb4fe0cc79410a352e8c225b0b9/latch/ldata/_transfer/upload.py#L31'>latch/ldata/_transfer/upload.py:31:39:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/latchbio/latch/blob/5e86e61c29234cb4fe0cc79410a352e8c225b0b9/latch/ldata/_transfer/upload.py#L32'>latch/ldata/_transfer/upload.py:32:42:</a> UP007 Use `X | Y` for type annotations
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/98d57d9547203fa3b5676ef6960d639989295cf8/cibuildwheel/typing.py#L19'>cibuildwheel/typing.py:19:17:</a> UP007 Use `X | Y` for type annotations
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/mypy/blob/fb31409b392c5533b25173705d62ed385ee39cfb/mypyc/build.py#L58'>mypyc/build.py:58:32:</a> UP007 Use `X | Y` for type annotations
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/repositories/link_sources/base.py#L26'>src/poetry/repositories/link_sources/base.py:26:17:</a> UP006 Use `collections.defaultdict` instead of `DefaultDict` for type annotation
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/repositories/link_sources/base.py#L26'>src/poetry/repositories/link_sources/base.py:26:45:</a> UP006 Use `collections.defaultdict` instead of `DefaultDict` for type annotation
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/repositories/link_sources/base.py#L26'>src/poetry/repositories/link_sources/base.py:26:66:</a> UP006 Use `list` instead of `List` for type annotation
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/inspection/test_lazy_wheel.py#L31'>tests/inspection/test_lazy_wheel.py:31:25:</a> UP006 Use `tuple` instead of `Tuple` for type annotation
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/inspection/test_lazy_wheel.py#L31'>tests/inspection/test_lazy_wheel.py:31:36:</a> UP006 Use `dict` instead of `Dict` for type annotation
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/inspection/test_lazy_wheel.py#L33'>tests/inspection/test_lazy_wheel.py:33:33:</a> UP006 Use `dict` instead of `Dict` for type annotation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6ed5a28fc85e621b03d984011d17def888ee0db/src/scikit_build_core/_compat/importlib/metadata.py#L20'>src/scikit_build_core/_compat/importlib/metadata.py:20:23:</a> UP006 Use `list` instead of `typing.List` for type annotation
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP007 | 27 | 27 | 0 | 0 | 0 |
| UP006 | 20 | 20 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @carljm on `crates/ruff_linter/resources/test/fixtures/pyflakes/F821_27.py`:52 on 2024-05-03 14:49_

This file says at the top that it is for "Tests for constructs allowed when `__future__` annotations are enabled but not otherwise"""

The added tests here are all independent of `__future__` annotations.

Should these tests not be in this file, or should the docstring of this file be updated?

I can see that we may want to just for completeness test various behaviors with and without `__future__.annotations`, even if they shouldn't be affected by it.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/model.rs`:1591 on 2024-05-03 18:41_

Why not use `in_typing_only_block` that you added in this PR? Currently it's only used in a debug assertion, but it seems like it's equivalent to this line?

---

_Review comment by @carljm on `crates/ruff_linter/src/checkers/ast/mod.rs`:2011 on 2024-05-03 18:42_

Curious if you've looked at other uses of `future_annotations_or_stub` -- is there any use of it that we _wouldn't_ want to apply also in a type checking block?

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/model.rs`:1596 on 2024-05-03 18:44_

looks like there isn't a `self.future_annotations()`, maybe there should be?

---

_@carljm approved on 2024-05-03 18:44_

Looks good!

---

_@AlexWaygood reviewed on 2024-05-03 20:33_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:1591 on 2024-05-03 20:33_

oops, I think I just added the `in_typing_only_block()` after I added this bit

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:1596 on 2024-05-03 20:40_

I worry that it's a *bit* of a footgun, as there aren't really very many situations where you want to be able to differentiate between `.py` files with `from __future__ import annotations` and stub files, and it's pretty easy to forget that you need to use `self.future_annotations_or_stub()` rather than `self.future_annotations()`. It's for those reasons that I... removed our previous `self.future_annotations()` method in https://github.com/astral-sh/ruff/commit/1dc93107dce911a535bdd9ac0d918f8052b7e326 😄 but, I'll add it back

---

_@AlexWaygood reviewed on 2024-05-03 20:40_

---

_Converted to draft by @AlexWaygood on 2024-05-03 22:14_

---

_Marked ready for review by @AlexWaygood on 2024-05-04 17:33_

---

_@AlexWaygood reviewed on 2024-05-05 07:50_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/mod.rs`:2068 on 2024-05-05 07:50_

I think honestly PYI020 is a better fit for stub files and `if TYPE_CHECKING` blocks, so the logic here should probably be changed. But for now I'm just maintaining compatibility with previous versions of Ruff, which emitted this lint on annotations in stub files as well as annotations in `.py` files. Abruptly reducing the scope of the rule like that would maybe be slightly backwards-incompatible; it at least should be considered in a separate PR.

---

_Converted to draft by @AlexWaygood on 2024-05-05 08:24_

---

_@Skylion007 reviewed on 2024-05-11 14:50_

---

_Review comment by @Skylion007 on `crates/ruff_linter/resources/test/fixtures/pyflakes/F821_26.py`:46 on 2024-05-11 14:50_

Can we a test for if there is code in an else block after the TYPE_CHECKING? That shouldn't be treated a stub, right?

---

_@AlexWaygood reviewed on 2024-05-11 14:54_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyflakes/F821_26.py`:46 on 2024-05-11 14:54_

Sure, I can add a test for that. 

---
