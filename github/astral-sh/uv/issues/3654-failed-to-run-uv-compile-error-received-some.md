---
number: 3654
title: " Failed to run uv compile error: Received some unexpected HTML Caused by: Unexpected fragment (expected `#sha256=...` or similar)"
type: issue
state: closed
author: njmaeff
labels:
  - compatibility
assignees: []
created_at: 2024-05-18T18:17:50Z
updated_at: 2024-05-20T21:09:43Z
url: https://github.com/astral-sh/uv/issues/3654
synced_at: 2026-01-07T13:12:17-06:00
---

#  Failed to run uv compile error: Received some unexpected HTML Caused by: Unexpected fragment (expected `#sha256=...` or similar)

---

_Issue opened by @njmaeff on 2024-05-18 18:17_

The [jetbrains space package index](https://pypi.pkg.jetbrains.space/) uses a URL encoded sha256. Should this be handled by the package manager?

URL Encoded: `sha256%3D4095ada29e51070f7d199a0a5bdf5c8d8e238e03f0bf4dcc02571e78c9ae800d`
Decoded: `sha256=4095ada29e51070f7d199a0a5bdf5c8d8e238e03f0bf4dcc02571e78c9ae800d`

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
rye 0.33.0
commit: 0.33.0 (58523f69f 2024-04-24)
platform: linux (x86_64)
self-python: cpython@3.12.3
symlink support: true
uv enabled: true


---

_Comment by @charliermarsh on 2024-05-18 21:15_

I'll take a look.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-18 21:15_

---

_Label `compatibility` added by @charliermarsh on 2024-05-18 21:15_

---

_Referenced in [astral-sh/uv#3655](../../astral-sh/uv/pulls/3655.md) on 2024-05-19 02:11_

---

_Closed by @charliermarsh on 2024-05-19 02:19_

---

_Comment by @charliermarsh on 2024-05-19 02:20_

Should work in the next release.

---

_Comment by @charliermarsh on 2024-05-20 21:09_

Out now in [v0.1.45](https://github.com/astral-sh/uv/releases/tag/0.1.45).

---
