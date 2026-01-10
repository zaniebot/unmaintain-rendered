---
number: 11283
title: uv run fails to open pyqt6.QMainWindow without error under certain condition
type: issue
state: closed
author: EDohmen
labels:
  - bug
assignees: []
created_at: 2025-02-06T15:43:52Z
updated_at: 2025-02-06T16:29:36Z
url: https://github.com/astral-sh/uv/issues/11283
synced_at: 2026-01-10T01:25:03Z
---

# uv run fails to open pyqt6.QMainWindow without error under certain condition

---

_Issue opened by @EDohmen on 2025-02-06 15:43_

### Summary

I ran into trouble with a PyQt6 gui-script that was neither opening, nor throwing any error, when running with `uv run`, but working fine when calling the gui-script without `uv`.
I could narrow down the issue and produced a MRE here (description with commands in README.md): https://github.com/EDohmen/MRE_uv_run_pyqt5vs6.git

However, I am still not sure what happens or what I am missing and would appreciate any feedback and ideas here in case it's not reproducible for others and a uv bug, but something I'm overlooking on my side. The behaviour got stranger the more I tried to narrow it down.
Here's what I found out:

- The bug only occurs with `pyqt6`, not with `pyqt5` (I would like to use PyQt6 for the original project though)
- only `uv run` is affected and calling the executable (defined gui-script from `pyproject.toml`) without `uv` results in expected behaviour, i.e. a window opens 
- The program seems to simply end when entering the `super().__init__()` function of a `pyqt6.QMainWindow`, however
- this behaviour is only triggered if there is also a `__new__()` function defined, if there is none additionally defined (which I need in my original project) all works fine again also with `uv run`

Any help would be very much appreciated. Thanks for your amazing work and uv!

### Platform

Ubuntu 22.04

### Version

0.5.29

### Python version

3.10, 3.11, 3.12

---

_Label `bug` added by @EDohmen on 2025-02-06 15:43_

---

_Closed by @EDohmen on 2025-02-06 16:29_

---
