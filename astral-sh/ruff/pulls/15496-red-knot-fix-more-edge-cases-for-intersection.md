```yaml
number: 15496
title: "[red-knot] Fix more edge cases for intersection simplification with `LiteralString` and `AlwaysTruthy`/`AlwaysFalsy`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/more-intersections
created_at: 2025-01-15T13:43:23Z
updated_at: 2025-01-15T15:02:43Z
url: https://github.com/astral-sh/ruff/pull/15496
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] Fix more edge cases for intersection simplification with `LiteralString` and `AlwaysTruthy`/`AlwaysFalsy`

---

_@AlexWaygood_

## Summary

Re-reading the diff in https://github.com/astral-sh/ruff/commit/bcf0a715c2d33a638785ba842fd0ec9f608f0932, I realised that there were still one or two cases I failed to handle properly. Namely, we currently simplify `LiteralString & AlwaysTruthy` to `LiteralString & ~Literal[""]`, but we do _not_ do the same simplification for `AlwaysTruthy & LiteralString`. Similarly, we currently simplify `LiteralString & ~AlwaysFalsy` to `LiteralString & ~Literal[""]`, but we don't do the same for `~AlwaysFalsy & LiteralString`.

This PR fixes those cases, and moves all `LiteralString` handling to the top of the `add_positive()` method so that it's easier to think through the various cases and see that they're both complete and correct. The default diff that GitHub shows is a bit messy because the refactoring to `add_positive` means that the indentation increases by one level for most of the function. Everything is _much_ easier to review if you select GitHub's option to view the diff without whitespace changes, however.

## Test Plan

- `cargo test -p red_knot_python_semantic`
- `QUICKCHECK_TESTS=200000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable`
- `uvx pre-commit run -a`


---

_Label `red-knot` added by @AlexWaygood on 2025-01-15 13:43_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-15 13:43_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-15 13:43_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-15 13:43_

---

_Converted to draft by @AlexWaygood on 2025-01-15 13:44_

---

_Marked ready for review by @AlexWaygood on 2025-01-15 13:48_

---

_Comment by @AlexWaygood on 2025-01-15 13:50_

Briefly deleted my whole patch there by accident 😅 it's back now

---

_Comment by @AlexWaygood on 2025-01-15 13:53_

Once we understand differently ordered intersections as equivalent to each other (which I'm working on), we'll be able to add a property test that would have caught this. I've noted it down as a TODO for myself.

---

_Comment by @AlexWaygood on 2025-01-15 13:55_

[No change](https://codspeed.io/astral-sh/ruff/branches/alex%2Fmore-intersections) on the codspeed benchmarks

---

_Comment by @github-actions[bot] on 2025-01-15 13:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2025-01-15 14:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:276 on 2025-01-15 14:01_

it's tempting to think that we could avoid some indirection here, e.g.

```suggestion
            // `AlwaysTruthy & LiteralString` -> `LiteralString & ~Literal[""]`
            Type::LiteralString if self.positive.swap_remove(&Type::AlwaysTruthy) => {
                self.positive.insert(Type::LiteralString);
                self.negative.insert(Type::string_literal(db, ""));
            }
            // `AlwaysFalsy & LiteralString` -> `Literal[""]`
            Type::LiteralString if self.positive.swap_remove(&Type::AlwaysFalsy) => {
                self.positive.insert(Type::string_literal(db, ""));
            }
            // `LiteralString & ~AlwaysTruthy` -> `LiteralString & AlwaysFalsy` -> `Literal[""]`
            Type::LiteralString if self.negative.swap_remove(&Type::AlwaysTruthy) => {
                self.positive.insert(Type::string_literal(db, ""));
            }
            // `LiteralString & ~AlwaysFalsy` -> `LiteralString & ~Literal[""]`
            Type::LiteralString if self.negative.swap_remove(&Type::AlwaysFalsy) => {
                self.positive.insert(Type::LiteralString);
                self.negative.insert(Type::string_literal(db, ""));
            }
```

but this causes many property test failures, because we skip the simplifications done in the fallback branch of this function where redundant supertypes are removed if subtypes exist in the intersection, and where the entire intersection simplifies to `Never` if it contains a pair of disjoint types.

---

_@sharkdp approved on 2025-01-15 15:01_

Thank you!

---

_Merged by @AlexWaygood on 2025-01-15 15:02_

---

_Closed by @AlexWaygood on 2025-01-15 15:02_

---

_Branch deleted on 2025-01-15 15:02_

---
