```yaml
number: 21940
title: "[ty] Stabilize rename"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/stabilize-rename
created_at: 2025-12-12T11:02:22Z
updated_at: 2025-12-12T12:52:49Z
url: https://github.com/astral-sh/ruff/pull/21940
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Stabilize rename

---

_Pull request opened by @MichaReiser on 2025-12-12 11:02_

## Summary

Stabilizes the rename functionality and removes the `experimental.rename` option.

I decided to keep the `Experimental` options section around for now because I forsee us adding new experimental features in the future
and having to redo all this boilerplate seems annoying. 

This PR also fixes a bug where rename didn't work in notebooks because we didn't consider virtual files (files that are just open but have no path) to be part of the project.

Closes https://github.com/astral-sh/ty/issues/1784


## Test Plan

Added e2e tests


---

_Review requested from @carljm by @MichaReiser on 2025-12-12 11:02_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-12 11:02_

---

_Label `server` added by @MichaReiser on 2025-12-12 11:02_

---

_Label `ty` added by @MichaReiser on 2025-12-12 11:02_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-12 11:02_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-12 11:02_

---

_Label `server` added by @MichaReiser on 2025-12-12 11:02_

---

_Label `ty` added by @MichaReiser on 2025-12-12 11:02_

---

_Comment by @astral-sh-bot[bot] on 2025-12-12 11:04_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-12 11:07_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 42 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5133 diagnostics
+ Found 5132 diagnostics

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

_Renamed from "[ty] Stabilizie rename" to "[ty] Stabilize rename" by @AlexWaygood on 2025-12-12 11:24_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-12-12 12:37_

---

_Review comment by @BurntSushi on `crates/ty_server/src/capabilities.rs`:37 on 2025-12-12 12:40_

How come we don't need this any more?

---

_Review comment by @BurntSushi on `crates/ty_server/src/capabilities.rs`:37 on 2025-12-12 12:41_

I guess we don't need to do dynamic registration of it any more since it's always enabled?

---

_@BurntSushi approved on 2025-12-12 12:45_

---

_Review comment by @MichaReiser on `crates/ty_server/src/capabilities.rs`:37 on 2025-12-12 12:52_

Yes, we don't use the dynamic registration capability anymore

---

_@MichaReiser reviewed on 2025-12-12 12:52_

---

_Merged by @MichaReiser on 2025-12-12 12:52_

---

_Closed by @MichaReiser on 2025-12-12 12:52_

---

_Branch deleted on 2025-12-12 12:52_

---
