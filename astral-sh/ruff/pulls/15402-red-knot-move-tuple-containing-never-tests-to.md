```yaml
number: 15402
title: "[red-knot] Move tuple-containing-Never tests to Markdown"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/tuples-containing-never
created_at: 2025-01-10T14:26:49Z
updated_at: 2025-01-10T15:33:26Z
url: https://github.com/astral-sh/ruff/pull/15402
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Move tuple-containing-Never tests to Markdown

---

_Pull request opened by @sharkdp on 2025-01-10 14:26_

## Summary

See title.

Part of #15397

## Test Plan

Ran new Markdown test.

---

_Label `red-knot` added by @sharkdp on 2025-01-10 14:26_

---

_Review requested from @carljm by @sharkdp on 2025-01-10 14:26_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-10 14:26_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-10 14:26_

---

_Comment by @github-actions[bot] on 2025-01-10 14:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/tuples_containing_never.md`:6 on 2025-01-10 14:46_

I think this confuses the ideas of runtime tuples and tuple _types_ a little bit... what about something like this?

```suggestion
A tuple type that has `Never` as a type argument simplifies to `Never`. One way to think about this is the following. For any heteregenous tuple type `T`, `T` will have type arguments E<sub>1</sub>, E<sub>2</sub>, E<sub>3</sub>... where each type argument represents the type a runtime tuple element must have at a given index if the runtime tuple as a whole can be said to inhabit `T`. Therefore, to construct a runtime tuple `t` that inhabits `T`, it must be possible to construct a runtime object that inhabits E<sub>2</sub>, E<sub>3</sub>, a runtime object that inhabits E<sub>2</sub>, E<sub>3</sub>... But since no runtime objects can ever inhabit the type `Never`, if `Never` is present as one of the type arguments for `T` you can therefore not construct any inhabitants of `T` at runtime. `T` is therefore uninhabited, and therefore equivalent to `Never`. 
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/tuples_containing_never.md`:8 on 2025-01-10 14:47_

```suggestion
In the language of algebraic data types, a tuple type is a product type and `Never` acts like the zero
```

---

_@AlexWaygood approved on 2025-01-10 14:48_

---

_@sharkdp reviewed on 2025-01-10 15:01_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/tuples_containing_never.md`:6 on 2025-01-10 15:01_

Hm, to be honest, I'm not sure if that helps with clarity? What part of my description confused runtime tuples with tuple types? The word "contains"? Maybe I should have written it as
```suggestion
A heterogeneous `tuple[…]` type that contains `Never` as a type argument simplifies to `Never`.
One way to think about this is the following: in order to construct a tuple, you need to have an
object of every element type. But since there is no object of type `Never`, you cannot construct
the tuple. Such a tuple type is therefore uninhabited and equivalent to `Never`.
```

---

_@AlexWaygood reviewed on 2025-01-10 15:07_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/tuples_containing_never.md`:6 on 2025-01-10 15:07_

> What part of my description confused runtime tuples with tuple types? The word "contains"?

yes -- when you say "a `tuple` that contains `Never`, I think the natural understanding of `tuple` there is "a runtime tuple" rather than "a tuple type", and even for a tuple _type_ I wouldn't really say that it "contains `Never`"

> Maybe I should have written it as

that rewrite looks great! "can not" should be "cannot" here, though

---

_Merged by @sharkdp on 2025-01-10 15:31_

---

_Closed by @sharkdp on 2025-01-10 15:31_

---

_Branch deleted on 2025-01-10 15:31_

---
