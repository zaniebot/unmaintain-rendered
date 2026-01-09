---
number: 13710
title: uv run python deleted .venv
type: issue
state: open
author: acscott
labels:
  - needs-mre
assignees: []
created_at: 2025-05-28T21:26:37Z
updated_at: 2025-07-04T02:16:53Z
url: https://github.com/astral-sh/uv/issues/13710
synced_at: 2026-01-07T13:12:18-06:00
---

# uv run python deleted .venv

---

_Issue opened by @acscott on 2025-05-28 21:26_

### Summary

I'm liking the tool and appreciate the tremendous amount of work into it.

However, this happened so need to report.

`uv start python <script> <param>` removed .venv, requiring a rebuild

Possible issue was to do with permissions. I changed permissions .venv and children. However, they weren't enough to prevent .venv from being removed. 

I know it removed it since it said it failed to remove the directory .venv Access is denied (os error 5) due to another process running the script, and I checked that it was there just before stopping the other process and then trying to `uv start ptyhon <script> <param>` again. (I assumed it was shuffling things around and doing a move)

(btw: in the bug report form it uses `uv self version` as an example when it should be `uv self --version`)

Thank you,
Adam


### Platform

Windows 10 Enterprise

### Version

uv-self 0.6.16 (d8ad9d3cd 2025-04-22)

### Python version

CPython 3.13.3

---

_Label `bug` added by @acscott on 2025-05-28 21:26_

---

_Comment by @zanieb on 2025-05-28 21:46_

Thanks for the kind words.

Can you share verbose logs, e.g., `uv run -v ...`, as well as a minimum reproduction as described in the issue template and https://github.com/astral-sh/uv/issues/9452?

> (btw: in the bug report form it uses uv self version as an example when it should be uv self --version)

This is because you're on an old version of uv, the command has since been added.

---

_Label `bug` removed by @charliermarsh on 2025-07-04 02:16_

---

_Label `needs-mre` added by @charliermarsh on 2025-07-04 02:16_

---
