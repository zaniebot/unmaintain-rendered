---
number: 4629
title: "`uv add` hangs with circular dependencies in the lock file."
type: issue
state: closed
author: blueraft
labels:
  - bug
  - preview
assignees: []
created_at: 2024-06-28T16:47:05Z
updated_at: 2024-06-28T20:15:29Z
url: https://github.com/astral-sh/uv/issues/4629
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv add` hangs with circular dependencies in the lock file.

---

_Issue opened by @blueraft on 2024-06-28 16:47_

Originally posted in #4599 

I've encountered a consistent hang when there's a circular dependency in the lock file.

```toml
name = "foo"
version = "0.1.0"
requires-python = ">=3.9"
dependencies = ["poetry"]
```

When I run `uv lock` followed by `uv add flask`, the process hangs indefinitely.

Did a little digging myself and I found a loop in the lock.rs file that appears to cause the issue. Adding a check to skip the loop iteration if the key already exists in the map seems to fix the problem for me.

https://github.com/astral-sh/uv/blob/796171e1e6d55dae095effebb6d815ac91727c2c/crates/uv-resolver/src/lock.rs#L354-L380


---

_Label `preview` added by @ibraheemdev on 2024-06-28 16:47_

---

_Label `bug` added by @ibraheemdev on 2024-06-28 16:47_

---

_Comment by @charliermarsh on 2024-06-28 17:05_

Thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-28 18:38_

---

_Comment by @charliermarsh on 2024-06-28 18:39_

I shall fix.

---

_Referenced in [astral-sh/uv#4633](../../astral-sh/uv/pulls/4633.md) on 2024-06-28 18:53_

---

_Closed by @charliermarsh on 2024-06-28 20:15_

---

_Closed by @charliermarsh on 2024-06-28 20:15_

---
