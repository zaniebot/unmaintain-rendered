```yaml
number: 21943
title: "[ty] Fix outdated version in publish diagnostics after `didChange`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/fix-outdated-publish-diagnostics-version
created_at: 2025-12-12T11:19:02Z
updated_at: 2025-12-12T11:30:58Z
url: https://github.com/astral-sh/ruff/pull/21943
synced_at: 2026-01-12T15:57:37Z
```

# [ty] Fix outdated version in publish diagnostics after `didChange`

---

_@MichaReiser_

## Summary

Fixes the bug reported in https://github.com/astral-sh/ty/issues/1652#issuecomment-3645976277

`publish_diagnostics` used the version of `DocumentHandle::version`. However, we didn't update
that version in the `didChange` handler, resulting in ty publishing diagnostics for the previous
version rather than the most recent version (making clients ignore the diagnostics).

This PR fixes this by updating the version on `DocumentHandle` when updating the document.

## Test Plan

Added E2E test


---

_Label `bug` added by @MichaReiser on 2025-12-12 11:19_

---

_Label `server` added by @MichaReiser on 2025-12-12 11:19_

---

_Label `ty` added by @MichaReiser on 2025-12-12 11:19_

---

_Review requested from @carljm by @MichaReiser on 2025-12-12 11:19_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-12 11:19_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-12 11:19_

---

_Comment by @astral-sh-bot[bot] on 2025-12-12 11:20_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-12 11:22_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_Review requested from @dhruvmanila by @MichaReiser on 2025-12-12 11:23_

---

_Review request for @dcreager removed by @MichaReiser on 2025-12-12 11:23_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-12-12 11:23_

---

_@dhruvmanila approved on 2025-12-12 11:26_

Nice!

Should we add a test case for pull diagnostics as well or rather update an existing test case to assert the diagnostic version?

---

_Comment by @MichaReiser on 2025-12-12 11:30_

> Should we add a test case for pull diagnostics as well or rather update an existing test case to assert the diagnostic version?

The pull diagnostics response doesn't contain the version

---

_Merged by @MichaReiser on 2025-12-12 11:30_

---

_Closed by @MichaReiser on 2025-12-12 11:30_

---

_Branch deleted on 2025-12-12 11:30_

---
