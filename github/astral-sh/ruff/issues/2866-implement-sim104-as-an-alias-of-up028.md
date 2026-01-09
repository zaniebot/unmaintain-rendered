---
number: 2866
title: Implement SIM104 as an alias of UP028
type: issue
state: closed
author: Skylion007
labels: []
assignees: []
created_at: 2023-02-13T16:33:15Z
updated_at: 2023-02-13T17:39:35Z
url: https://github.com/astral-sh/ruff/issues/2866
synced_at: 2026-01-07T13:12:14-06:00
---

# Implement SIM104 as an alias of UP028

---

_Issue opened by @Skylion007 on 2023-02-13 16:33_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
I noticed SIM104 is unimplemented, but UP028 is and they are exactly the same rule. It would be great if an alias could be added to enable the rule for SIM104.

---

_Renamed from "Implement SIM104, its' just UP028" to "Implement SIM104 as an alias of UP028" by @Skylion007 on 2023-02-13 16:37_

---

_Comment by @sbrugman on 2023-02-13 17:10_

Thanks for flagging. These cases are tracked here: https://github.com/charliermarsh/ruff/issues/2186
Solution in progress: https://github.com/charliermarsh/ruff/pull/2517

---

_Comment by @charliermarsh on 2023-02-13 17:39_

Thanks, commented there!

---

_Closed by @charliermarsh on 2023-02-13 17:39_

---
