```yaml
number: 5018
title: "`uv tool install` should hint correct package when executable is provided by a dependency"
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - error messages
assignees: []
created_at: 2024-07-12T16:29:34Z
updated_at: 2024-07-15T16:54:41Z
url: https://github.com/astral-sh/uv/issues/5018
synced_at: 2026-01-10T05:31:37Z
```

# `uv tool install` should hint correct package when executable is provided by a dependency

---

_Issue opened by @zanieb on 2024-07-12 16:29_

e.g. this command works:

```
uvx fastapi
```

but

```
uv tool install fastapi
```

fails because `fastapi` is provided by `fastapi-cli` (see #5017 for details).

In this error case, `uv tool install` should detect that an executable matching the requested package name is provided by a dependency and suggest `uv tool install fastapi-cli` instead.

---

_Label `error messages` added by @zanieb on 2024-07-12 16:29_

---

_Comment by @charliermarsh on 2024-07-12 16:31_

That's a nice idea. Somewhat expensive but not terrible (and it's an error case anyway).

---

_Label `help wanted` added by @charliermarsh on 2024-07-12 16:31_

---

_Comment by @zanieb on 2024-07-12 16:33_

Yeah I don't love scanning the entire site packages for executables but since it's on-error 🤷‍♀️ we could limit to direct dependencies if we wanted to optimize later.

---

_Comment by @charliermarsh on 2024-07-12 16:40_

Should be cheap, it's just reading `RECORD` files.

---

_Comment by @blueraft on 2024-07-12 17:10_

I can pick this one up! 

---

_Closed by @zanieb on 2024-07-15 16:54_

---

_Closed by @zanieb on 2024-07-15 16:54_

---
