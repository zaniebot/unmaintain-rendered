---
number: 17309
title: Conflicting Python Requirements when workspace members have different Python requirements
type: issue
state: closed
author: btakita
labels:
  - question
assignees: []
created_at: 2026-01-02T19:29:03Z
updated_at: 2026-01-02T21:53:46Z
url: https://github.com/astral-sh/uv/issues/17309
synced_at: 2026-01-07T13:12:19-06:00
---

# Conflicting Python Requirements when workspace members have different Python requirements

---

_Issue opened by @btakita on 2026-01-02 19:29_

### Question

I have a monorepo where that has `project.requires-python = ">=3.12"`. Most of the apps + libraries use Python 3.13. One of the workspace members has to be pinned to 3.12 due to a dependency.

When I run `uv`, I get the conflicting Python Requirements error.

Is there a way to have workspace members have conflicting Python Requirements without an error?

Or do I need a workaround...I tried to have the app requires the Pinned 3.12 configured with `project.requires-python = ">=3.12"` while setting `tool.uv.python-version = "3.12"`...with the other apps having `tool.uv.python-version = "3.13"` but it didn't work.

I removed the conflicting apps from the workspace members...and that worked.

I guess I can reason about the baseline libraries that supports all apps can be a workspace member. But the conflicting apps + conflicting libraries must be excluded. Fortunately dependency sources can be added using `path`...so it works. I suppose supporting multiple versions of python in a workspace is out of the scope of this library.

### Platform

Linux 6.18.2-3-cachyos x86_64 unknown

### Version

uv 0.9.21 (0dc9556ad 2025-12-30)

---

_Label `question` added by @btakita on 2026-01-02 19:29_

---

_Comment by @btakita on 2026-01-02 21:53_

Closing due to problem solved.

---

_Closed by @btakita on 2026-01-02 21:53_

---
