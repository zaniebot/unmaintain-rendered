---
number: 15903
title: add a uv init --if-folder-not-exist flag
type: issue
state: closed
author: CNOCTAVE
labels:
  - enhancement
assignees: []
created_at: 2025-09-17T05:09:14Z
updated_at: 2025-09-17T11:16:18Z
url: https://github.com/astral-sh/uv/issues/15903
synced_at: 2026-01-10T01:26:01Z
---

# add a uv init --if-folder-not-exist flag

---

_Issue opened by @CNOCTAVE on 2025-09-17 05:09_

### Summary

In Jenkins or other CI, users will write `uv init` in shell command for init project, they want to both auto `uv init` and auto skip `uv init` after the project is inited. This is --if-folder-not-exist flag
Now the same workflow will fail after the 1st time of `uv init`. This is a very bad user experience.

### Example

_No response_

---

_Label `enhancement` added by @CNOCTAVE on 2025-09-17 05:09_

---

_Comment by @konstin on 2025-09-17 10:25_

Why are you initializing a project in CI, instead of e.g. cloning an existing repo or installing a tool or a script?

---

_Comment by @CNOCTAVE on 2025-09-17 10:48_

> Why are you initializing a project in CI, instead of e.g. cloning an existing repo or installing a tool or a script?

Because Jenkins here is to do OM work, not do git clone work.

---

_Comment by @CNOCTAVE on 2025-09-17 10:53_

Every worker machine has their own OM options. If I maintain many scripts (or many real uv projects here), it is unbearable.

---

_Comment by @zanieb on 2025-09-17 10:53_

Why not `uv init || echo "Project already exists"`?

---

_Comment by @CNOCTAVE on 2025-09-17 10:56_

> Why not `uv init || echo "Project already exists"`?

It make sense.

---

_Comment by @CNOCTAVE on 2025-09-17 10:56_

But it's ugly

---

_Comment by @CNOCTAVE on 2025-09-17 10:58_

Try to compare `uv init --if-folder-not-exist` to `uv init || echo "Project already exists"`. I prefer the flag one.

---

_Comment by @zanieb on 2025-09-17 11:02_

I understand the flag is nicer for you, but there is a complexity cost to every option we add to uv.

---

_Comment by @zanieb on 2025-09-17 11:02_

You can also just put the `uv init` invocation in an `if` that checks for the directory first.

---

_Comment by @CNOCTAVE on 2025-09-17 11:13_

How about me add this option and make PR?

---

_Comment by @zanieb on 2025-09-17 11:16_

No thank you.

---

_Closed by @CNOCTAVE on 2025-09-17 11:16_

---
