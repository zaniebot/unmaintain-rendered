```yaml
number: 5641
title: "pyupgrade in pyi files doesn't respect target-version"
type: issue
state: closed
author: trim21
labels: []
assignees: []
created_at: 2023-07-10T11:27:06Z
updated_at: 2023-07-10T13:40:42Z
url: https://github.com/astral-sh/ruff/issues/5641
synced_at: 2026-01-12T15:54:45Z
```

# pyupgrade in pyi files doesn't respect target-version

---

_@trim21_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

rules like `UP007 [*] Use X | Y for type annotations` doesn't respect target-version in type stub (pyi files)

pyi type `Dict[...]` are rewrite to `dict[...]` with `target-version='py37'` anyway

---

_Comment by @charliermarsh on 2023-07-10 13:40_

Thanks for filing! This is intentional. Since stub files are never executed by the interpreter, and only exist to support static analysis, `.pyi` files are always treated as if `from __future__ import annotations` is enabled, i.e., you can always use features like built-in generics and the pipe union syntax: https://typing.readthedocs.io/en/latest/source/stubs.html#syntax. So, we respect that behavior and allow rewriting regardless of the current Python version.

---

_Closed by @charliermarsh on 2023-07-10 13:40_

---
