```yaml
number: 8321
title: "`UV_FROZEN` env var please"
type: issue
state: closed
author: samuelcolvin
labels:
  - good first issue
  - help wanted
  - configuration
assignees: []
created_at: 2024-10-18T08:10:48Z
updated_at: 2024-10-18T17:37:51Z
url: https://github.com/astral-sh/uv/issues/8321
synced_at: 2026-01-12T15:59:23Z
```

# `UV_FROZEN` env var please

---

_@samuelcolvin_

uv is great, thank you!

It would be great if uv respected a `UV_FROZEN` env var with the same behaviour as `--frozen`.

This would match `UV_NO_SYNC`.

It would be particularly helpful in CI where:
1. I could set `UV_FROZEN` globally rather than adding it to every line
2. I could tell uv to run in "frozen" mode even when commands are withing makefiles or bash scripts, where it's not possible to set the `--frozen` cli arg


---

_Comment by @charliermarsh on 2024-10-18 12:58_

This makes sense, thanks!

---

_Label `configuration` added by @charliermarsh on 2024-10-18 12:58_

---

_Label `help wanted` added by @charliermarsh on 2024-10-18 12:59_

---

_Label `good first issue` added by @zanieb on 2024-10-18 14:34_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-18 17:22_

---

_Closed by @charliermarsh on 2024-10-18 17:37_

---
