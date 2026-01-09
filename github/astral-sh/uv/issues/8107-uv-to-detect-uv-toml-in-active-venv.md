---
number: 8107
title: uv to detect uv.toml in active venv
type: issue
state: open
author: Reclocco
labels:
  - configuration
assignees: []
created_at: 2024-10-10T20:02:10Z
updated_at: 2024-10-13T13:22:10Z
url: https://github.com/astral-sh/uv/issues/8107
synced_at: 2026-01-07T13:12:17-06:00
---

# uv to detect uv.toml in active venv

---

_Issue opened by @Reclocco on 2024-10-10 20:02_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

With pip you can create custom config per venv (custom index-url etc) with pip.conf files. I think it's impossible with uv as it doesn't look for the `uv.toml` file within the active venv.

This could be helpful with managing these configs if one needs to work with / test different scenarios without modifying repo code or constantly changing `uv.toml` that is being detected (IIRC it works with closest to cwd working upwards)

---

_Comment by @charliermarsh on 2024-10-12 14:58_

To clarify, you want to be able to put the `uv.toml` within the virtual environment directory? Like at `.venv/uv.toml` or similar?

---

_Label `configuration` added by @charliermarsh on 2024-10-12 14:58_

---

_Comment by @Reclocco on 2024-10-13 13:22_

Yes

---

_Referenced in [astral-sh/uv#9935](../../astral-sh/uv/pulls/9935.md) on 2024-12-16 13:38_

---
