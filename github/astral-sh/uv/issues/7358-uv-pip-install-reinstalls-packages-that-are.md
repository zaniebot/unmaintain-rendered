---
number: 7358
title: uv pip install reinstalls packages that are available via --system-site-packages
type: issue
state: closed
author: BubuOT
labels:
  - bug
  - duplicate
assignees: []
created_at: 2024-09-13T11:55:21Z
updated_at: 2024-09-13T12:56:00Z
url: https://github.com/astral-sh/uv/issues/7358
synced_at: 2026-01-07T13:12:17-06:00
---

# uv pip install reinstalls packages that are available via --system-site-packages

---

_Issue opened by @BubuOT on 2024-09-13 11:55_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

When creating a virtualenv (either with `python -m venv --system-site-packages` <dir>` or with `uv venv --system-site-packages` the packages installed in the system python installation are not recognized by uv at all. We then install a wheel into our venv, expecting uv to only download the missing dependencies. uv redownloeads and reinstalls a lot of additional packages which are already available in the system though. pip correctly installs only the missing packages.

Our targets are embedded devices which ship a python installation with most dependencies installed and updated via the firmware image, for custom deployments and prototyping we'd like an easy way to "overlay" the available system packages with newer versions or additional dependencies. (Also this happens via mobile network, so downloading an additional maybe 30MB of dependencies could be a big deal, depending on the connection quality)

Platform: linux-aarch64, uv 0.4.9

---

_Comment by @charliermarsh on 2024-09-13 12:55_

We don't really support `--system-site-packages` -- it's the same as https://github.com/astral-sh/uv/issues/4466 etc. Instead you'd need to use `uv pip install --system` and other commands, or feel free to comment on https://github.com/astral-sh/uv/issues/4466.

---

_Closed by @charliermarsh on 2024-09-13 12:55_

---

_Label `duplicate` added by @charliermarsh on 2024-09-13 12:55_

---

_Label `bug` added by @charliermarsh on 2024-09-13 12:56_

---

_Referenced in [astral-sh/uv#4466](../../astral-sh/uv/issues/4466.md) on 2024-09-13 13:18_

---

_Referenced in [mendhak/waveshare-epaper-display#112](../../mendhak/waveshare-epaper-display/pulls/112.md) on 2025-08-30 20:11_

---
