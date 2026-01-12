```yaml
number: 18911
title: "[ty] Introduce `UnionType::try_from_elements` and `UnionType::try_map`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/trymap
created_at: 2025-06-24T11:28:30Z
updated_at: 2025-06-24T12:09:02Z
url: https://github.com/astral-sh/ruff/pull/18911
synced_at: 2026-01-12T15:56:27Z
```

# [ty] Introduce `UnionType::try_from_elements` and `UnionType::try_map`

---

_@AlexWaygood_

## Summary

An increasingly common pattern in our codebase is to do something like this, where we iterate over a sequence of types and apply a fallible mapping operation to each element in turn:

```rs
                let mut builder = UnionBuilder::new(db);
                for element in union.elements(db) {
                    builder = builder.add(element.to_instance(db)?);
                }
                Some(builder.build())
```

While the mapping function continues to return `Some(Type<'db>)` for each element, we build up a union; but as soon as the mapping operation returns `None` for any element, we short-circuit and return `None`.

This PR adds some helper functions to `UnionType` to make this pattern less verbose: `UnionType::try_from_elements()` and `UnionType::try_map`.

Unfortunately there are a couple of places where a mapping function returns a `Result`:

https://github.com/astral-sh/ruff/blob/27eee5a1a845498aa6a938281cea60c1b1f3f7c9/crates/ty_python_semantic/src/types/infer.rs#L7465-L7480

But I don't think this approach can be generalized to support mapping operations that return `Result`s as well as mapping operations that return `Option`s until [the `Try` trait is stabilised](https://doc.rust-lang.org/std/ops/trait.Try.html) in Rust.

## Test Plan

`cargo test -p ty_python_semantic`


---

_Review requested from @carljm by @AlexWaygood on 2025-06-24 11:28_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-24 11:28_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-24 11:28_

---

_Label `internal` added by @AlexWaygood on 2025-06-24 11:28_

---

_Label `ty` added by @AlexWaygood on 2025-06-24 11:28_

---

_Renamed from "Introduce `UnionType::try_from_elements` and `UnionType::try_map`" to "[ty] Introduce `UnionType::try_from_elements` and `UnionType::try_map`" by @AlexWaygood on 2025-06-24 11:28_

---

_Comment by @github-actions[bot] on 2025-06-24 11:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser reviewed on 2025-06-24 11:38_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:7665 on 2025-06-24 11:38_

Maybe: `try_map` and `try_map_result` or `try_map` and `try_map_ok`?

and `try_from_elements` and `try_from_result_elements`?

I also think it's important to document what "fallible" means because it wasn't clear to me if it bails if there's any `None` value or if it skip sover `None` values.

---

_@MichaReiser approved on 2025-06-24 11:55_

It's unfortunate that the `Try` trait isn't stabilized :( 

---

_@AlexWaygood reviewed on 2025-06-24 12:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7665 on 2025-06-24 12:04_

> Maybe: `try_map` and `try_map_result` or `try_map` and `try_map_ok`?
> 
> and `try_from_elements` and `try_from_result_elements`?

We could, but I don't think it's worth it right now considering that there's only one callsite that uses `Result`s. We can add it later if that pattern becomes more common, I think.

> I also think it's important to document what "fallible" means because it wasn't clear to me if it bails if there's any `None` value or if it skip sover `None` values.

👍 I'll push an update

---

_Merged by @AlexWaygood on 2025-06-24 12:09_

---

_Closed by @AlexWaygood on 2025-06-24 12:09_

---

_Branch deleted on 2025-06-24 12:09_

---
