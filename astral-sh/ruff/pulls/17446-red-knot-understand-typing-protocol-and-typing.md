```yaml
number: 17446
title: "[red-knot] Understand `typing.Protocol` and `typing_extensions.Protocol` as equivalent"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/typing-extensions-protocol
created_at: 2025-04-17T11:54:41Z
updated_at: 2025-04-17T20:54:25Z
url: https://github.com/astral-sh/ruff/pull/17446
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Understand `typing.Protocol` and `typing_extensions.Protocol` as equivalent

---

_Pull request opened by @AlexWaygood on 2025-04-17 11:54_

## Summary

We currently have special-casing that avoids false positives for `typing.Protocol`, but the special-casing doesn't currently recognise `typing_extensions.Protocol` as also being a valid class base, etc. Due to various bugfixes and performance optimisations having been backported via typing_extensions, `typing_extensions.Protocol` is a different symbol to `typing.Protocol` on most Python versions: it would be wrong for us to infer `Literal[True]` as the result of the expression `typing_extensions.Protocol is typing.Protocol`, for example. Nonetheless, they should be treated identically by a type checker in terms of their semantics. This PR implements that.

## Test Plan

Existing mdtests have been updated to reflect that we no longer issue false-positive diagnostics on classes inheriting from `typing_extensions.Protocol`.


---

_Label `red-knot` added by @AlexWaygood on 2025-04-17 11:54_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-17 11:54_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-17 11:54_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-17 11:54_

---

_Comment by @github-actions[bot] on 2025-04-17 11:56_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dacite (https://github.com/konradhalas/dacite)
- error[lint:invalid-base] /tmp/mypy_primer/projects/dacite/dacite/data.py:8:12: Invalid class base with type `typing.Protocol | _SpecialForm` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/dacite/dacite/data.py:11:48: Function can implicitly return `None`, which is not assignable to return type `bool`
- Found 158 diagnostics
+ Found 156 diagnostics

```
</details>


---

_@AlexWaygood reviewed on 2025-04-17 11:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:264 on 2025-04-17 11:57_

I suspect that there are other `KnownInstanceType` variants where we might currently incorrectly assume that the `typing_extensions` version of the symbol shares the same memory address as the `typing` version of the symbol, which would lead us to incorrect inferences in things like this

---

_Converted to draft by @AlexWaygood on 2025-04-17 12:15_

---

_Comment by @AlexWaygood on 2025-04-17 12:36_

> ## `mypy_primer` results
> Changes were detected when running on open source projects
> 
> ```diff
> dacite (https://github.com/konradhalas/dacite)
> + error[lint:invalid-base] /tmp/mypy_primer/projects/dacite/dacite/data.py:8:12: Invalid class base with type `typing.Protocol | typing_extensions.Protocol` (all bases must be a class, `Any`, `Unknown` or `Todo`)
> - error[lint:invalid-base] /tmp/mypy_primer/projects/dacite/dacite/data.py:8:12: Invalid class base with type `typing.Protocol | _SpecialForm` (all bases must be a class, `Any`, `Unknown` or `Todo`)
> ```

This shows the cost of treating the symbols `typing_extensions.Protocol` and `typing.Protocol` as inhabiting distinct types: if they were understood by us as inhabiting exactly the same type, then we wouldn't infer a union type for the `Protocol` symbol [here](https://github.com/konradhalas/dacite/blob/9898ccbb783e7e6a35ae165e7deb9fa84edfe21c/dacite/data.py#L1-L4), so we wouldn't emit that diagnostic. I think instead of my current approach, I'll switch to treating them as inhabiting exactly the same type, but rework some of the logic around `KnownInstanceType` so that we no longer assume a type is a singleton type if it's a `KnownInstanceType`. This doesn't necessarily hold true if the symbol could originate from either `typing` or `typing_extensions`.

---

_Comment by @AlexWaygood on 2025-04-17 12:53_

The mypy_primer results with the new approach look much better!

---

_Marked ready for review by @AlexWaygood on 2025-04-17 12:53_

---

_@carljm approved on 2025-04-17 20:52_

Looks great!

---

_Merged by @AlexWaygood on 2025-04-17 20:54_

---

_Closed by @AlexWaygood on 2025-04-17 20:54_

---

_Branch deleted on 2025-04-17 20:54_

---
