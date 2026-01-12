```yaml
number: 1447
title: "Extend `KnownFunction` machinery to work with class methods"
type: issue
state: closed
author: dcreager
labels: []
assignees: []
created_at: 2025-10-27T20:09:29Z
updated_at: 2025-10-28T14:13:16Z
url: https://github.com/astral-sh/ty/issues/1447
synced_at: 2026-01-12T15:54:25Z
```

# Extend `KnownFunction` machinery to work with class methods

---

_@dcreager_

`KnownFunction` lets us detect particular functions that need to be handled specially in our type inference logic. We use this for things like `is_subtype_of`, to implement "evaluation" rules that are visible in the inferred return type of the function.

With our constraint set work, `ty_extensions` is become more complex, and now has a `ConstraintSet` type that we use in some return type annotations, and which has overridden dunder method for things like rich comparison and `bool` coercion.

There are also [some functions](https://github.com/astral-sh/ruff/blob/96b60c11d9efbfbf97faa5394fc3f7cb0a5c40c5/crates/ty_vendored/ty_extensions/ty_extensions.pyi#L45-L59) that would ideally be implemented as methods on `ConstraintSet`. (And [more](https://github.com/astral-sh/ruff/pull/21010#discussion_r2464978109) are in the works!) But those are currently implemented as top-level functions in the `ty_extensions` module, since that is what works with our `KnownFunction` machinery.

It would be nice to extend `KnownFunction` to work with class methods too.

---

_Comment by @sharkdp on 2025-10-28 08:37_

Did you see that we have `KnownBoundMethod`/ `KnownBoundMethodType`?

---

_Comment by @dcreager on 2025-10-28 12:49_

> Did you see that we have `KnownBoundMethod`/ `KnownBoundMethodType`?

Ah that looks perfect, thank you! I had been looking mostly in `class.rs`, thinking that I would need to hook into `own_class_member`.

---

_Comment by @dcreager on 2025-10-28 14:13_

That worked perfectly! https://github.com/astral-sh/ruff/pull/21108

---

_Closed by @dcreager on 2025-10-28 14:13_

---
