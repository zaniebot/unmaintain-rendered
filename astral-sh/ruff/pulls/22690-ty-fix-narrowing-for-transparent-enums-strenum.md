```yaml
number: 22690
title: "[ty] Fix narrowing for transparent enums (StrEnum, IntEnum)"
type: pull_request
state: open
author: bxff
labels: []
assignees: []
base: main
head: fix-enum-custom-eq-narrowing
created_at: 2026-01-18T21:14:30Z
updated_at: 2026-01-18T21:14:32Z
url: https://github.com/astral-sh/ruff/pull/22690
synced_at: 2026-01-18T21:20:25Z
```

# [ty] Fix narrowing for transparent enums (StrEnum, IntEnum)

---

_@bxff_

## Summary

This PR fixes incorrect type narrowing in `match` statements for enums with "transparent" equality (`StrEnum`, `IntEnum`, etc.) where enum members compare equal to their underlying primitive values.

**Problem**: When matching a primitive literal (e.g., `Literal["g"]`) against a transparent enum member (e.g., `Color.GREEN`), the type checker incorrectly narrowed the type to `Never` because it treated them as disjoint types. This was reported in issue #1454.

**Root Cause**: 
1. `is_disjoint_from` treated enum literals and primitive literals as disjoint based solely on type differences
2. `IntersectionBuilder` couldn't simplify intersections like `Literal["g"] & ~Color.GREEN` correctly

**Solution**:
1. Added `has_transparent_equality()` in `enums.rs` to explicitly detect enums inheriting from both `Enum` and primitive types (`str`, `int`, `bytes`)
2. Modified `is_disjoint_from_impl()` in `relation.rs` to compare underlying values when dealing with transparent enums instead of just checking type identity
3. Updated `add_negative()` in `builder.rs` to correctly simplify `PrimitiveLiteral & ~TransparentEnumLiteral` to `Never`

## Test Plan

- **New regression tests**: Created `match_enums.md` with targeted tests for transparent enum narrowing in match statements
- All tests pass successfully.

---

_Review requested from @carljm by @bxff on 2026-01-18 21:14_

---

_Review requested from @AlexWaygood by @bxff on 2026-01-18 21:14_

---

_Review requested from @sharkdp by @bxff on 2026-01-18 21:14_

---

_Review requested from @dcreager by @bxff on 2026-01-18 21:14_

---
