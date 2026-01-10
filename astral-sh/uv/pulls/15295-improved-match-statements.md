```yaml
number: 15295
title: "Improved `match` statements"
type: pull_request
state: open
author: adamnemecek
labels: []
assignees: []
base: main
head: uv6
created_at: 2025-08-15T02:38:37Z
updated_at: 2025-08-16T00:00:14Z
url: https://github.com/astral-sh/uv/pull/15295
synced_at: 2026-01-10T06:44:33Z
```

# Improved `match` statements

---

_Pull request opened by @adamnemecek on 2025-08-15 02:38_

Replacement of #15094.

---

_Review comment by @zanieb on `crates/uv-cache/src/lib.rs`:1219 on 2025-08-15 13:26_

Did you mean to include this here?

---

_@zanieb reviewed on 2025-08-15 13:26_

---

_@adamnemecek reviewed on 2025-08-15 15:58_

---

_Review comment by @adamnemecek on `crates/uv-cache/src/lib.rs`:1219 on 2025-08-15 15:58_

You mean the change where I remove the max and use the built-in one? I think so . Should I remove it?

---

_Comment by @adamnemecek on 2025-08-15 15:59_

What is up  with the failing test?

---

_@zanieb reviewed on 2025-08-15 20:30_

---

_Review comment by @zanieb on `crates/uv-cache/src/lib.rs`:1219 on 2025-08-15 20:30_

Yeah I guess I don't see what it has to with the match statements? It makes sense to use the built-in max though.

---

_@adamnemecek reviewed on 2025-08-15 22:23_

---

_Review comment by @adamnemecek on `crates/uv-cache/src/lib.rs`:1219 on 2025-08-15 22:23_

It's only called from inside of match statements, so I am improving match statement logic.

---

_@zanieb reviewed on 2025-08-15 22:33_

---

_Review comment by @zanieb on `crates/uv-cache/src/lib.rs`:1219 on 2025-08-15 22:33_

I'm sorry but I don't think a change like

```diff
-            (Self::None(t1), Self::All(t2)) => Self::All(max(t1, t2)),
+            (Self::None(t1), Self::None(t2)) => Self::None(t1.max(t2)),
```

has anything to do with the structure of match statements like the rest of the changes here?


---

_@adamnemecek reviewed on 2025-08-15 22:36_

---

_Review comment by @adamnemecek on `crates/uv-cache/src/lib.rs`:1219 on 2025-08-15 22:36_

The max or the regrouping? The max does, the regrouping is clippy.

---

_@zanieb reviewed on 2025-08-15 22:56_

---

_Review comment by @zanieb on `crates/uv-cache/src/lib.rs`:1219 on 2025-08-15 22:56_

Specifically the `max` changes.

---

_Comment by @zanieb on 2025-08-15 22:57_

Are you turning on enforcement of this match style?

---

_@adamnemecek reviewed on 2025-08-15 22:58_

---

_Review comment by @adamnemecek on `crates/uv-cache/src/lib.rs`:1219 on 2025-08-15 22:58_

Ok. Should I change the title?

---

_Comment by @adamnemecek on 2025-08-15 22:58_

I think it’s the default 

---

_Comment by @zanieb on 2025-08-15 23:02_

We run Clippy so CI would be failing if it was the enforced already?

---

_Comment by @adamnemecek on 2025-08-15 23:16_

It was after some of the other changes. If you open the other PR there was one force push to fix that.

---

_Review comment by @zanieb on `crates/uv-cache/src/lib.rs`:1219 on 2025-08-15 23:59_

I'd rather have each pull request focused on a single change, so I'd prefer you backed this out so we can consider it separately.

---

_@zanieb reviewed on 2025-08-15 23:59_

---

_Comment by @zanieb on 2025-08-16 00:00_

> It was after some of the other changes. If you open the other PR there was one force push to fix that.

I'm sorry, I don't get what you mean? If this is the default and already enforced, why is [clippy passing on main](https://github.com/astral-sh/uv/actions/runs/17001407277/job/48203760390)?

---
