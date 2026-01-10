---
number: 8883
title: "Feature request: PLW1514 autofix"
type: issue
state: closed
author: Avasam
labels:
  - fixes
assignees: []
created_at: 2023-11-28T21:37:12Z
updated_at: 2023-12-01T19:48:56Z
url: https://github.com/astral-sh/ruff/issues/8883
synced_at: 2026-01-10T01:22:48Z
---

# Feature request: PLW1514 autofix

---

_Issue opened by @Avasam on 2023-11-28 21:37_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Can an autofix be created for the preview rule `PLW1514` ? Even if it's "unsafe". Updating hundreds of instances manually is quite tedious.

In Python 3.10+ this would autofix to `encoding="locale"`, in Python <=3.9 this would autofix to `encoding=locale.getpreferredencoding(False)` (see: https://peps.python.org/pep-0597/). Search & replacing those if I want a specific encoding is easier. An option to specify prefered encoding can also be set (which could even be re-used by a new rule that ensures consistent encoding is used with desired value, like `"utf-8"`)

---

_Renamed from "PLW1514 autofix" to "Feature request: PLW1514 autofix" by @Avasam on 2023-11-28 21:37_

---

_Label `autofix` added by @charliermarsh on 2023-11-28 21:58_

---

_Referenced in [astral-sh/ruff#8928](../../astral-sh/ruff/pulls/8928.md) on 2023-11-30 18:39_

---

_Comment by @qdegraaf on 2023-11-30 20:06_

While I'm working on the general utility mentioned in https://github.com/astral-sh/ruff/pull/8928#discussion_r1411171679, if anyone wants to do the autofix separately in the meantime, feel free to jump in. 

---

_Closed by @charliermarsh on 2023-12-01 18:23_

---

_Comment by @Avasam on 2023-12-01 19:48_

Thank you! Will give it a go next release.

---

_Referenced in [astral-sh/ruff#12069](../../astral-sh/ruff/issues/12069.md) on 2024-06-27 12:11_

---
