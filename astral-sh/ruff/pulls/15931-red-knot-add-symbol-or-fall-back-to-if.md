```yaml
number: 15931
title: "[red-knot] Add `Symbol::or_fall_back_to_if`"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
base: main
head: alex/fallback-if
created_at: 2025-02-04T13:16:06Z
updated_at: 2025-02-05T14:48:51Z
url: https://github.com/astral-sh/ruff/pull/15931
synced_at: 2026-01-12T15:55:53Z
```

# [red-knot] Add `Symbol::or_fall_back_to_if`

---

_@AlexWaygood_

## Summary

This PR replaces introduces a new API, `Symbol::or_fall_back_to_if()`. The new API replaces two existing methods on `Symbol`: `Symbol::possibly_unbound()` and `Symbol::or_fall_back_to()`, as well as `Boundness::or()`.

The new API allows us to elegantly chain a series of fallback symbol-lookups. It also means we're lazy by default (I realized we were eagerly executing several `ModuleType::to_class_literal(db).member(db, name)` calls which would be immediately thrown away in several situations). Lastly, it means that in some situations we construct union types where the elements are arranged in a more intuitive order than we currently have on `main`.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2025-02-04 13:16_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-04 13:16_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-04 13:16_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-04 13:16_

---

_@AlexWaygood reviewed on 2025-02-04 13:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:271 on 2025-02-04 13:20_

(Came here to do the TODO, ended up making this PR instead)

---

_Comment by @AlexWaygood on 2025-02-04 13:26_

[Neutral on codspeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Ffallback-if)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/symbol.rs`:71 on 2025-02-04 13:34_

Nit: Just `or_else`?

---

_@MichaReiser reviewed on 2025-02-04 13:34_

---

_@MichaReiser reviewed on 2025-02-04 13:34_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/symbol.rs`:71 on 2025-02-04 13:34_

Never mind, it also accepts a condition

---

_@MichaReiser reviewed on 2025-02-04 13:37_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/symbol.rs`:71 on 2025-02-04 13:37_

The `Option` API for this would be:

`symbol.bound().filter(condition).unwrap_or_else(fallback)` 

However, that doesn't work because the `b` case sometimes falls back to `self` and sometimes to the `fallback`. It still makes me wonder if we can decompose this operation.

---

_@AlexWaygood reviewed on 2025-02-04 13:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:71 on 2025-02-04 13:47_

I wondered about a version of this where we kept `Symbol::fall_back_to()` and implemented `Symbol::fall_back_to_if()` as a wrapper around `Symbol::fall_back_to()`:

```rs
impl<'db> Symbol<'db> {
    fn or_fall_back_to(self, db, &'db dyn Db, fallback: Symbol<'db>) -> Self {
        match (&self, &other) {
            (Symbol::Type(_, Boundness::Bound), _) | (_, Symbol::Unbound) => self,
            (Symbol::Unbound, _) => other,
            (Symbol::Type(self_ty, _), (Symbol::Type(fallback_ty, fallback_boundness) => Symbol::Type(
                UnionType::from_elements(db, [self_ty, fallback_ty]),
                *fallback_boundness
            )
        }
    }

    fn or_fall_back_to_if(
        self,
        db: &'db dyn Db,
        condition: impl FnOnce() -> bool,
        fallback_fn: impl FnOnce() -> Symbol<'db>,
    ) -> Self {
        if self.possibly_unbound() && condition() {
            self.or_fall_back_to(fallback_fn())
        } else {
            self
        }
    }
}
```

I don't like this much, though. The `self.possibly_unbound()` call goes through another `match` expression which is redundant. We can do it in one `match`, which I find more readable (there's less indirection). `Symbol::or_fall_back_to()` would also _only_ be used by this one method (`Symbol::or_fall_back_to_if()`) if we did it like this; `Symbol::or_fall_back_to_if()` seems better for all our current use cases.

---

_Renamed from "[red-knot] Add `Symbol::fall_back_to_if`" to "[red-knot] Add `Symbol::or_fall_back_to_if`" by @AlexWaygood on 2025-02-04 13:58_

---

_@AlexWaygood reviewed on 2025-02-04 15:09_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:71 on 2025-02-04 15:09_

https://github.com/astral-sh/ruff/compare/alex/symbol-fallback-builder?expand=1 is an alternative API, which in some ways I like more. What do you think of that?

---

_Comment by @AlexWaygood on 2025-02-04 17:32_

#15943 is a competing PR to this, which I think I like more overall...

---

_@AlexWaygood reviewed on 2025-02-04 17:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:71 on 2025-02-04 17:33_

(opened #15943)

---

_Closed by @AlexWaygood on 2025-02-05 14:48_

---

_Branch deleted on 2025-02-05 14:48_

---
