```yaml
number: 16088
title: "[red-knot] `T | object == object`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/unions-with-object
created_at: 2025-02-10T21:52:03Z
updated_at: 2025-02-10T22:21:47Z
url: https://github.com/astral-sh/ruff/pull/16088
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] `T | object == object`

---

_Pull request opened by @sharkdp on 2025-02-10 21:52_

## Summary

- Simplify unions with `object` to `object`.
- Add a new `Type::object(db)` constructor to abbreviate `KnownClass::Object.to_instance(db)` in some places.
- Add a `Type::is_object` and `Class::is_object` function to make some tests for a bit easier to read.

Happy to revert the latter two changes if these are not desirable.

closes #16084

## Test Plan

New Markdown tests.

---

_Label `red-knot` added by @sharkdp on 2025-02-10 21:52_

---

_Review requested from @carljm by @sharkdp on 2025-02-10 21:52_

---

_Review requested from @MichaReiser by @sharkdp on 2025-02-10 21:52_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-02-10 21:52_

---

_@sharkdp reviewed on 2025-02-10 21:54_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/union_types.md`:149 on 2025-02-10 21:54_

The new simplified result here kind of defeats the purpose of the original test, if I correctly understand its purpose. Should this be changed to something like `int | Unknown | bool` -> `Unknown | int`?

---

_@sharkdp reviewed on 2025-02-10 21:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:66 on 2025-02-10 21:56_

Could also be
```suggestion
            Type::Instance(instance) if instance.class.is_object(self.db) => {
```
but I somehow doubt that it would be much faster(?)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/union_types.md`:149 on 2025-02-10 21:56_

Yes, using `int | Unknown | bool` -> `Unknown | int` looks right to me.

---

_@carljm approved on 2025-02-10 21:58_

Looks great!

---

_@sharkdp reviewed on 2025-02-10 21:59_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:51 on 2025-02-10 21:59_

I also thought about extending the `UnionBuilder` to keep track of the fact that it represented `object`. It could then short-circuit any `.add(…)` into a no-op. Not sure if it's worth the extra complexity though, as we probably don't union with `object` a lot in reality.

---

_@carljm reviewed on 2025-02-10 21:59_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:66 on 2025-02-10 21:59_

I wondered the same thing; I think it could be unpacked even further to match against `class.known` of the `InstanceType` and eliminate the `if` entirely? But in the absence of evidence that that is faster, it just looks more verbose.

---

_@carljm reviewed on 2025-02-10 22:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:51 on 2025-02-10 22:00_

This is the sort of thing IMO best left to testing with actual benchmarks once we ideally have a broader ecosystem check that we can also benchmark.

---

_Merged by @sharkdp on 2025-02-10 22:07_

---

_Closed by @sharkdp on 2025-02-10 22:07_

---

_Branch deleted on 2025-02-10 22:07_

---

_@AlexWaygood reviewed on 2025-02-10 22:21_

Nice!

---
