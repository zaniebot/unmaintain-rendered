```yaml
number: 13361
title: "minor unexpected inconsistency between `uv python install X` and `uv seed -p X`"
type: issue
state: open
author: dimaqq
labels:
  - bug
assignees: []
created_at: 2025-05-09T06:51:31Z
updated_at: 2025-05-09T06:55:04Z
url: https://github.com/astral-sh/uv/issues/13361
synced_at: 2026-01-12T16:01:26Z
```

# minor unexpected inconsistency between `uv python install X` and `uv seed -p X`

---

_@dimaqq_

### Summary

```
🦐/c/otlp-proto (py3.14t)> uv python install 3.14+freethreaded
Installed Python 3.14.0a6 in 1m 29s
 + cpython-3.14.0a6+freethreaded-macos-aarch64-none
🦐/c/otlp-proto (py3.14t)> uv venv --seed -p 3.14+freethreaded
Using CPython 3.14.0a1 interpreter at: /usr/local/bin/python3.14t
```

So I've just installed alpha 6, but then alpha 1 is used.

### Platform

macos

### Version

latest

### Python version

3.14t

---

_Label `bug` added by @dimaqq on 2025-05-09 06:51_

---

_Comment by @dimaqq on 2025-05-09 06:53_

To make things even more confusing, when I try to use the virtualenv, 3.13 is used?

```
🦐/c/otlp-proto (py3.14t)> . .venv/bin/activate.fish
(otlp-proto) 🦐/c/otlp-proto (py3.14t)> uv sync
Using CPython 3.13.0
Removed virtual environment at: .venv
Creating virtual environment at: .venv
⠸ otlp-test-data==0.9.4 [... takes a while to build ...]
```

The above can be fixed by syncing with explicit version: `uv sync -p 3.14.0a6+freethreaded` 

---
