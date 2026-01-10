```yaml
number: 390
title: "Failed to install jupyter: File exists"
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-11-10T14:56:15Z
updated_at: 2023-11-10T20:24:21Z
url: https://github.com/astral-sh/uv/issues/390
synced_at: 2026-01-10T05:40:31Z
```

# Failed to install jupyter: File exists

---

_Issue opened by @konstin on 2023-11-10 14:56_

Trying to install `jupyter.txt` created from the requirement `jupyter`:

```
$ cargo run --bin puffin --release -q -- pip-sync jupyter.txt
Uninstalled 2 packages in 5ms
error: Failed to install: jupyter-core==5.5.0
  Caused by: failed to hardlink file from /home/konsti/.cache/puffin/wheels-v0/registry/jupyter_core-5.5.0/jupyter.py to /home/konsti/projects/puffin/.venv/lib/python3.10/site-packages/jupyter.py
  Caused by: File exists (os error 17)
```

I have a hunch that this is two packages containing the same file.

---

_Label `bug` added by @charliermarsh on 2023-11-10 17:15_

---

_Comment by @charliermarsh on 2023-11-10 17:15_

What _should_ happen here?

---

_Comment by @konstin on 2023-11-10 17:46_

The current consensus is overriding the existing file

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-10 19:55_

---

_Closed by @charliermarsh on 2023-11-10 20:24_

---
