---
number: 16095
title: uv pip list --outdated does not use UV_FIND_LINKS
type: issue
state: open
author: tomazfs
labels:
  - bug
assignees: []
created_at: 2025-10-02T05:34:41Z
updated_at: 2025-10-02T18:25:23Z
url: https://github.com/astral-sh/uv/issues/16095
synced_at: 2026-01-07T13:12:19-06:00
---

# uv pip list --outdated does not use UV_FIND_LINKS

---

_Issue opened by @tomazfs on 2025-10-02 05:34_

### Summary

Command 'uv pip list --outdated' does not take into account local packages that are in UV_FIND_LINKS or --find-links locations while command 'pip list --outdated' does.

### Platform

Windows 10

### Version

uv 0.8.22 (ade2bdbd2 2025-09-23)

### Python version

Python 3.13.7

---

_Label `bug` added by @tomazfs on 2025-10-02 05:34_

---

_Referenced in [astral-sh/uv#16103](../../astral-sh/uv/pulls/16103.md) on 2025-10-02 15:50_

---

_Comment by @terror on 2025-10-02 18:25_

Interesting, I was able to reproduce this as well. I put up a draft PR (https://github.com/astral-sh/uv/pull/16103) to address this, however looking for feedback from maintainers to see if this is something we'd like to pursue before I un-draft it.

---
