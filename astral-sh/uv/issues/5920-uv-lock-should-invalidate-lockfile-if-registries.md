---
number: 5920
title: "`uv lock` should invalidate lockfile if registries change"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
created_at: 2024-08-08T17:11:51Z
updated_at: 2024-08-13T23:42:06Z
url: https://github.com/astral-sh/uv/issues/5920
synced_at: 2026-01-10T01:23:54Z
---

# `uv lock` should invalidate lockfile if registries change

---

_Issue opened by @charliermarsh on 2024-08-08 17:11_

If a lockfile lists a package from a registry that isn't part of the index URLs passed to `uv lock`, we should probably not reuse it.


---

_Label `bug` added by @charliermarsh on 2024-08-08 17:11_

---

_Label `preview` added by @charliermarsh on 2024-08-08 17:11_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-08 18:39_

---

_Comment by @charliermarsh on 2024-08-09 00:55_

I'm actually unsure about this. Like if you do:

```
uv add idna --index-url https://test.pypi.org/simple
```

And then `uv lock` later (no arguments)... should we "allow" the lockfile to use the existing entries, which reference https://test.pypi.org/simple?

---

_Comment by @zanieb on 2024-08-09 13:24_

Definitely! Seems weird to force everything to use another index URL after an `add` operation.

---

_Comment by @charliermarsh on 2024-08-09 13:38_

The only quirk is that if you don’t specify _any_ index URL, we treat it as PyPI, so the lock would be invalidated if you “stop” passing it on the command-line for any command.

---

_Comment by @charliermarsh on 2024-08-10 18:53_

Actually, I think the type is rich enough that we can avoid enforcing this if no index URLs are provided (i.e., assume the lockfile is fine). It's a little strange because if we fail, we'll then resolve against PyPI. But probably fine in practice.

---

_Referenced in [astral-sh/uv#6026](../../astral-sh/uv/pulls/6026.md) on 2024-08-12 02:26_

---

_Closed by @charliermarsh on 2024-08-13 23:42_

---
