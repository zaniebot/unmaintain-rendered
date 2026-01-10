```yaml
number: 1005
title: "goto-declaration on a real definition doesn't work"
type: issue
state: open
author: Gankra
labels:
  - server
assignees: []
created_at: 2025-08-15T13:59:48Z
updated_at: 2025-11-23T16:14:18Z
url: https://github.com/astral-sh/ty/issues/1005
synced_at: 2026-01-10T01:58:59Z
```

# goto-declaration on a real definition doesn't work

---

_Issue opened by @Gankra on 2025-08-15 13:59_

Similar to #1004 but the reverse (and I think actually a bit harder to solve).

In theory, if you goto-declaration on a definition in a .py, it would be desirable for us to jump to the definition in the .pyi, allowing you to freely jump between the "real" implementation and the type info.

I *think* this would require us to implement, essentially, "stub unmapping" with a third [ResolveMode](https://github.com/astral-sh/ruff/blob/bd4506aac56228af83d8ad76569530b3b32be473/crates/ty_python_semantic/src/module_resolver/resolver.rs#L49): StubsOnly. A mode that would literally ignore the .py you're current in.

---

_Added to milestone `Beta` by @Gankra on 2025-08-15 13:59_

---

_Label `bug` added by @Gankra on 2025-08-15 13:59_

---

_Label `server` added by @Gankra on 2025-08-15 13:59_

---

_Comment by @Gankra on 2025-08-15 14:08_

Note that unlike #1004 this behaviour *is not* implemented by pylance.

---

_Comment by @MichaReiser on 2025-08-15 14:10_

I'd be comfortable excluding this from the beta (also based on your observation that this isn't implemented in pylance). Though, it does sound cool 

---

_Assigned to @Gankra by @carljm on 2025-08-15 15:40_

---

_Removed from milestone `Beta` by @carljm on 2025-08-15 15:40_

---

_Added to milestone `GA` by @carljm on 2025-08-15 15:40_

---

_Comment by @erictraut on 2025-08-15 15:48_

AFAIK, no pyright or pylance users has ever requested this feature, so I think it's reasonable to make this low priority — or defer it entirely until you get a request from a user.

---

_Removed from milestone `GA` by @carljm on 2025-08-15 16:25_

---

_Label `bug` removed by @carljm on 2025-08-15 16:26_

---

_Added to milestone `Z post-stable` by @MichaReiser on 2025-11-14 09:02_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---

_Unassigned @Gankra by @Gankra on 2025-11-23 16:14_

---
