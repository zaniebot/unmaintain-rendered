---
number: 10750
title: Install Python from local python build
type: issue
state: closed
author: BruceChen2017
labels:
  - question
assignees: []
created_at: 2025-01-19T07:34:45Z
updated_at: 2025-01-19T15:53:40Z
url: https://github.com/astral-sh/uv/issues/10750
synced_at: 2026-01-07T13:12:18-06:00
---

# Install Python from local python build

---

_Issue opened by @BruceChen2017 on 2025-01-19 07:34_

> Python does not publish official distributable binaries. As such, uv uses distributions from Astral [python-build-standalone](https://github.com/astral-sh/python-build-standalone) project   


Due to some network issue, I try to download python build manually and put it on my machine.

Then can I install python by local python build binary without downloading from remote?

I read the documentation, it seems no such option

---

_Comment by @FishAlchemist on 2025-01-19 10:33_

Is **UV_PYTHON_INSTALL_MIRROR**?
> Distributions can be read from a local directory by using the file:// URL scheme.

https://docs.astral.sh/uv/configuration/environment/#uv_python_install_mirror

---

_Comment by @charliermarsh on 2025-01-19 14:05_

What are you actually trying to do? You can just point any uv command to your Python with `--python {path/to/build}`.

---

_Label `question` added by @charliermarsh on 2025-01-19 14:06_

---

_Comment by @BruceChen2017 on 2025-01-19 15:53_

`UV_PYTHON_INSTALL_MIRROR` works for me

@FishAlchemist  thanks. It works now.

@charliermarsh thanks. I want to install python distribution locally without downloading python build from remote.



---

_Closed by @BruceChen2017 on 2025-01-19 15:53_

---
