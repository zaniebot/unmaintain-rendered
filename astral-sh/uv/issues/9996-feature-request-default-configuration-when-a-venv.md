```yaml
number: 9996
title: "Feature request: Default configuration when a venv is created"
type: issue
state: closed
author: faameunier
labels:
  - configuration
assignees: []
created_at: 2024-12-18T13:00:55Z
updated_at: 2025-01-10T17:51:50Z
url: https://github.com/astral-sh/uv/issues/9996
synced_at: 2026-01-12T16:00:04Z
```

# Feature request: Default configuration when a venv is created

---

_@faameunier_

I recently started using uv and it is awesome.

I have a use case where I want to use the `--system-site-packages` flag by default whenever a python venv is created by uv.
This is to avoid having to run `uv venv --system-site-packages` each time I need it, and just be able to `uv run`.

Linked issue for scripts:
- https://github.com/astral-sh/uv/issues/7999

### Current behavior
```
conda install numpy==1.25.0
uv init
uv run python -c "import numpy" 
```
results in `ModuleNotFoundError`

### Feature request behavior
```
export UV_SYSTEM_SITE_PACKAGES = true
conda install numpy==1.25.0
uv init
uv run python -c "import numpy" 
```
works

### Misc
Similar behavior used to be possible with [virtualenv](https://virtualenv.pypa.io/en/legacy/reference.html#configuration-file) but is not implemented in venv.

---

_Renamed from "Default configuration when a venv is created" to "Feature request: Default configuration when a venv is created" by @faameunier on 2024-12-18 15:05_

---

_Label `configuration` added by @samypr100 on 2024-12-19 01:06_

---

_Comment by @ghost on 2025-01-10 17:42_

Is this a duplicate of #5737?

---

_Comment by @zanieb on 2025-01-10 17:51_

Yes, thanks!

---

_Closed by @zanieb on 2025-01-10 17:51_

---
