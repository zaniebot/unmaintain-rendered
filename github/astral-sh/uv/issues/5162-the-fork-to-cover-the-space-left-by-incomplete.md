---
number: 5162
title: the fork to cover the space left by incomplete markers is probably not correctly constructed
type: issue
state: closed
author: BurntSushi
labels:
  - bug
assignees: []
created_at: 2024-07-17T18:19:59Z
updated_at: 2024-07-26T14:28:22Z
url: https://github.com/astral-sh/uv/issues/5162
synced_at: 2026-01-07T13:12:17-06:00
---

# the fork to cover the space left by incomplete markers is probably not correctly constructed

---

_Issue opened by @BurntSushi on 2024-07-17 18:19_

Whenever we have incomplete marker expressions that provoke a fork, we create an extra fork that spans the negation of the union of those marker expressions. I believe the way we are creating that fork today is not quite correct:

https://github.com/astral-sh/uv/blob/af66f75980ad06ac5801ae4ed2417a2ef1dd209b/crates/uv-resolver/src/resolver/mod.rs#L2557-L2563

Namely, it has no dependencies in it, but I believe it should have all dependencies _except_ for the dependencies that provoked the fork in the first place. That is, it should have every "non fork package" in it:

https://github.com/astral-sh/uv/blob/af66f75980ad06ac5801ae4ed2417a2ef1dd209b/crates/uv-resolver/src/resolver/mod.rs#L2547-L2551

I believe this is the cause of one aspect of the problem reported in #5161.

---

_Referenced in [astral-sh/uv#5161](../../astral-sh/uv/issues/5161.md) on 2024-07-17 18:23_

---

_Referenced in [astral-sh/uv#5163](../../astral-sh/uv/pulls/5163.md) on 2024-07-17 18:24_

---

_Label `bug` added by @BurntSushi on 2024-07-25 14:47_

---

_Closed by @BurntSushi on 2024-07-26 14:28_

---
