```yaml
number: 15947
title: "Support `uv.lock` and `pylock.toml` as constraints files"
type: issue
state: closed
author: konstin
labels:
  - enhancement
assignees: []
created_at: 2025-09-19T13:08:51Z
updated_at: 2025-09-19T17:15:26Z
url: https://github.com/astral-sh/uv/issues/15947
synced_at: 2026-01-10T03:23:54Z
```

# Support `uv.lock` and `pylock.toml` as constraints files

---

_Issue opened by @konstin on 2025-09-19 13:08_

Currently, we only support `requirements.txt`-format files as constraints in the `uv pip` interface. We can extend this support for `uv.lock` and `pylock.toml` files as constraints.

---

_Label `enhancement` added by @konstin on 2025-09-19 13:08_

---

_Comment by @zanieb on 2025-09-19 13:22_

Can we track these separately?

---

_Comment by @konstin on 2025-09-19 13:22_

Are they not the same feature?

---

_Comment by @my1e5 on 2025-09-19 13:23_

Related:

* https://github.com/astral-sh/uv/issues/13031

---

_Comment by @zanieb on 2025-09-19 13:26_

I don't know how much overlap there will be in an implementation. They're certainly distinct features though — they're different formats.

---

_Comment by @konstin on 2025-09-19 13:29_

I missed that we were already tracking #13031 and #5815, we can use those.

---

_Closed by @konstin on 2025-09-19 13:29_

---

_Comment by @zanieb on 2025-09-19 17:14_

I think https://github.com/astral-sh/uv/issues/5815 is a little different, in that it's a `--locked` interface rather than `--constraints path/to/uv.lock`

---

_Comment by @zanieb on 2025-09-19 17:15_

(The former uses the lockfile from a target package, while the latter uses an arbitrary lockfile)

---
