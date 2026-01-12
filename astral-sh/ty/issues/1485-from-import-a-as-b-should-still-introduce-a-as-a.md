```yaml
number: 1485
title: "`from . import a as b` should still introduce `a` as a local in packages"
type: issue
state: open
author: Gankra
labels:
  - imports
assignees: []
created_at: 2025-11-05T17:20:29Z
updated_at: 2025-11-11T21:20:17Z
url: https://github.com/astral-sh/ty/issues/1485
synced_at: 2026-01-12T15:54:25Z
```

# `from . import a as b` should still introduce `a` as a local in packages

---

_@Gankra_

This is a followup to https://github.com/astral-sh/ruff/pull/21173 and pertains to behaviours described in [imports/nonstandard_conventions.md](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md)

Currently `from .a import b` in an `__init__.py(i)` can introduce a local for `a` (via `ImportFromSubmoduleDefinitionNodeRef`).

Similarly, `from . import a` introduces the same local (via `ImportFromDefinitionNodeRef`).

We should *also* support `from . import a as c` in an `__init__.py(i)` introducing the local `a` (via `ImportFromSubmoduleDefinitionNodeRef`). (Ditto for `from whatever.thispackage import a as c` - #1484) 

This reflects the fact that the python runtime introduces the submodule attribute for the first import of `thispackage.a` regardless of how it gets imported, even if you rename it in the import.


---

_Assigned to @Gankra by @Gankra on 2025-11-05 17:20_

---

_Label `bug` added by @Gankra on 2025-11-05 17:20_

---

_Label `imports` added by @Gankra on 2025-11-05 17:20_

---

_Comment by @Gankra on 2025-11-11 20:18_

For the record, I will die happy if we never ship this. I expect very few people are relying on it, and they probably shouldn't be. But time will tell if the accuracy gods demand it...

---

_Comment by @carljm on 2025-11-11 20:20_

The tricky thing about implementing this is that the syntactic form `from . import a as b` in an `__init__.py` may or may not create a submodule attribute `a`. It does only if `a` is actually a submodule, but `a` may be something else entirely. (In which case this statement is just a really strange way to write `b = a`.)

---

_Label `bug` removed by @carljm on 2025-11-11 20:20_

---

_Comment by @Gankra on 2025-11-11 20:48_

Having now worked in this code a bunch, it's my understanding that we actually refuse to believe that the `b = a` situation can happen ([look for `import_is_self_referential` in `infer_import_from_definition`](https://github.com/astral-sh/ruff/blob/e4374f14ed12873541d9a9ccca7eea92456684f6/crates/ty_python_semantic/src/types/infer/builder.rs#L5768-L5779)), so in terms of current ty semantics it's actually "safe" to assume that `from . import a` only ever refers to a submodule if you're in an `__init__.py(i)`.

---

_Comment by @Gankra on 2025-11-11 21:03_

With that claim, the implementation of this actually looks a lot like the one I describe in:

* https://github.com/astral-sh/ty/issues/1526

But instead you check `if is_self_import {` and roughly duplicate this logic here:

https://github.com/astral-sh/ruff/blob/e4374f14ed12873541d9a9ccca7eea92456684f6/crates/ty_python_semantic/src/semantic_index/builder.rs#L1481-L1496

Unfortunately *this* case actually runs hard into the "ast nodes can introduce at-most one definition" rule, because `from . import a as c, b as d` would need two `ImportFromSubmoduleDefinitionNodeRef`, and the Alias nodes are already claimed by `ImportFromDefinitionNodeRefs`. Maybe I was wrong and you actually can hang them off of `Identifier`? So some hacky little thing where one gets the `Alias` and the other gets the `Identifier` inside the alias?

---

_Unassigned @Gankra by @Gankra on 2025-11-11 21:20_

---
