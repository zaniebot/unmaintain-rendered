```yaml
number: 21695
title: "[ty] Move `all_members`, and related types/routines, out of `ide_support.rs`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/all-members
created_at: 2025-11-29T16:34:39Z
updated_at: 2025-12-02T14:45:26Z
url: https://github.com/astral-sh/ruff/pull/21695
synced_at: 2026-01-12T15:57:31Z
```

# [ty] Move `all_members`, and related types/routines, out of `ide_support.rs`

---

_@AlexWaygood_

## Summary

These routines and types are now used by several modules elsewhere in `ty_python_semantic` that have little to do with supporting IDE usecases. @carljm asked in https://github.com/astral-sh/ruff/pull/21436#discussion_r2539331295 for these to therefore be moved out of this module and put somewhere else. I said I'd do that as a followup, so here's the followup!

## Test Plan

Existing tests all pass


---

_Review requested from @carljm by @AlexWaygood on 2025-11-29 16:34_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-29 16:34_

---

_Label `internal` added by @AlexWaygood on 2025-11-29 16:34_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-29 16:34_

---

_Label `ty` added by @AlexWaygood on 2025-11-29 16:34_

---

_Comment by @astral-sh-bot[bot] on 2025-11-29 16:36_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-29 16:38_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 45 diagnostics
+ Found 41 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1207:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5815 diagnostics
+ Found 5814 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@MichaReiser reviewed on 2025-12-01 07:42_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/list_members.rs`:19 on 2025-12-01 07:42_

This function seems unrelated to `members`. We also use it to compute the completions for the definitions in the current scope. That's why I think `all_members.rs` isn't a great fit for it. 


I think I'd move it into `semantic_index.rs` alongside `attribute_declarations`


---

_@AlexWaygood reviewed on 2025-12-01 08:06_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/list_members.rs`:19 on 2025-12-01 08:06_

This function returns an Iterator of `MemberWithDefinition`, which is a type defined in this module and which is intended to be a much higher-level API than what the semantic index has to offer. So to me, it feels very much like it belongs in this module — but maybe we should rename the module? `list_members.rs` rather than `all_members.rs`?

---

_@MichaReiser reviewed on 2025-12-01 09:17_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/list_members.rs`:19 on 2025-12-01 09:17_

I think it depends on the terminology we want here. We should either:

* Rename the method to `all_members`
* Rename `MemberWithDefinition` to something else, making it clear it's not just class members

What's unclear to me is if `Member` should be reserved to class members only (in which case we should rename the return type) or if we're fine using it for all sort of members (in which case we should rename the method)

---

_@AlexWaygood reviewed on 2025-12-01 13:22_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/list_members.rs`:19 on 2025-12-01 13:22_

I've renamed the module and updated some docs. LMK what you think.

> What's unclear to me is if `Member` should be reserved to class members only (in which case we should rename the return type) or if we're fine using it for all sort of members (in which case we should rename the method)

We already use the term "member" across the codebase much more broadly than just to refer to "class members only" -- for example, the `Type::member()` method, which can be called on any type (not just class types or instance types). And `all_members()` can also be called on any type, not just a class or an instance.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/list_members.rs`:19 on 2025-12-02 09:13_

Should we call this `all_member_definitions`? 

---

_@MichaReiser approved on 2025-12-02 09:13_

---

_Merged by @AlexWaygood on 2025-12-02 14:45_

---

_Closed by @AlexWaygood on 2025-12-02 14:45_

---

_Branch deleted on 2025-12-02 14:45_

---
