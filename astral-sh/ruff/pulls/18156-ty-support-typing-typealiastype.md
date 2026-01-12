```yaml
number: 18156
title: "[ty] Support `typing.TypeAliasType`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/bare-type-alias-type
created_at: 2025-05-17T20:12:05Z
updated_at: 2025-05-19T14:36:50Z
url: https://github.com/astral-sh/ruff/pull/18156
synced_at: 2026-01-12T15:56:13Z
```

# [ty] Support `typing.TypeAliasType`

---

_@sharkdp_

## Summary

Support direct uses of `typing.TypeAliasType`, as in:

```py
from typing import TypeAliasType

IntOrStr = TypeAliasType("IntOrStr", int | str)

def f(x: IntOrStr) -> None:
    reveal_type(x)  # revealed: int | str
```

closes https://github.com/astral-sh/ty/issues/392

## Ecosystem

The new false positive here:
```diff
+ error[invalid-type-form] altair/utils/core.py:49:53: The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
```
comes from the fact that we infer the second argument as a type expression now. We silence false positives for PEP695 `ParamSpec`s, but not for `P = ParamSpec("P")` inside `Callable[P, ...]`.

## Test Plan

New Markdown tests


---

_Label `ty` added by @sharkdp on 2025-05-17 20:12_

---

