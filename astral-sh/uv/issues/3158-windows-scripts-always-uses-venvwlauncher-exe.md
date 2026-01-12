```yaml
number: 3158
title: "Windows scripts always uses `venvwlauncher.exe`"
type: issue
state: closed
author: jbott
labels:
  - bug
  - windows
assignees: []
created_at: 2024-04-20T06:31:49Z
updated_at: 2024-04-20T14:16:49Z
url: https://github.com/astral-sh/uv/issues/3158
synced_at: 2026-01-12T15:58:42Z
```

# Windows scripts always uses `venvwlauncher.exe`

---

_@jbott_

I was doing some windows spelunking and noticed that uv is unconditionally using `venvwlauncher.exe` for both the `python.exe` and `pythonw.exe` case. This seems to be a typo compared to the upstream cpython venv module, which uses the binary that matches the presence or absence of `w`.

I haven't actually run into an issue using this, so I can't actually confirm it's a bug.

https://github.com/astral-sh/uv/blob/b4ee7d73594183e677f8324f3e890ce53a3ba76d/crates/uv-virtualenv/src/bare.rs#L182-L186
https://github.com/python/cpython/blob/d457345bbc6414db0443819290b04a9a4333313d/Lib/venv/__init__.py#L274-L277

---

_Comment by @charliermarsh on 2024-04-20 13:30_

That definitely looks wrong, but trying to understand why it hasn't caused any problems...

---

_Comment by @charliermarsh on 2024-04-20 13:32_

Ah, it only affects Python 3.13.

---

_Comment by @charliermarsh on 2024-04-20 13:33_

PR welcome or I can fix!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-20 14:12_

---

_Comment by @charliermarsh on 2024-04-20 14:12_

Fixed in https://github.com/astral-sh/uv/pull/3160, thanks.

---

_Label `bug` added by @charliermarsh on 2024-04-20 14:12_

---

_Label `windows` added by @charliermarsh on 2024-04-20 14:12_

---

_Closed by @charliermarsh on 2024-04-20 14:16_

---
