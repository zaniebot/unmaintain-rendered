```yaml
number: 8775
title: "Detect if a script is called with `uv run`"
type: issue
state: closed
author: samuelcolvin
labels:
  - enhancement
assignees: []
created_at: 2024-11-03T14:36:19Z
updated_at: 2025-02-13T23:42:03Z
url: https://github.com/astral-sh/uv/issues/8775
synced_at: 2026-01-12T15:59:34Z
```

# Detect if a script is called with `uv run`

---

_@samuelcolvin_

I'd like to know if my script was called with `uv run`, but there doesn't seem to be an obvious way to tell.

I suggest you set an extra environment variable (e.g. `UV=1`) which the process can use to check if it was called that way.

(My use case if it matters is that I'm printing instructions on how to run other scripts, and I'd like to show "Run with `uv run -m ...`" or "Run with `python -m ...`")

---

_Label `enhancement` added by @zanieb on 2024-11-03 15:05_

---

_Comment by @zanieb on 2024-11-03 15:06_

Seems fine to me, though I'm a little wary of people keying behavior off of executing in a uv context in general.

---

_Comment by @charliermarsh on 2024-11-03 21:26_

This could also be helpful for the infinitely-recursive shebang @zanieb...

---

_Comment by @zanieb on 2024-11-04 01:35_

True I basically suggested this as a solution. Though the problem there is we can't tell if the recursion is intentional or not without capturing more information.

---

_Comment by @samuelcolvin on 2024-11-12 07:17_

Your could increment the count in the env car, so `UV=1`, `UV=2` etc.

---

_Comment by @samuelcolvin on 2024-11-12 07:20_

Fwiw, (maybe supports @zanieb's point) I no longer need this right now. Although I think adding the env var is still a good idea.

---

_Comment by @konstin on 2024-11-12 08:22_

This sounds like something that would fit nicely into the `sys` module, similar [sys.orig_argv](https://docs.python.org/3/library/sys.html#sys.orig_argv)

---

_Comment by @zanieb on 2025-02-13 23:41_

Closed in https://github.com/astral-sh/uv/pull/11326

---

_Closed by @zanieb on 2025-02-13 23:41_

---
