```yaml
number: 17533
title: "[red-knot] Class literal `__new__` function callable subtyping"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: class-literal-new-function-callable-subtyping
created_at: 2025-04-21T23:21:44Z
updated_at: 2025-04-25T20:55:09Z
url: https://github.com/astral-sh/ruff/pull/17533
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Class literal `__new__` function callable subtyping

---

_Pull request opened by @MatthewMckee4 on 2025-04-21 23:21_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

From https://typing.python.org/en/latest/spec/constructors.html#converting-a-constructor-to-callable

this covers step 2 and partially step 3 (always respecting the `__new__`)

## Test Plan

Update is_subtype_of.md

---

_Review requested from @carljm by @MatthewMckee4 on 2025-04-21 23:21_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-04-21 23:21_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-04-21 23:21_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-04-21 23:21_

---

_Comment by @github-actions[bot] on 2025-04-21 23:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1169 on 2025-04-22 00:26_

```suggestion

// TODO handle `__init__` also
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:5949 on 2025-04-22 00:28_

nit: I don't think the `_type` suffix adds anything here, we can just call this `into_bound_method`.

If we are going to add this utility, it seems like there are quite a few other places we may as well use it -- basically everywhere we are currently calling `BoundMethodType::new`, I think?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1206 on 2025-04-22 00:30_

````suggestion
```

#### Classes with `__call__` and `__new__`

If `__call__` and `__new__` are both present, `__call__` takes precedence.

```py
````

---

_@carljm reviewed on 2025-04-22 00:31_

Looks good, thank you! Just a couple comments.

---

_@MatthewMckee4 reviewed on 2025-04-22 00:37_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types.rs`:5949 on 2025-04-22 00:37_

I added it to match the function above

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:5949 on 2025-04-22 00:46_

Ah ok. I don't think either of them needs it (the `_type` suffix), but it really doesn't matter either way, fine to leave it as is

---

_@carljm reviewed on 2025-04-22 00:46_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1203 on 2025-04-22 00:48_

```suggestion
```py
from typing import Callable
from knot_extensions import TypeOf, static_assert, is_subtype_of

```


---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types.rs`:1170 on 2025-04-22 00:49_

```suggestion
            // TODO handle `__init__` also
```


---

_@MatthewMckee4 reviewed on 2025-04-22 00:50_

---

_Label `red-knot` added by @MichaReiser on 2025-04-22 06:11_

---

_@AlexWaygood reviewed on 2025-04-22 10:47_

---

_Review comment by @AlexWaygood on `playground/shared/package-lock.json`:1 on 2025-04-22 10:47_

need to delete this file :-)

---

_@MatthewMckee4 reviewed on 2025-04-22 11:41_

---

_Review comment by @MatthewMckee4 on `playground/shared/package-lock.json`:1 on 2025-04-22 11:41_

Sorry, will do soon

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:1167 on 2025-04-22 15:57_

nit:
```suggestion
                    return new_function.into_bound_method_type(db, self).is_subtype_of(db, target);
```

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:5952 on 2025-04-22 16:00_

The parameter name is a bit misleading because in this PR above where this is being called it's not an instance of the class but the class itself. This is also the case for the field name on `BoundMethodType` which is also called `self_instance`. I'm not sure what to name it, maybe `self_or_cls` (and remove the `_instance` suffix).

Regardless, this shouldn't block this PR and something that could be done in a follow-up instead.

---

_@dhruvmanila approved on 2025-04-22 16:06_

Thank you.

I think it would be useful to create a `into_callable` method on `ClassLiteralType` to avoid expanding the `is_subtype_of` and to keep the match arms simple.

---

_Comment by @MatthewMckee4 on 2025-04-22 16:12_

Sounds good

---

_@MatthewMckee4 reviewed on 2025-04-22 18:02_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/class.rs`:830 on 2025-04-22 18:02_

For some reason this wasnt working
```rs
        let metaclass_call_function_symbol = self
            .class_member(
                db,
                "__call__",
                MemberLookupPolicy::NO_INSTANCE_FALLBACK
                    | MemberLookupPolicy::META_CLASS_NO_TYPE_FALLBACK,
            )
            .symbol;
```
It couldn't find `__call__`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:830 on 2025-04-23 05:39_

I suspect it found it, but it wasn't a `BoundMethod`. That's because it's `Type::member_lookup_with_policy` that implements the descriptor protocol, which turns a `FunctionType` into a `BoundMethod`.

---

_@carljm approved on 2025-04-23 05:40_

Looks good, thank you!

---

_Merged by @carljm on 2025-04-23 05:40_

---

_Closed by @carljm on 2025-04-23 05:40_

---

_Branch deleted on 2025-04-25 20:55_

---
