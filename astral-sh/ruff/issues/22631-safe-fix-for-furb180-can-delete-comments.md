```yaml
number: 22631
title: "Safe fix for `FURB180` can delete comments"
type: issue
state: open
author: ntBre
labels:
  - good first issue
  - fixes
  - preview
assignees: []
created_at: 2026-01-16T20:54:14Z
updated_at: 2026-01-18T17:11:10Z
url: https://github.com/astral-sh/ruff/issues/22631
synced_at: 2026-01-18T17:26:19Z
```

# Safe fix for `FURB180` can delete comments

---

_@ntBre_

This comment will be deleted:

```py
import abc


class C(
    other_kwarg=1,
    # comment
    metaclass=abc.ABCMeta,
):
    pass
```

https://play.ruff.rs/f64dfdc1-811d-466f-8e49-0f400e1859d0

<hr>

We should make the fix unsafe here if there are comments in the deletion range, as well as when there are other base classes:

https://github.com/astral-sh/ruff/blob/b80d8ff6fffa3b9f3c54877f21775c2ba958ba63/crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs#L85-L89

and update the fix safety docs to reflect the other way the fix can be unsafe:

https://github.com/astral-sh/ruff/blob/b80d8ff6fffa3b9f3c54877f21775c2ba958ba63/crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs#L41-L44





---

_Label `fixes` added by @ntBre on 2026-01-16 20:54_

---

_Label `preview` added by @ntBre on 2026-01-16 20:54_

---

_Label `good first issue` added by @ntBre on 2026-01-16 20:54_

---

_Comment by @AndyBodnar on 2026-01-18 17:01_

I'll take this one.

---

_Comment by @ntBre on 2026-01-18 17:11_

Sorry @AndyBodnar, this is already resolved by the linked PR #22234.

---
