---
number: 14550
title: "`uv run --package` doesn't sync the environment to the listed package"
type: issue
state: closed
author: iandolge
labels:
  - needs-mre
assignees: []
created_at: 2025-07-10T20:38:00Z
updated_at: 2025-07-10T20:49:47Z
url: https://github.com/astral-sh/uv/issues/14550
synced_at: 2026-01-10T01:25:46Z
---

# `uv run --package` doesn't sync the environment to the listed package

---

_Issue opened by @iandolge on 2025-07-10 20:38_

### Summary

In a project with workspace members: when I run a command with `uv run --package my-package` the environment is not synced to `my-package`.

### Platform

Linux x86_64

### Version

uv 0.7.20

### Python version

Python 3.12.11

---

_Label `bug` added by @iandolge on 2025-07-10 20:38_

---

_Comment by @charliermarsh on 2025-07-10 20:39_

Can you please provide a reproduction? It performs an inexact sync, ensuring that all of `my-package` dependencies are installed, but not uninstalling extraneous dependencies. (You can pass `--exact` to uninstall extraneous dependencies.) We have test coverage for this and it works as expected in my testing.

---

_Label `bug` removed by @charliermarsh on 2025-07-10 20:39_

---

_Label `needs-mre` added by @charliermarsh on 2025-07-10 20:39_

---

_Comment by @iandolge on 2025-07-10 20:44_

Ah, yep running with `--exact` gives the behavior I wanted. Thank you!

---

_Comment by @iandolge on 2025-07-10 20:49_

I forgot that the default of uv sync is exact vs uv run default is inexact, for some reason I was expecting this to be different for running against a package.

---

_Closed by @iandolge on 2025-07-10 20:49_

---
