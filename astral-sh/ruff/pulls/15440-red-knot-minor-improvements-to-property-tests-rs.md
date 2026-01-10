```yaml
number: 15440
title: "[red-knot] Minor improvements to `property_tests.rs`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/property-test-tweaks
created_at: 2025-01-12T13:50:31Z
updated_at: 2025-01-12T16:27:56Z
url: https://github.com/astral-sh/ruff/pull/15440
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Minor improvements to `property_tests.rs`

---

_Pull request opened by @AlexWaygood on 2025-01-12 13:50_

## Summary

- Share helper functions between the `stable` and `flaky` submodules
- Add a comment to `subtype_of_is_antisymmetric` explaining why it can't be marked as stable yet. I very nearly made a PR promoting it to stable -- it ran for 1,000,000 iterations without failing... but then when I ran it once more locally prior to filing the PR, it failed instantly with `Arguments: (Union([BuiltinClassLiteral("str"), KnownClassInstance(Tuple)]), Union([KnownClassInstance(Tuple), BuiltinClassLiteral("str")]))`

## Test Plan

`QUICKCHECK_TESTS=200000 cargo test -p red_knot_python_semantic -- --ignored types::property_tests::stable`

---

_Label `red-knot` added by @AlexWaygood on 2025-01-12 13:50_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-12 13:50_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-12 13:50_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-12 13:50_

---

_Merged by @AlexWaygood on 2025-01-12 13:55_

---

_Closed by @AlexWaygood on 2025-01-12 13:55_

---

_Branch deleted on 2025-01-12 13:55_

---

_Comment by @github-actions[bot] on 2025-01-12 13:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @sharkdp on 2025-01-12 15:53_

> I very nearly made a PR promoting it to stable -- it ran for 1,000,000 iterations without failing...

Did you see the comment at the top regarding running these in release mode?

---

_Comment by @AlexWaygood on 2025-01-12 16:27_

> Did you see the comment at the top regarding running these in release mode?

I've read it and processed it before but I forgot about it 😶 thanks for the reminder!

---
