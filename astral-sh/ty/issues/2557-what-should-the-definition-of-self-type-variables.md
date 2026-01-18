```yaml
number: 2557
title: "What should the `definition` of `Self` type variables be?"
type: issue
state: open
author: AlexWaygood
labels:
  - server
assignees: []
created_at: 2026-01-18T19:17:22Z
updated_at: 2026-01-18T19:17:22Z
url: https://github.com/astral-sh/ty/issues/2557
synced_at: 2026-01-18T20:18:22Z
```

# What should the `definition` of `Self` type variables be?

---

_@AlexWaygood_

The root cause of https://github.com/astral-sh/ty/issues/2514 was that we were attaching a subdiagnostic pointing to the definition of a type variable (so that users could see the typevar's upper bounds, defaults, etc.) for certain `invalid-argument-type` calls. The `definition` of `Self` type variables pointed to the definition of the class that was the upper bound of the `Self` typevar in that context, which meant that a huge subdiagnostic was displayed in some situations (the whole class definition was printed out). But even if we had only displayed the defining class's header range, it still would have been a bad subdiagnostic. Pointing to the class definition with the message "Type variable defined here" is highly confusing UX; it's better in that context to simply omit the subdiagnostic altogether rather than point to the class definition. The definition of a typevar's upper bound is conceptually a very different thing to the definition of the typevar itself.

To prevent bugs like this, https://github.com/astral-sh/ruff/pull/22648 changed things so that the `definition` of `Self` type variables no longer points to the class that is `Self`'s upper bound in that context; instead, it is now set to `None`. The disadvantage of this is that it probably leads to degraded language server functionality, but exactly what we want the language-server experience here to be is uncertain:
- When hovering over `Self` in a method signature, do we want that to show the docs for the class that is `Self`'s upper bound in that instance? Or do we want to show [the docs for `Self` itself](https://github.com/astral-sh/ruff/blob/7c9803310edad1fab3e113d36d53432d909d0bb5/crates/ty_vendored/vendor/typeshed/stdlib/typing.pyi#L566-L581)?
  - If the former: should we also display the docs for a typevar's upper bound more generally, even for non-`Self` typevars?
- Do we want go-to definition and go-to type definition on `Self` to take the user to the enclosing class, or to the definition of the `Self` special form in `typing.py(i)`?
  - Similarly, if the former, do we want to special-case `Self`, or do we want go-to (type) definition to always take the user to a type variable's upper bound, if the typevar has an upper bound?

Whatever behaviour we decide on here, ideally we would implement this in a way that does not reintroduce the bug magnet (that https://github.com/astral-sh/ruff/pull/22648 sought to get rid of) for `ty_python_semantic` subdiagnostics. This may involve adding some special-casing in the `ty_ide` crate for `Self`, but perhaps there's a way of centralising that special-casing so that we don't repeat logic in multiple places.

We should also add more tests around language-server functionality for `Self`; it's surprising that no snapshots had to be updated as part of https://github.com/astral-sh/ruff/pull/22648.

---

_Label `server` added by @AlexWaygood on 2026-01-18 19:17_

---
