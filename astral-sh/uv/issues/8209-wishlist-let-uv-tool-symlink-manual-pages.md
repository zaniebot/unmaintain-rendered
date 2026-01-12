```yaml
number: 8209
title: "Wishlist: let uv tool symlink manual pages"
type: issue
state: closed
author: callegar
labels:
  - duplicate
assignees: []
created_at: 2024-10-15T10:28:11Z
updated_at: 2024-10-15T13:54:03Z
url: https://github.com/astral-sh/uv/issues/8209
synced_at: 2026-01-12T15:59:22Z
```

# Wishlist: let uv tool symlink manual pages

---

_@callegar_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

For uv tool to be at feature parity with `pipx`, I think it would be good to symlink not just the executables themselves into `~/.local /bin`, but to also offer the possibility to link their manual pages (if available) into `~/.local/share/man`.

A good example is the `sympy` package that offers the `isympy` tools with a manual page.

See also https://github.com/pypa/pipx/issues/857 and https://github.com/pypa/pipx/pull/1047

---

_Comment by @zanieb on 2024-10-15 13:53_

Duplicate of https://github.com/astral-sh/uv/issues/4731

---

_Closed by @zanieb on 2024-10-15 13:53_

---

_Label `duplicate` added by @zanieb on 2024-10-15 13:54_

---
