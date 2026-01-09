---
number: 13446
title: Support requires-python setting from PEP 723
type: issue
state: closed
author: majutsushi
labels: []
assignees: []
created_at: 2024-09-22T05:07:34Z
updated_at: 2024-09-22T05:31:19Z
url: https://github.com/astral-sh/ruff/issues/13446
synced_at: 2026-01-07T13:12:15-06:00
---

# Support requires-python setting from PEP 723

---

_Issue opened by @majutsushi on 2024-09-22 05:07_

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

It would be really helpful for standalone scripts if Ruff would support reading the `requires-pyton` setting from the PEP 723 metadata. Standalone scripts usually don't have a separate project config file, but in some cases they may still want to configure the Python version that Ruff uses.

In my case I ran into this because I was using the `zoneinfo` module that was added to the standard library in 3.9, and I was surprised that Ruff sorted it under third-party imports (see also https://github.com/astral-sh/ruff/issues/6475 and https://github.com/astral-sh/ruff/issues/13354). So it would be great if Ruff could support the metadata comment block for that.

---

_Comment by @majutsushi on 2024-09-22 05:31_

I just noticed that https://github.com/astral-sh/ruff/issues/10457 already exists, so I'll add a comment there instead.

---

_Closed by @majutsushi on 2024-09-22 05:31_

---

_Referenced in [astral-sh/ruff#14258](../../astral-sh/ruff/issues/14258.md) on 2024-11-11 08:06_

---
