---
number: 16302
title: "[red-knot] Decide if `Type::to_instance` should return a `Result`"
type: issue
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
created_at: 2025-02-21T12:34:59Z
updated_at: 2025-03-11T16:42:47Z
url: https://github.com/astral-sh/ruff/issues/16302
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Decide if `Type::to_instance` should return a `Result`

---

_Issue opened by @MichaReiser on 2025-02-21 12:34_

### Description

There's a TODO in `Types::to_instance`:

```
// TODO: calling `.to_instance()` on any of these should result in a diagnostic,
// since they already indicate that the object is an instance of some kind:
```

We could consider returning a `Result` from `to_instance` so that it's the caller's responsibility, that the method is never called on a type that already is an instance or handle the error gracefully. 

I remember that I replaced the fallback to `Type::Unknown` with a panic and it didn't result in any failing tests. But @AlexWaygood mentioned that it is possible to trigger the panic with a custom/broken typeshed. I'm not sure if I care about that use case too much (existing with a useful panic message might be good enough?) to reduce ergonomics. 

I'm leaning toward making it a `Result` unless it is too painful and can only ever happen with a rather fundamentally broken typeshed.

---

_Label `red-knot` added by @MichaReiser on 2025-02-21 12:35_

---

_Added to milestone `Red Knot Q1 2025` by @MichaReiser on 2025-02-21 12:35_

---

_Comment by @AlexWaygood on 2025-02-21 12:48_

I'm currently leaning towards making `Type::to_instance()` return an `Option<Type>` and then doing something like this in `KnownClass::to_instance()`:

```rs
impl KnownClass {
    pub fn to_instance(&self, db: &'db dyn Db) -> Type<'db> {
        self
            .to_class_literal(db)
            .to_instance(db)
            .unwrap_or_else(|| {
                tracing::debug!("oh no, typeshed seems broken!");
                Type::unknown()
            })
    }
}
```

But I haven't tried it yet to see how well it works

---

_Comment by @MichaReiser on 2025-02-21 12:55_

I like that. Although I would make this at least a warning (and we may want to use a `Lazy` to avoid logging the same message a million times :D

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-02-24 18:50_

---

_Referenced in [astral-sh/ruff#16427](../../astral-sh/ruff/pulls/16427.md) on 2025-02-27 23:47_

---

_Referenced in [astral-sh/ruff#16428](../../astral-sh/ruff/pulls/16428.md) on 2025-02-28 00:20_

---

_Closed by @AlexWaygood on 2025-03-11 16:42_

---

_Closed by @AlexWaygood on 2025-03-11 16:42_

---
