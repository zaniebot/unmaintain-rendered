```yaml
number: 21447
title: "[ty] Support stringified annotations in value-position `Annotated` instances"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/annotated-stringified
created_at: 2025-11-14T11:14:35Z
updated_at: 2025-11-14T13:09:10Z
url: https://github.com/astral-sh/ruff/pull/21447
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Support stringified annotations in value-position `Annotated` instances

---

_Pull request opened by @sharkdp on 2025-11-14 11:14_

## Summary

Infer the first argument `type` inside `Annotated[type, …]` as a type expression. This allows us to support stringified annotations inside `Annotated`.

## Ecosystem

* The removed diagnostic on `prefect` shows that we now understand the `State.data` type annotation in `src/prefect/client/schemas/objects.py:230`, which uses a stringified annotation in `Annoated`. The other diagnostics are downstream changes that result from this, it seems to be a commonly used data type.
* `artigraph` does something like `Annotated[cast(Any, field_info.annotation), *field_info.metadata]` which I'm not sure we need to allow? It's unfortunate since this is probably supported at runtime, but it seems reasonable that they need to add a `# type: ignore` for that.
* `pydantic` uses something like `Annotated[(self.annotation, *self.metadata)]` but adds a `# type: ignore`

## Test Plan

New Markdown test

---

_Label `ty` added by @sharkdp on 2025-11-14 11:14_

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 11:16_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-14 11:18_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/bite/collection/infercollectionsabc.py:200:26: error[invalid-type-form] Variable of type `object` is not allowed in a type expression
- Found 498 diagnostics
+ Found 499 diagnostics

artigraph (https://github.com/artigraph/artigraph)
+ src/arti/internal/models.py:101:31: error[invalid-type-form] Function calls are not allowed in type expressions
- Found 174 diagnostics
+ Found 175 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/client/schemas/objects.py:230:13: error[invalid-type-form] Variable of type `Literal["ResultRecordMetadata"]` is not allowed in a type expression
+ src/prefect/states.py:515:22: error[not-iterable] Object of type `~None & ~BaseException & ~str & ~Top[State[Any]]` is not iterable
+ src/prefect/states.py:616:22: error[not-iterable] Object of type `~None & ~BaseException & ~str & ~Top[State[Any]]` is not iterable
+ src/prefect/testing/utilities.py:241:9: error[invalid-argument-type] Argument to bound method `aread` is incorrect: Expected `str`, found `str | None`
+ src/prefect/testing/utilities.py:258:50: error[invalid-argument-type] Argument to bound method `read_block_document` is incorrect: Expected `UUID`, found `UUID | None`
+ src/prefect/testing/utilities.py:269:50: error[invalid-argument-type] Argument to bound method `read_block_document` is incorrect: Expected `UUID`, found `UUID | None`
- Found 3233 diagnostics
+ Found 3237 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:780:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 4818 diagnostics
+ Found 4817 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @sharkdp on 2025-11-14 11:54_

---

_Review requested from @carljm by @sharkdp on 2025-11-14 11:54_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-14 11:54_

---

_Review requested from @dcreager by @sharkdp on 2025-11-14 11:54_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/annotated.md`:85 on 2025-11-14 11:56_

Is it worth also adding a test for a variant of this:

```py
class E(Annotated[list["E"], "metadata"]): ...
```

(which works fine at runtime, I think)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class_base.rs`:195 on 2025-11-14 12:00_

hmmmm I feel like there's probably going to be some edge case someone will come up with at some point where this lossy conversion will come back to bite us... but I'm not sure what they are, and this seems fine for now 😆

---

_@AlexWaygood approved on 2025-11-14 12:02_

---

_Merged by @sharkdp on 2025-11-14 13:09_

---

_Closed by @sharkdp on 2025-11-14 13:09_

---

_Branch deleted on 2025-11-14 13:09_

---
