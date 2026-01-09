---
number: 12948
title: "Consider not showing PyPy downloads in `uv python list` by default"
type: issue
state: open
author: zanieb
labels:
  - needs-decision
  - cli
assignees: []
created_at: 2025-04-17T17:02:32Z
updated_at: 2025-04-17T17:02:40Z
url: https://github.com/astral-sh/uv/issues/12948
synced_at: 2026-01-07T13:12:18-06:00
---

# Consider not showing PyPy downloads in `uv python list` by default

---

_Issue opened by @zanieb on 2025-04-17 17:02_

I sort of wonder if we want to be showing all those PyPy interpreters by default? Most users don't need them.

We do already show them if you request `pypy`

```
❯ uv python list pypy
pypy-3.11.11-macos-aarch64-none    <download available>
pypy-3.10.16-macos-aarch64-none    <download available>
pypy-3.10.14-macos-aarch64-none    /opt/homebrew/bin/pypy3 -> ../Cellar/pypy3.10/7.3.17_1/bin/pypy3
pypy-3.9.19-macos-aarch64-none     <download available>
pypy-3.8.16-macos-aarch64-none     <download available>
```

_Originally posted by @zanieb in https://github.com/astral-sh/uv/issues/12915#issuecomment-2813017836_
            

---

_Label `cli` added by @zanieb on 2025-04-17 17:02_

---

_Label `needs-decision` added by @zanieb on 2025-04-17 17:02_

---
