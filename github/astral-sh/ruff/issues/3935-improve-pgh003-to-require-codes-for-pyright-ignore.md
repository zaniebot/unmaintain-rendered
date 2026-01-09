---
number: 3935
title: "Improve `PGH003` to require codes for `# pyright: ignore`"
type: issue
state: closed
author: bryanforbes
labels:
  - good first issue
  - rule
assignees: []
created_at: 2023-04-11T16:53:28Z
updated_at: 2023-04-12T03:10:31Z
url: https://github.com/astral-sh/ruff/issues/3935
synced_at: 2026-01-07T13:12:14-06:00
---

# Improve `PGH003` to require codes for `# pyright: ignore`

---

_Issue opened by @bryanforbes on 2023-04-11 16:53_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Ruff version: 0.0.261

Given the following configuration:

```toml
[tool.pyright]
typeCheckingMode = "strict"

[tool.ruff]
select = ["PGH003"]
```

And the following file `test.py`:

```python
from __future__ import annotations


class One:
    def do_something(self, a: int) -> None:
        ...


class Two(One):
    def do_something(self, a: str) -> None:  # pyright: ignore
        ...
```

No error is generated for line 10. It would be nice if `PGH003` checked for `# pyright: ignore` without codes.

---

_Label `rule` added by @charliermarsh on 2023-04-11 20:27_

---

_Comment by @charliermarsh on 2023-04-11 20:27_

Agreed.

---

_Label `good first issue` added by @charliermarsh on 2023-04-11 20:27_

---

_Referenced in [astral-sh/ruff#3941](../../astral-sh/ruff/pulls/3941.md) on 2023-04-12 02:51_

---

_Comment by @Avasam on 2023-04-12 02:55_

+1
Note that not all Pyright issues have an associated code, but I've only landed on such valid use-cases in some very rare stub-only edge cases. (like making a protocol from non-protocol bases, there's a couple examples in typeshed)
At that point you could do `# pyright: ignore # noqa: PGH003`

---

_Closed by @charliermarsh on 2023-04-12 03:10_

---
