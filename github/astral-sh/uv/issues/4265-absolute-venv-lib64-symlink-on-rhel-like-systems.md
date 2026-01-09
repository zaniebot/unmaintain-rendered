---
number: 4265
title: Absolute venv lib64 symlink on RHEL-like systems
type: issue
state: closed
author: dalcinl
labels:
  - compatibility
assignees: []
created_at: 2024-06-12T09:01:54Z
updated_at: 2024-06-12T13:36:28Z
url: https://github.com/astral-sh/uv/issues/4265
synced_at: 2026-01-07T13:12:17-06:00
---

# Absolute venv lib64 symlink on RHEL-like systems

---

_Issue opened by @dalcinl on 2024-06-12 09:01_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I'm using uv 0.2.11 installed via pip on Fedora 40. The `uv venv` command creates a virtual env that is slightly different to the Python's venv module: The `<venvdir>/lib64` symlink points to the absolute `<venvdir>/lib` directory, and not the relative `lib` directory. Besides the slight difference that it would be interesting to fix on its own, IMHO `python -m venv`  behavior is better, as it allows to relocate the full virtual environment tree elsewhere with `mv`, while `uv venv` behavior would lead to broken symlinks after a virtual environment relocation.

Reproducer:
```console
python -m venv /tmp/venv1
uv venv -q /tmp/venv2
readlink /tmp/venv1/lib64
readlink /tmp/venv2/lib64
```

Current output:
```
lib
/tmp/venv2/lib
```

Expected output:
```
lib
lib
```


---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-12 12:33_

---

_Label `compatibility` added by @charliermarsh on 2024-06-12 12:33_

---

_Referenced in [astral-sh/uv#4268](../../astral-sh/uv/pulls/4268.md) on 2024-06-12 13:19_

---

_Closed by @charliermarsh on 2024-06-12 13:36_

---
