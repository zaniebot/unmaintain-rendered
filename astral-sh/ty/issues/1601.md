```yaml
number: 1601
title: goto-type for SpecialForms
type: issue
state: closed
author: Gankra
labels:
  - server
assignees: []
created_at: 2025-11-20T15:25:13Z
updated_at: 2025-11-20T18:14:31Z
url: https://github.com/astral-sh/ty/issues/1601
synced_at: 2026-01-10T01:58:59Z
```

# goto-type for SpecialForms

---

_Issue opened by @Gankra on 2025-11-20 15:25_

Point of terminology: goto-definition, goto-declaration, and goto-type are 3 different operations with very similar behaviour. cmd+click prefers those options in that order, if they have a result. goto-type has been a bit neglected because it's the last in the chain, and so users largely don't interact with it.

<img width="798" height="244" alt="Image" src="https://github.com/user-attachments/assets/a28482c1-97e1-4f02-a5b4-53cd3db9eb07" />

Now that inlay-hints are clickable with goto-type semantics, users are *way* more exposed to goto-type, and so the random places where goto-type fails out are way more noticeable.

goto-type is just a thin wrapper around `Type::definition`:

https://github.com/astral-sh/ruff/blob/416e2267daaf9699c26ec73c644a03356e3de7f2/crates/ty_python_semantic/src/types.rs#L7553

Notably all SpecialForms yield `None`, which means e.g. you can't click on `Literal` in an inlay hint, even though it would be nice for it to go to `typing.Literal`'s definition (where we could potentially have docs explaining Literals In General).

---

_Label `server` added by @Gankra on 2025-11-20 15:25_

---

_Comment by @Gankra on 2025-11-20 16:26_

(Off the top of my head I'm not sure how to implement this mapping, I imagine this is trivial for the right person)

---

_Closed by @AlexWaygood on 2025-11-20 18:14_

---
