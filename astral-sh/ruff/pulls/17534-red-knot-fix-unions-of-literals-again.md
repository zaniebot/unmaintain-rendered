```yaml
number: 17534
title: "[red-knot] fix unions of literals, again"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/unionliterals
created_at: 2025-04-22T00:16:15Z
updated_at: 2025-04-22T19:44:54Z
url: https://github.com/astral-sh/ruff/pull/17534
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] fix unions of literals, again

---

_@carljm_

## Summary

#17451 was incomplete. `AlwaysFalsy` and `AlwaysTruthy` are not the only two types that are super-types of some literals (of a given kind) and not others. That set also includes intersections containing `AlwaysTruthy` or `AlwaysFalsy`, and intersections containing literal types of the same kind. Cover these cases as well.

Fixes #17478.

## Test Plan

Added mdtests.

`QUICKCHECK_TESTS=1000000 cargo test -p red_knot_python_semantic -- --ignored types::property_tests::stable` failed on both `all_fully_static_type_pairs_are_subtypes_of_their_union` and `all_type_pairs_are_assignable_to_their_union` prior to this PR, passes after it. 


---

_Label `red-knot` added by @carljm on 2025-04-22 00:16_

---

_Review requested from @AlexWaygood by @carljm on 2025-04-22 00:16_

---

_Review requested from @sharkdp by @carljm on 2025-04-22 00:16_

---

_Review requested from @dcreager by @carljm on 2025-04-22 00:16_

---

_Comment by @github-actions[bot] on 2025-04-22 00:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:58 on 2025-04-22 10:00_

I'm slightly concerned about how much special-casing we're adding for `Type::AlwaysTruthy` and `Type::AlwaysFalsy`, given that conceptually these are really just two protocol types, and we'll be adding a lot more protocol types to our model very soon... but I guess we (I) can cross that bridge when we (I) come to it :-)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:78 on 2025-04-22 10:12_

nit: I find this slightly cleaner (fewer guards in the `match` statement)

```suggestion
    fn splits_literals(self, db: &'db dyn Db, kind: LiteralKind) -> bool {
        match (self, kind) {
            (Type::AlwaysFalsy | Type::AlwaysTruthy, _) => true,
            (Type::StringLiteral(_), LiteralKind::String) => true,
            (Type::BytesLiteral(_), LiteralKind::Bytes) => true,
            (Type::IntLiteral(_), LiteralKind::Int) => true,
            (Type::Intersection(intersection), _) => {
                intersection
                    .positive(db)
                    .iter()
                    .any(|ty| ty.splits_literals(db, kind))
                    || intersection
                        .negative(db)
                        .iter()
                        .any(|ty| ty.splits_literals(db, kind))
            }
            (Type::Union(union), _) => union
                .elements(db)
                .iter()
                .any(|ty| ty.splits_literals(db, kind)),
            _ => false,
        }
    }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/union.md`:187 on 2025-04-22 10:14_

shouldn't this simplify to `object`? Doesn't need to be done in this PR of course, but could add a TODO?

---

_@AlexWaygood approved on 2025-04-22 10:24_

Nice! I also confirmed locally that the property tests are no longer flaky (ran them with 2,000,000 iterations 😄) 

---

_@carljm reviewed on 2025-04-22 14:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:58 on 2025-04-22 14:58_

I think even with the addition of general protocol types, `AlwaysTruthy` and `AlwaysFalsy` will remain "special" in that I don't think any other protocol type can "distinguish" between literals of the same kind?

---

_@AlexWaygood reviewed on 2025-04-22 15:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:58 on 2025-04-22 15:03_

let's hope so!!

---

_@carljm reviewed on 2025-04-22 16:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/union.md`:187 on 2025-04-22 16:01_

Looks like this is also a regression from the unions-of-literals optimization; I won't fix it in this PR but I will fix it.

---

_Merged by @carljm on 2025-04-22 16:12_

---

_Closed by @carljm on 2025-04-22 16:12_

---

_Branch deleted on 2025-04-22 16:12_

---