_Comment by @github-actions[bot] on 2025-05-17 20:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
altair (https://github.com/vega/altair)
+ error[invalid-type-form] altair/utils/core.py:49:53: The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- error[invalid-argument-type] altair/utils/core.py:48:58: Argument to bound method `__init__` is incorrect: Expected `tuple[TypeVar | ParamSpec | TypeVarTuple, ...]`, found `tuple[typing.TypeVar]`
- error[invalid-argument-type] altair/utils/core.py:49:60: Argument to bound method `__init__` is incorrect: Expected `tuple[TypeVar | ParamSpec | TypeVarTuple, ...]`, found `tuple[ParamSpec, typing.TypeVar]`
- error[invalid-argument-type] altair/utils/core.py:53:56: Argument to bound method `__init__` is incorrect: Expected `tuple[TypeVar | ParamSpec | TypeVarTuple, ...]`, found `tuple[typing.TypeVar, typing.TypeVar]`
- error[invalid-argument-type] altair/utils/core.py:56:54: Argument to bound method `__init__` is incorrect: Expected `tuple[TypeVar | ParamSpec | TypeVarTuple, ...]`, found `tuple[typing.TypeVar, ParamSpec, typing.TypeVar]`
- error[invalid-argument-type] altair/utils/plugin_registry.py:25:52: Argument to bound method `__init__` is incorrect: Expected `tuple[TypeVar | ParamSpec | TypeVarTuple, ...]`, found `tuple[typing.TypeVar]`
- error[invalid-argument-type] altair/utils/schemapi.py:903:56: Argument to bound method `__init__` is incorrect: Expected `tuple[TypeVar | ParamSpec | TypeVarTuple, ...]`, found `tuple[typing.TypeVar]`
- error[invalid-argument-type] altair/vegalite/v5/schema/_typing.py:105:61: Argument to bound method `__init__` is incorrect: Expected `tuple[TypeVar | ParamSpec | TypeVarTuple, ...]`, found `tuple[typing.TypeVar]`
- error[invalid-context-manager] tests/vegalite/v5/test_api.py:1409:10: Object of type `Unknown | PluginEnabler[@Todo(unknown type subscript), ThemeConfig]` cannot be used with `with` because it does not correctly implement `__exit__`
+ error[invalid-context-manager] tests/vegalite/v5/test_api.py:1409:10: Object of type `Unknown | PluginEnabler[@Todo(Generic PEP-695 type alias), ThemeConfig]` cannot be used with `with` because it does not correctly implement `__exit__`
- error[invalid-context-manager] tests/vegalite/v5/test_api.py:1414:10: Object of type `Unknown | PluginEnabler[@Todo(unknown type subscript), ThemeConfig]` cannot be used with `with` because it does not correctly implement `__exit__`
+ error[invalid-context-manager] tests/vegalite/v5/test_api.py:1414:10: Object of type `Unknown | PluginEnabler[@Todo(Generic PEP-695 type alias), ThemeConfig]` cannot be used with `with` because it does not correctly implement `__exit__`
- error[invalid-context-manager] tests/vegalite/v5/test_api.py:1420:10: Object of type `Unknown | PluginEnabler[@Todo(unknown type subscript), ThemeConfig]` cannot be used with `with` because it does not correctly implement `__exit__`
+ error[invalid-context-manager] tests/vegalite/v5/test_api.py:1420:10: Object of type `Unknown | PluginEnabler[@Todo(Generic PEP-695 type alias), ThemeConfig]` cannot be used with `with` because it does not correctly implement `__exit__`
- error[invalid-context-manager] tests/vegalite/v5/test_theme.py:48:14: Object of type `Unknown | PluginEnabler[@Todo(unknown type subscript), ThemeConfig]` cannot be used with `with` because it does not correctly implement `__exit__`
+ error[invalid-context-manager] tests/vegalite/v5/test_theme.py:48:14: Object of type `Unknown | PluginEnabler[@Todo(Generic PEP-695 type alias), ThemeConfig]` cannot be used with `with` because it does not correctly implement `__exit__`
- Found 1349 diagnostics
+ Found 1343 diagnostics

```
</details>


---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:7558 on 2025-05-17 20:16_

This is used for go-to-type-definition. It seems more involved to provide the original definition here, as we would need to keep track of it through various `Type` operations and the calling machinery.

---

_@sharkdp reviewed on 2025-05-17 20:16_

---

_Marked ready for review by @sharkdp on 2025-05-17 20:36_

---

_Review requested from @carljm by @sharkdp on 2025-05-17 20:36_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-17 20:36_

---

_Review requested from @dcreager by @sharkdp on 2025-05-17 20:36_

---

_Review requested from @MichaReiser by @sharkdp on 2025-05-17 20:36_

---

_@sharkdp reviewed on 2025-05-17 20:39_

---

_Review comment by @sharkdp on `crates/ty_project/tests/check.rs`:300 on 2025-05-17 20:39_

This is unfortunate, but that file could just as easily have been written using PEP 695 type aliases …

---

_@AlexWaygood reviewed on 2025-05-17 21:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7537 on 2025-05-17 21:09_

nit: I think `ast::name::Name` would probably be slightly more efficient, as `Name` wraps a `CompactString`, and the names of type aliases are likely to nearly always be pretty small strings

```suggestion
    pub name: ast::name::Name,
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:923 on 2025-05-18 14:07_

If you move this construction logic into `infer.rs`, like we do for legacy typevars, you'll be able to get the `Definition` of the assignment statement, at least for the case where a call to the `TypeAliasType` constructor is being immediately assigned to a variable. That will let you return a non-`None` result for `definition` up above for bare type aliases too.

https://github.com/astral-sh/ruff/blob/dd04ca7f5885b7bc96303dbb5ce7635a039d2f93/crates/ty_python_semantic/src/types/infer.rs#L5197-L5199

(Moving the construction logic also made it easier to emit diagnostics for legacy typevar construction, which might help you discharge the TODO comment)

---

_@dcreager reviewed on 2025-05-18 14:09_

---

_@sharkdp reviewed on 2025-05-19 07:39_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/call/bind.rs`:923 on 2025-05-19 07:39_

Thank you — I didn't realize we had call-related code in `infer.rs`, too. That was really helpful. TODO comment is also resolved.

---

_@sharkdp reviewed on 2025-05-19 07:41_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/diagnostic.rs`:607 on 2025-05-19 07:41_

I wasn't sure if we should have the same strict requirements here as for legacy `TypeVar` constructions. I couldn't find anything in the spec, but pyright also emits diagnostics, if a `TypeAliasType` is not immediately assigned to a variable, for example.

---

_Review comment by @dcreager on `crates/ty/docs/rules.md`:53 on 2025-05-19 14:30_

~Did you have to update these manually?~

Whew, you did not! Looks like this was auto-generated from `diagnostic.rs`

[Not for this PR] We might consider marking this as a generated file in `.gitattributes` to hide them in PR diffs

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/diagnostic.rs`:607 on 2025-05-19 14:34_

Works for me!

---

_@dcreager approved on 2025-05-19 14:35_

---

_Merged by @sharkdp on 2025-05-19 14:36_

---

_Closed by @sharkdp on 2025-05-19 14:36_

---

_Branch deleted on 2025-05-19 14:36_

---
