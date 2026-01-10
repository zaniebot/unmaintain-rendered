---
number: 8127
title: uv sync without uninstalling packages
type: issue
state: closed
author: skeapskeap
labels:
  - question
assignees: []
created_at: 2024-10-11T12:56:40Z
updated_at: 2024-10-11T13:13:03Z
url: https://github.com/astral-sh/uv/issues/8127
synced_at: 2026-01-10T01:24:24Z
---

# uv sync without uninstalling packages

---

_Issue opened by @skeapskeap on 2024-10-11 12:56_

I need to use a virtual environment of the base Docker image with the packages installed in it. The problem is that the `uv sync` command removes packages that are not listed in `uv.lock`. The only working solution for me at the moment is to export `uv.lock` to `requirements.txt` and install them using `uv pip install`.
Is there a way to make the behavior of `uv sync` simillar to `pip install`, so that it only adds packages to the existing venv?

---

_Comment by @charliermarsh on 2024-10-11 13:03_

I think you're looking for `uv sync --inexact`.

---

_Label `question` added by @charliermarsh on 2024-10-11 13:03_

---

_Comment by @skeapskeap on 2024-10-11 13:10_

@charliermarsh that's exactly what I needed. Thanks a lot.

---

_Closed by @skeapskeap on 2024-10-11 13:10_

---

_Renamed from "uc sync without uninstalling packages" to "uv sync without uninstalling packages" by @skeapskeap on 2024-10-11 13:13_

---
