```yaml
number: 392
title: "support for bare `TypeAliasType`"
type: issue
state: closed
author: jorenham
labels:
  - feature
  - help wanted
  - typing semantics
assignees: []
created_at: 2025-05-14T18:14:51Z
updated_at: 2025-05-19T14:36:50Z
url: https://github.com/astral-sh/ty/issues/392
synced_at: 2026-01-12T15:54:23Z
```

# support for bare `TypeAliasType`

---

_@jorenham_

For example `Unit = TypeAliasType("Unit", tuple[()])` is reported as `error[invalid-type-form]`, independent of whether `TypeAliasType` is imported from `typing` or `typing_extensions`.

https://play.ty.dev/9903c4a3-6d05-4663-b282-4731ed610369

---

_Label `feature` added by @carljm on 2025-05-14 18:24_

---

_Label `help wanted` added by @carljm on 2025-05-14 18:24_

---

_Comment by @carljm on 2025-05-14 18:24_

Thanks for the report! I guess the use case here is using `TypeAliasType` from `typing_extensions` in order to use PEP 695 type aliases on Python versions that don't have PEP 695 syntax.

(In case it's not clear to another reader of this issue, `Unit = TypeAliasType("Unit", tuple[()])` has the same runtime effect as the PEP 695 syntactic sugar `type Unit = tuple[()]`.)

We already support the latter, we should ideally support the former as well. Shouldn't be a difficult change, I don't think; we already have a `KnownClass::TypeAliasType` and it already recognizes it from both `typing` and `typing_extensions`, we just need to add a special known-case for evaluating calls to it.

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-14 18:27_

---

_Comment by @jorenham on 2025-05-14 18:43_

> I guess the use case here is using `TypeAliasType` from `typing_extensions` in order to use PEP 695 type aliases on Python versions that don't have PEP 695 syntax.

Another use-case is to control the how certain type-checkers report it. Mypy, for instance, will in certain situations report the full type expression if it's a `TypeAlias`, but if it's a `TypeAliasType` it just report its name. So I mostly use it to reduce the amount of error message spaghettification.

---

_Comment by @MatthewMckee4 on 2025-05-15 21:56_

Is this another example of a `TypeForm` that we need to handle _differently_, like in #122?

---

_Comment by @carljm on 2025-05-16 02:22_

> Is this another example of a `TypeForm` that we need to handle _differently_, like in [#122](https://github.com/astral-sh/ty/issues/122)?

Yes, the second argument when instantiating a `TypeAliasType` will need to be treated as a type expression, similar to e.g. the `bound` or `constraints` argument when instantiating a legacy `TypeVar`.

---

_Assigned to @sharkdp by @sharkdp on 2025-05-16 13:32_

---

_Closed by @sharkdp on 2025-05-19 14:36_

---

_Closed by @sharkdp on 2025-05-19 14:36_

---
