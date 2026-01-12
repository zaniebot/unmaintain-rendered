```yaml
number: 19777
title: "[ty] Disallow `typing.TypedDict` in type expressions"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/typeddict-todos
created_at: 2025-08-06T08:02:47Z
updated_at: 2025-08-06T13:58:37Z
url: https://github.com/astral-sh/ruff/pull/19777
synced_at: 2026-01-12T15:56:46Z
```

# [ty] Disallow `typing.TypedDict` in type expressions

---

_@sharkdp_

## Summary

Disallow `typing.TypedDict` in type expressions.

Related reference: https://github.com/python/mypy/issues/11030

## Test Plan

New Markdown tests, checked ecosystem and conformance test impact.

---

_Label `ty` added by @sharkdp on 2025-08-06 08:02_

---

_Comment by @github-actions[bot] on 2025-08-06 08:04_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-06 13:45:47.198830453 +0000
+++ new-output.txt	2025-08-06 13:45:47.262830278 +0000
@@ -885,4 +885,5 @@
 tuples_type_form.py:36:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[2], Literal[3], Literal[""]]` is not assignable to `tuple[int, ...]`
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
-Found 886 diagnostics
+typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
+Found 887 diagnostics
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-06 08:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/v1/annotated_types.py:13:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/v1/annotated_types.py:24:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/v1/validators.py:621:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/v1/validators.py:632:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 772 diagnostics
+ Found 768 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Marked ready for review by @sharkdp on 2025-08-06 08:50_

---

_Review requested from @carljm by @sharkdp on 2025-08-06 08:50_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-08-06 08:50_

---

_Review requested from @dcreager by @sharkdp on 2025-08-06 08:50_

---

_Label `internal` added by @sharkdp on 2025-08-06 08:56_

---

_Label `internal` removed by @sharkdp on 2025-08-06 08:56_

---

_Renamed from "[ty] Resolve two TypedDict-related TODOs" to "[ty] Disallow `typing.TypedDict` in type expressions" by @sharkdp on 2025-08-06 08:56_

---

_@sharkdp reviewed on 2025-08-06 08:57_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:2321 on 2025-08-06 08:57_

Drive-by fix. I don't think it has any impact, as there are no implicit instance attributes on that class. But doesn't hurt to just implement it correctly.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:385 on 2025-08-06 10:17_

nit (and it's fine to leave this for a followup): I wonder if we could be a bit more helpful with this error message -- naively this might feel like a sensible thing to do if you're new to typing? Maybe something like this:

```suggestion
x: TypedDict = {"name": "Alice"}  # error: [invalid-type-form] "Function `typing.TypedDict` is not allowed in type expressions"
```

with a subdiagnostic "note: Consider using `collections.abc.Mapping[str, object]`"?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2321 on 2025-08-06 10:20_

it looks like the `Type::ClassLiteral()` branch of `Type::instance_member()` always returns `Place::Unbound.into()`:

https://github.com/astral-sh/ruff/blob/4887bdf20581f815c2bfa3edc2d93c4e92e6d03b/crates/ty_python_semantic/src/types.rs#L2783-L2789

Should this be `.to_instance(db)` rather than `.to_class_literal(db)`?

---

_@AlexWaygood approved on 2025-08-06 10:20_

---

_@sharkdp reviewed on 2025-08-06 11:13_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:2321 on 2025-08-06 11:13_

Yes — thank you for catching this.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:385 on 2025-08-06 11:16_

I quickly looked into it but it's not straightforward to add subdiagnostics/hints at this position.

Concerning  "**Function** `typing.TypedDict`": I know that it's a function at runtime, but if you're unfamiliar with Python typing, then you might be even more confused by this? "I wanted to use a *type*, not a function…".

Even the Python documentation doesn't reveal that it's a function.

<img width="519" height="261" alt="image" src="https://github.com/user-attachments/assets/3e98bfb0-f8bc-4cea-9d60-eaded09dfddd" />


Leaving the diagnostic as-is for now, but also not opposed to a change.

---

_@sharkdp reviewed on 2025-08-06 11:16_

---

_@AlexWaygood reviewed on 2025-08-06 11:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:385 on 2025-08-06 11:19_

> Concerning "**Function** `typing.TypedDict`": I know that it's a function at runtime, but if you're unfamiliar with Python typing, then you might be even more confused by this? "I wanted to use a _type_, not a function…".

Yeah — the main thing I think might be nice to clarify is that obviously TypedDict _types_ are allowed in type expressions; it's just the TypedDict symbol _itself_ that is not really a type, and is thus disallowed in type expressions. 

Anyway, fine to leave as-is for now!

---

_@sharkdp reviewed on 2025-08-06 13:43_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:385 on 2025-08-06 13:43_

That makes sense. I made an attempt to improve it. With a long message instead of a `.info` for now.

---

_@AlexWaygood reviewed on 2025-08-06 13:56_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:385 on 2025-08-06 13:56_

Thank you, the revised message is great!

---

_@AlexWaygood approved on 2025-08-06 13:56_

---

_Merged by @sharkdp on 2025-08-06 13:58_

---

_Closed by @sharkdp on 2025-08-06 13:58_

---

_Branch deleted on 2025-08-06 13:58_

---
