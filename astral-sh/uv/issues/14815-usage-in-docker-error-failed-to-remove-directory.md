```yaml
number: 14815
title: "Usage in docker: `error: failed to remove directory `/app/.venv`: Resource busy (os error 16)`"
type: issue
state: closed
author: coretl
labels:
  - bug
assignees: []
created_at: 2025-07-22T16:09:03Z
updated_at: 2025-07-22T20:18:59Z
url: https://github.com/astral-sh/uv/issues/14815
synced_at: 2026-01-12T16:01:56Z
```

# Usage in docker: `error: failed to remove directory `/app/.venv`: Resource busy (os error 16)`

---

_@coretl_

### Summary

I am using uv inside a devcontainer using a volume mount for `.venv` as desribed in the [docs](https://docs.astral.sh/uv/guides/integration/docker/#developing-in-a-container). With the uv binaries from the latest published 0.7 container this works fine. With the uv binaries from the latest published 0.8 container then `uv sync` fails with `error: failed to remove directory `/app/.venv`: Resource busy (os error 16)`

The reproducer from https://github.com/astral-sh/uv/issues/9423 also fails and demonstrates the error

### Platform

RHEL8

### Version

uv 0.8.0

### Python version

Python 3.11.13

---

_Label `bug` added by @coretl on 2025-07-22 16:09_

---

_Comment by @konstin on 2025-07-22 16:13_

Before you run `uv sync`, do you run any other commands, is any other process using or modifying the venv? Can you share the sequence of commands you run to reproduce this problem?

When you're saying latest 0.7, does that mean you test 0.7.22 and there it works?


---

_Comment by @zanieb on 2025-07-22 16:35_

I can reproduce this

---

_Comment by @zanieb on 2025-07-22 16:42_

I guess we previously did not delete the directory if it's empty, but now try to. I suspect https://github.com/astral-sh/uv/pull/14309

---

_Closed by @zanieb on 2025-07-22 18:50_

---

_Comment by @coretl on 2025-07-22 20:18_

Thanks for the speedy fix!

---

_Comment by @zanieb on 2025-07-22 20:18_

You're welcome! It'll be released in a minute.

---
