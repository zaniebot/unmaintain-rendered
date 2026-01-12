```yaml
number: 2853
title: "`RUF100` strips valid `noqa` comments when file contains syntax error"
type: issue
state: closed
author: stinodego
labels:
  - bug
assignees: []
created_at: 2023-02-13T11:51:55Z
updated_at: 2023-02-13T15:37:57Z
url: https://github.com/astral-sh/ruff/issues/2853
synced_at: 2026-01-12T15:54:43Z
```

# `RUF100` strips valid `noqa` comments when file contains syntax error

---

_@stinodego_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

See the example below.

### Config

```toml
[tool.ruff]
fix = true
select = ["W", "RUF"]

[tool.ruff.pycodestyle]
max-doc-length = 88
```

### Code

```
class Repro:
    def foo(self) -> None:
        """Docstring way toooooooooooooooooooooooooooooooooooooooooooooooooooooooooooo long."""  # noqa: W505
        ...

@cool_decorator()  # Syntax error due to indentation
    def bar(self) -> None:
        ...
```

Running `ruff .` results in the `noqa` comment erroneously being stripped.

### Version
Latest (`0.0.246`)

---

_Renamed from "RUF100 strips valid `noqa` comments when file contains syntax error" to "`RUF100` strips valid `noqa` comments when file contains syntax error" by @stinodego on 2023-02-13 11:53_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-13 13:55_

---

_Comment by @charliermarsh on 2023-02-13 13:55_

Ah good call.

---

_Label `bug` added by @charliermarsh on 2023-02-13 13:55_

---

_Closed by @charliermarsh on 2023-02-13 15:37_

---
