```yaml
number: 14221
title: Warn if anything marked as deprecated is used
type: issue
state: open
author: mamekoro
labels:
  - rule
  - type-inference
assignees: []
created_at: 2024-11-09T07:09:46Z
updated_at: 2025-05-28T09:38:15Z
url: https://github.com/astral-sh/ruff/issues/14221
synced_at: 2026-01-12T15:54:53Z
```

# Warn if anything marked as deprecated is used

---

_@mamekoro_

Python 3.13 introduced a decorator called [`@warnings.deprecated`](https://docs.python.org/3/library/warnings.html#warnings.deprecated) that marks classes, functions and methods as deprecated. For older versions of Python, [`@deprecated.deprecated`](https://pypi.org/project/Deprecated/) is available as an alternative.

Would you consider adding the functionality to warn of the use of objects marked as deprecated by these decorators? In other words, a feature like that provided by [memestra](https://github.com/QuantStack/memestra).



---

_Comment by @sbrugman on 2024-11-09 10:23_

In general such a rule could be a great addition for library developers that want to do a quick check of expired deprecations before a release. Warning users of such functions would require multi-file analysis.

In practice, many libraries define their own deprecation decorator with additional functionality such as version and expiry information. My impression is that for popular packages it's more common to use a custom decorator than that to use the standard library (`warnings`) or a third-party (`deprecated`).

Since we cannot detect these custom implementations reliably, we could consider letting the user configure fully qualified names of the deprecation functions, like https://docs.astral.sh/ruff/settings/#lint_flake8-boolean-trap_extend-allowed-calls. This would make this rule effective for those users too.

Examples:
- https://github.com/astropy/astropy/blob/main/astropy/cosmology/flrw/base.py#L111
- https://github.com/apache/airflow/blob/main/providers/src/airflow/providers/google/cloud/operators/mlengine.py#L82
- https://github.com/flet-dev/flet/blob/main/sdk/python/packages/flet/src/flet/core/page.py#L726
- https://github.com/langchain-ai/langchain/blob/master/libs/core/langchain_core/tracers/schemas.py#L31
- https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/eager/profiler.py#L62
- https://github.com/apache/superset/blob/master/superset/views/base.py#L375
- https://github.com/conda/conda/blob/main/conda/exports.py#L112
- https://github.com/ray-project/ray/blob/master/python/ray/runtime_context.py#L22

---

_Comment by @MichaReiser on 2024-11-10 09:33_

That makes sense but it does require multifile analysis. For now, you could consider using https://docs.astral.sh/ruff/rules/banned-api/

---

_Label `rule` added by @MichaReiser on 2024-11-10 09:33_

---

_Label `type-inference` added by @MichaReiser on 2024-11-10 09:33_

---

_Comment by @Andrej730 on 2025-05-28 09:38_

Maybe it would make more sense as part of `ty` than `ruff`? 

`ty`, as a type checker, would analyze all related files just by it's nature, when `ruff`, as a linter, is more focused on analyzing each file separately, without considering the entire context.

---
