---
number: 13037
title: A004 Preview Mode ExceptionGroup import in pre-3.11
type: issue
state: closed
author: CoolCat467
labels:
  - rule
  - accepted
  - preview
assignees: []
created_at: 2024-08-21T18:50:40Z
updated_at: 2024-09-01T17:03:45Z
url: https://github.com/astral-sh/ruff/issues/13037
synced_at: 2026-01-10T01:22:53Z
---

# A004 Preview Mode ExceptionGroup import in pre-3.11

---

_Issue opened by @CoolCat467 on 2024-08-21 18:50_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Getting `A004 Import BaseExceptionGroup is shadowing a Python builtin` and `ExceptionGroup` errors raised in this case:
```python
if sys.version_info < (3, 11):
    from exceptiongroup import BaseExceptionGroup, ExceptionGroup
```
I would argue that this should not be the case. `BaseExceptionGroup` and `ExceptionGroup` do not exist in python versions prior to 3.11, and using the `exceptiongroup` package is commonly used for backwards compatibility.

Using ruff preview mode v0.6.1

---

_Comment by @MichaReiser on 2024-08-22 07:50_

This makes sense to me, but we should only allow the use of `BaseExceptionGroup` and `ExceptionGroup` when inside a `sys.version_info < (3.11)` branch. I'm not sure if we have the necessary infrastructure today. wdyt @AlexWaygood ?

---

_Label `rule` added by @MichaReiser on 2024-08-22 07:50_

---

_Comment by @AlexWaygood on 2024-08-22 15:37_

I'm quite hesistant to add the infrastructure to Ruff to support detecting which `sys.version_info` branch we're in. Generalised `sys.version_info` parsing would be tricky (though we'll have to implement it for red-knot to comply with PEP 484), and I'm not sure it's actually needed in this case. For this case, I think we could fix the issue by making the `is_python_builtin()` function smarter:
- The rule calls `shadows_builtin()` [here](https://github.com/astral-sh/ruff/blob/2edd32aa31446c81c245beb1230de26220a7d7e8/crates/ruff_linter/src/rules/flake8_builtins/rules/builtin_import_shadowing.rs#L37-L41).
- `shadows_builtin()` calls `is_python_builtin()` [here](https://github.com/astral-sh/ruff/blob/2edd32aa31446c81c245beb1230de26220a7d7e8/crates/ruff_linter/src/rules/flake8_builtins/helpers.rs#L9)
- `is_python_builtin()` is defined [here](https://github.com/astral-sh/ruff/blob/2edd32aa31446c81c245beb1230de26220a7d7e8/crates/ruff_python_stdlib/src/builtins.rs#L192). Unlike other [similar functions](https://github.com/astral-sh/ruff/blob/2edd32aa31446c81c245beb1230de26220a7d7e8/crates/ruff_python_stdlib/src/sys/known_stdlib.rs#L3-L585) in the `ruff_python_stdlib` crate, it assumes that all Python builtins have always been Python builtins; it doesn't take account of the fact that some builtins were added in newer Python versions. If we added a `minor_version` parameter, similar to `ruff_python_stdlib::sys::is_builtin_module` or `ruff_python_stdlib::sys::is_known_standard_library`, then we could take account of the `--target-version` the user has provided when deciding whether something is a Python builtin or not. Since @CoolCat467 clearly supports running Python <3.11 on their code, they'll probably be running Ruff with `--target-version py310` or lower.

---

_Comment by @CoolCat467 on 2024-08-22 18:59_

I think that using `--target-version` is a good solution

---

_Label `accepted` added by @MichaReiser on 2024-08-22 19:31_

---

_Label `preview` added by @dhruvmanila on 2024-08-23 04:58_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-30 22:23_

---

_Referenced in [astral-sh/ruff#13172](../../astral-sh/ruff/pulls/13172.md) on 2024-08-30 23:00_

---

_Closed by @charliermarsh on 2024-09-01 17:03_

---

_Closed by @charliermarsh on 2024-09-01 17:03_

---
