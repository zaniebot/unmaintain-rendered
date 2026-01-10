```yaml
number: 15766
title: "[red-knot] Document public symbol type inference"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/public-type-tests
created_at: 2025-01-27T08:45:34Z
updated_at: 2025-01-27T19:34:10Z
url: https://github.com/astral-sh/ruff/pull/15766
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Document public symbol type inference

---

_Pull request opened by @sharkdp on 2025-01-27 08:45_

## Summary

Adds a slightly more comprehensive documentation of our behavior regarding type inference for public uses of symbols. In particular:

- What public type do we infer for `x: int = any()`?
- What public type do we infer for `x: Unknown = 1`?



---

_Label `testing` added by @sharkdp on 2025-01-27 08:45_

---

_Label `red-knot` added by @sharkdp on 2025-01-27 08:45_

---

_Review requested from @carljm by @sharkdp on 2025-01-27 08:45_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-27 08:45_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-27 08:45_

---

_@sharkdp reviewed on 2025-01-27 08:47_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness/public.md`:222 on 2025-01-27 08:47_

This behavior is the main thing I wanted to document.

---

_Merged by @sharkdp on 2025-01-27 09:52_

---

_Closed by @sharkdp on 2025-01-27 09:52_

---

_Branch deleted on 2025-01-27 09:52_

---

_Renamed from "[red-knot] Document public symbol type inferece" to "[red-knot] Document public symbol type inference" by @AlexWaygood on 2025-01-27 12:11_

---

_@carljm reviewed on 2025-01-27 18:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness/public.md`:215 on 2025-01-27 18:20_

I think something that is noteworthy here is that the entire purpose of `Unknown` (as distinct from `Any`) is to distinguish a dynamic type that originates from an explicit annotation vs one that originates from lack of annotation. So the whole idea of "declared with `Unknown`" is a contradiction. We have made it _possible_ by introducing `knot_extensions.Unknown`, but perhaps that is just confusing and was a mistake? It should really only be used in tests, but I would guess that in most cases we could structure the test so as to generate an `Unknown` type via lack-of-annotation instead (e.g. an un-annotated function parameter).

---

_@carljm reviewed on 2025-01-27 19:34_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness/public.md`:215 on 2025-01-27 19:34_

Actually I just realized that "declared with Unknown" is not a contradiction; it could occur anytime someone annotates with a name we can't resolve, or an invalid type expression.

Still not totally clear to me whether tests for this case are better written using `knot_extensions.Unknown` (which is clear and explicit, but not "realistic") or using an actual unknown name (which then requires suppressing the unknown-name diagnostic.) I can see good arguments both ways, so no strong feelings.

---
