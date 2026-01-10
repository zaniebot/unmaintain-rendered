```yaml
number: 3724
title: Deadlock during resolution
type: issue
state: closed
author: zanieb
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-05-21T22:02:51Z
updated_at: 2024-06-03T16:27:12Z
url: https://github.com/astral-sh/uv/issues/3724
synced_at: 2026-01-10T05:31:37Z
```

# Deadlock during resolution

---

_Issue opened by @zanieb on 2024-05-21 22:02_

We're observing non-deterministic deadlocks in our test suite that we believe comes from #3627 which would mean it's present in v0.1.45+.

Please chime in if you're encountering this. We have yet to come up with a reliable reproduction.

This looks limited to Linux (Ubuntu in our test suite).

---

_Label `bug` added by @zanieb on 2024-05-21 22:03_

---

_Label `help wanted` added by @zanieb on 2024-05-21 22:03_

---

_Comment by @ChannyClaus on 2024-05-27 14:02_

is there a link to the deadlocked build?

---

_Closed by @ibraheemdev on 2024-06-03 16:25_

---

_Comment by @ibraheemdev on 2024-06-03 16:27_

https://github.com/astral-sh/uv/pull/3987 should fix this, but we can reopen this issue if it ever appears again.

---
