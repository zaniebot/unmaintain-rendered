```yaml
number: 1482
title: "Submodule imports: \"first from first serve\" analysis is too coarse"
type: issue
state: closed
author: Gankra
labels:
  - bug
  - imports
assignees: []
created_at: 2025-11-05T15:03:27Z
updated_at: 2025-11-10T23:59:49Z
url: https://github.com/astral-sh/ty/issues/1482
synced_at: 2026-01-12T15:54:25Z
```

# Submodule imports: "first from first serve" analysis is too coarse

---

_@Gankra_

This is a followup to https://github.com/astral-sh/ruff/pull/21173 and pertains to behaviours described in [imports/nonstandard_conventions.md](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md)

The "first from first serve" rule says it applies to the "first time in this scope (or any parent scope)", but we actually only implement "first time in the entire file" which doesn't make sense but was easy to implement and worked well. There is a test in that file that currently demonstrates the failure mode of this (two functions import a submodule, only the first function gets the submodule as a local).

---

_Assigned to @Gankra by @Gankra on 2025-11-05 15:03_

---

_Label `imports` added by @Gankra on 2025-11-05 15:03_

---

_Label `bug` added by @Gankra on 2025-11-05 17:16_

---

_Renamed from "first from first serve analysis is too coarse" to ""first from first serve" analysis is too coarse" by @Gankra on 2025-11-05 17:51_

---

_Comment by @carljm on 2025-11-05 21:10_

Didn't we discuss that the right answer here is that _neither_ function should get the submodule as a local? That is, we should skip the extra Definition-of-submodule entirely in nested scopes. Where the failure mode we have currently is something like this:

```py
def somefunc(mything):
    from .mything import whatever
    # now we think that `mything` is the submodule instead of the function parameter, but at runtime it is not
```

This would mean we don't model that the submodule (on first import) is added to module global scope, but given that we don't know if or when the function will be called, I think this is the right conservative modeling.

---

_Comment by @Gankra on 2025-11-05 21:29_

The intuitive model I was hoping to push for was essentially that "if you really want to do lazy imports by doing them inside a function body, you should have to do it in every function body you want that import in".

It has similar vibes to this note in pyright's docs:

> If a module contains the statement `import a.b` in the global scope and a function that includes the statement `import a` or `import a.c`, the function should not assume that it can access `a.b`. This assumption might or might not be safe depending on execution order.

Although not quite the same approach, and I'm not sure I agree with that rule, it's gesturing to the idea that imports are partially "function-local".

--------

I think it's worth trying a "global-only" implementation (neither function gets submodule) and seeing how much that affects the ecosystem.


---

_Comment by @carljm on 2025-11-05 21:34_

> The intuitive model I was hoping to push for was essentially that "if you really want to do lazy imports by doing them inside a function body, you should have to do it in every function body you want that import in".

This is already true for the RHS of the import. The problem with the left-hand-submodule part of the import is that at runtime it is not added to the function's own scope, it is actually added to the global scope. This leads to the bug shown in the code example in my previous comment.

I think it is better to not model the left-hand-submodule-attribute part at all for imports-in-functions, than to model it with this scoping bug. So I think if we want to model the submodule-attribute effects of in-function imports, we also need to place the attribute in the right scope, which I think is quite tricky since you don't actually know where in the module global control flow to consider that submodule attributed added. So I doubt that it is worth it at all.

---

_Comment by @Gankra on 2025-11-05 21:40_

Yeah, I'm aware it should go to global scope, and therefore the semantic is incorrect. In a fantasy world I had been considering a fake scope that exists under the current scope (or maybe under global scope?) where we can stow "things that are logically in global scope but execution order is a nightmare so it's in scope as long as a local wouldn't be shadowing it". Which I guess now that I've said it out loud, is another instance of:

* #1488 

---

_Comment by @carljm on 2025-11-05 21:45_

Except you'd also want a version of `available_submodule_attributes` that is somehow per-function? Because I do think it's important that we only make it available in the function where the import occurs, where we know it's actually available. If we don't do that also, then I still think it is better (not just "better because less work", but actually preferable) to be conservative and not model it as making the submodule attribute available at all. Overall it still feels very likely not worth trying to model it at all, but I guess the ecosystem report will tell.

---

_Comment by @Gankra on 2025-11-05 21:46_

Yes exactly a function-scoped version of available_submodule_attributes (I did say fantasy!).

---

_Renamed from ""first from first serve" analysis is too coarse" to "Submodule imports: "first from first serve" analysis is too coarse" by @AlexWaygood on 2025-11-09 17:02_

---

_Closed by @Gankra on 2025-11-10 23:59_

---
