```yaml
number: 2383
title: "Ignore empty string for `S105`/`S106`/`S107`"
type: issue
state: closed
author: ngnpope
labels:
  - bug
  - good first issue
assignees: []
created_at: 2023-01-31T08:37:56Z
updated_at: 2023-01-31T21:56:05Z
url: https://github.com/astral-sh/ruff/issues/2383
synced_at: 2026-01-12T15:54:42Z
```

# Ignore empty string for `S105`/`S106`/`S107`

---

_@ngnpope_

Using `v0.0.237` I get: ```S105 Possible hardcoded password: ""```

This was in some code that was checking whether a password/secret matched the empty string, i.e. `if secret == ""`.

I think we want to ignore any comparisons to `""` (or `None` if that's not already handled) for `S105`, `S106`, and `S107`.

---

_Comment by @charliermarsh on 2023-01-31 12:29_

I'm tempted to follow `bandit` here even if it's not totally ideal. (Not sure what their exact behavior is for this.)

---

_Comment by @ngnpope on 2023-01-31 13:06_

So I checked, see https://github.com/charliermarsh/ruff/issues/2384#issuecomment-1410302523, and `bandit` doesn't ignore empty string.

While I was being thorough, I checked what happens if we have a byte string, e.g. `password = b"shh, don't tell anyone!"` and neither `ruff`, nor `bandit` complain. Wondering if they should? 🤔  

---

_Comment by @charliermarsh on 2023-01-31 13:10_

Yeah maybe we should ignore `""` at least.

---

_Label `bug` added by @charliermarsh on 2023-01-31 17:46_

---

_Comment by @charliermarsh on 2023-01-31 17:46_

I'll call it a "bug", even though it's really a refinement :)

---

_Label `good first issue` added by @charliermarsh on 2023-01-31 21:29_

---

_Closed by @charliermarsh on 2023-01-31 21:56_

---
