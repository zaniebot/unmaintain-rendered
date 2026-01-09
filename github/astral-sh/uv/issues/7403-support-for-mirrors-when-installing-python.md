---
number: 7403
title: Support for mirrors when installing Python versions
type: issue
state: closed
author: dillydill123
labels:
  - question
assignees: []
created_at: 2024-09-15T02:53:37Z
updated_at: 2024-09-15T13:12:40Z
url: https://github.com/astral-sh/uv/issues/7403
synced_at: 2026-01-07T13:12:17-06:00
---

# Support for mirrors when installing Python versions

---

_Issue opened by @dillydill123 on 2024-09-15 02:53_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Apologies if this was brought up before or exists as a feature already, but I could not find either in documentation or existing issues.

It would be useful to have the ability to pass a flag or environment variable to tell `uv` where to download pre-built python interpreters as opposed to always choosing `python-build-standalone`. This would be useful for machines which may not access to the internet. In this case a mirror could be supplied.

---

_Comment by @Werni2A on 2024-09-15 07:53_

The `UV_PYTHON_INSTALL_MIRROR` environment variable may help you in this case. Search for the term in the official [docs](https://docs.astral.sh/uv/configuration/environment/) for more details.

---

_Comment by @zanieb on 2024-09-15 13:12_

Yep! We support this via that variable and `UV_PYPY_INSTALL_MIRROR`.

---

_Closed by @zanieb on 2024-09-15 13:12_

---

_Label `question` added by @zanieb on 2024-09-15 13:12_

---

_Assigned to @zanieb by @zanieb on 2024-09-15 13:12_

---

_Comment by @zanieb on 2024-09-15 13:12_

We can probably call this out in the Python version concept doc if we don't already.

---
