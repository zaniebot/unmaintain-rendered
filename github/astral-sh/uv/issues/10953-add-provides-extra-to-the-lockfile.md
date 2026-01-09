---
number: 10953
title: "Add `provides-extra` to the lockfile"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2025-01-25T02:31:38Z
updated_at: 2025-02-13T22:17:53Z
url: https://github.com/astral-sh/uv/issues/10953
synced_at: 2026-01-07T13:12:18-06:00
---

# Add `provides-extra` to the lockfile

---

_Issue opened by @charliermarsh on 2025-01-25 02:31_

### Summary

We write `requires-dist`, but not `provides-extra`, so it's not possible to determine the set of valid extras (since an extra could be empty, in which case, it wouldn't appear in `requires-dist`).


---

_Label `enhancement` added by @charliermarsh on 2025-01-25 02:31_

---

_Comment by @charliermarsh on 2025-01-25 02:33_

Basically, we want to add `provides-extra` here:

https://github.com/astral-sh/uv/blob/1ef47aa1d51837931f89602b3277d83389e3425d/crates/uv-resolver/src/lock/mod.rs#L2632

And then serialize and deserialize it like we do for `requires-dist`.

---

_Assigned to @Gankra by @Gankra on 2025-01-25 02:58_

---

_Referenced in [astral-sh/uv#11063](../../astral-sh/uv/pulls/11063.md) on 2025-01-29 14:20_

---

_Closed by @zanieb on 2025-02-13 22:17_

---
