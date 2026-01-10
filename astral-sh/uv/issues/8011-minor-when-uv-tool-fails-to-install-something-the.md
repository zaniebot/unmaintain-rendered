---
number: 8011
title: "Minor: when uv tool fails to install something the environment remains dangling"
type: issue
state: closed
author: callegar
labels:
  - bug
  - uv tool
assignees: []
created_at: 2024-10-08T17:02:01Z
updated_at: 2024-10-09T13:01:54Z
url: https://github.com/astral-sh/uv/issues/8011
synced_at: 2026-01-10T01:24:22Z
---

# Minor: when uv tool fails to install something the environment remains dangling

---

_Issue opened by @callegar on 2024-10-08 17:02_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

E.g `uv tool install pyenv`. Because `pyenv` cannot be installed in this way the command fails. From this moment on `uv tool update --all` complains about `pyenv`. Easy to fix with a manual `uv tool uninstall pyenv`, but the latter should probably not be needed.

---

_Label `bug` added by @zanieb on 2024-10-08 17:33_

---

_Label `uv tool` added by @zanieb on 2024-10-08 17:33_

---

_Comment by @zanieb on 2024-10-08 17:33_

Thanks! We should clean that up.

---

_Referenced in [astral-sh/uv#8038](../../astral-sh/uv/pulls/8038.md) on 2024-10-09 07:58_

---

_Closed by @charliermarsh on 2024-10-09 13:01_

---

_Closed by @charliermarsh on 2024-10-09 13:01_

---
