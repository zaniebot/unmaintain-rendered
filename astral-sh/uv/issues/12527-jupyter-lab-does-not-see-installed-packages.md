```yaml
number: 12527
title: Jupyter Lab does not see installed packages
type: issue
state: open
author: plebik
labels:
  - question
assignees: []
created_at: 2025-03-28T09:15:45Z
updated_at: 2025-03-28T19:19:39Z
url: https://github.com/astral-sh/uv/issues/12527
synced_at: 2026-01-12T16:01:05Z
```

# Jupyter Lab does not see installed packages

---

_@plebik_

### Summary

I have installed packes such as `numpy` and `pandas`. When I ran `uvx jupter lab` and opened the notebook, packages couldn't have been imported. However when I installed `jupterlab` through `pip` I was able to run the notebook through `jupyter-lab` and import the packages.

### Platform

Windows 11 x86_64

### Version

uv 0.6.10 (f2a2d982b 2025-03-25)

### Python version

Python 3.12.9

---

_Label `bug` added by @plebik on 2025-03-28 09:15_

---

_Comment by @charliermarsh on 2025-03-28 19:19_

If you run `uvx jupter lab`, that will just install Jupyter into an isolated environment and start the Jupyter Lab application. I suggest reading the Jupyter guide: https://docs.astral.sh/uv/guides/integration/jupyter/.

---

_Label `bug` removed by @charliermarsh on 2025-03-28 19:19_

---

_Label `question` added by @charliermarsh on 2025-03-28 19:19_

---
