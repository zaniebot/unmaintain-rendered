```yaml
number: 21667
title: "[ty] Add IDE autofixes for two \"Did you mean...?\" suggestions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/fixes
created_at: 2025-11-27T17:14:45Z
updated_at: 2025-11-29T16:10:06Z
url: https://github.com/astral-sh/ruff/pull/21667
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Add IDE autofixes for two "Did you mean...?" suggestions

---

_Pull request opened by @AlexWaygood on 2025-11-27 17:14_

## Summary

Add two autofixes. One will allow you to easily fix a typo in a `TypedDict` string-literal key; the other allows you to fix `x: datetime` to `x: datetime.datetime`.

Most of our other "Did you mean...?" suggestions are surprisingly hard to add fixes for with our current infrastructure. E.g., it would be great to add a fix that switches `raise NotImplemented` to `raise NotImplementedError`, but you first have to check whether the builtin symbol `NotImplementedError` has been shadowed anywhere in this module. It's doable, but I don't want to spend time building that infrastructure _right_ now :-)

## Test Plan

Snapshots added. And, manual testing in the playground:

https://github.com/user-attachments/assets/1b7f4c72-bed2-4508-9359-1248288bd81b

---

_Review requested from @carljm by @AlexWaygood on 2025-11-27 17:14_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-27 17:14_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-27 17:14_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-11-27 17:14_

---

_Label `ty` added by @AlexWaygood on 2025-11-27 17:14_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-27 17:14_

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 17:16_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_@AlexWaygood reviewed on 2025-11-27 17:18_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Misspelled_key_for_`…_(7cf0fa634e2a2d59).snap`:42 on 2025-11-27 17:18_

it feels a bit weird to me that the

```
info: rule `invalid-key` is enabled by default
```

note comes above the rendering of the fix. If I add a `help` subdiagnostic (which is currently the only way to give the quick-fix a title in the IDE), the `help` subdiagnostic comes before this note, so the `help` subdiagnostic doesn't look like a fix-title at all when it's rendered in a snapshot (or the CLI, though we don't offer fixes on the CLI yet).

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 17:18_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 505 diagnostics
+ Found 507 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/accounting/structures/processed_event.py:85:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/api/rest.py:1039:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/api/rest.py:2896:25: error[invalid-argument-type] Argument to function `reset_events_for_redecode` is incorrect: Expected `Literal[Location.ETHEREUM, Location.OPTIMISM, Location.POLYGON_POS, Location.ARBITRUM_ONE, Location.BASE, ... omitted 7 literals]`, found `Location | Unknown`
- Found 2042 diagnostics
+ Found 2041 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-27 17:38_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:8917 on 2025-11-27 18:07_

Remove the TODO on line 8914

---

_Review comment by @MichaReiser on `crates/ty_test/src/lib.rs`:648 on 2025-11-27 18:07_

We should also enable this for the CLI or this will be another instance where we think this has always existed but never shipped to users, similar to showing fixes in Ruff 😆 

https://github.com/astral-sh/ruff/blob/df66946b8959589307e4278b0fd5c1d0b187e854/crates/ty/src/lib.rs#L314-L317


---

_@Gankra approved on 2025-11-27 18:09_

Nice!

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Misspelled_key_for_`…_(7cf0fa634e2a2d59).snap`:42 on 2025-11-27 18:11_

I agree. But that's rather tricky to fix because the diff rendering isn't really integrated into annotated-snippets. Instead, we first render the diagnostic with annotated-snippets, but the fix is rendered separately, only mimicking the annotated-snippets design.

---

_@MichaReiser approved on 2025-11-27 18:12_

Sweet

---

_@AlexWaygood reviewed on 2025-11-27 18:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:8917 on 2025-11-27 18:13_

Whooops, thanks!

---

_Renamed from "[ty] Add autofixes for two "Did you mean...?" suggestions" to "[ty] Add IDE autofixes for two "Did you mean...?" suggestions" by @AlexWaygood on 2025-11-27 18:16_

---

_Merged by @AlexWaygood on 2025-11-27 18:20_

---

_Closed by @AlexWaygood on 2025-11-27 18:20_

---

_Branch deleted on 2025-11-27 18:20_

---

_@AlexWaygood reviewed on 2025-11-29 16:10_

---

_Review comment by @AlexWaygood on `crates/ty_test/src/lib.rs`:648 on 2025-11-29 16:10_

I opened https://github.com/astral-sh/ty/issues/1679

---
