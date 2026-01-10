```yaml
number: 20541
title: "[ty] Fix incorrect inference of `enum.auto()` for enums with non-`int` mixins, and imprecise inference of `enum.auto()` for single-member enums"
type: pull_request
state: merged
author: thejchap
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: thejchap/enum-auto2
created_at: 2025-09-24T00:01:58Z
updated_at: 2025-11-10T17:53:09Z
url: https://github.com/astral-sh/ruff/pull/20541
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Fix incorrect inference of `enum.auto()` for enums with non-`int` mixins, and imprecise inference of `enum.auto()` for single-member enums

---

_Pull request opened by @thejchap on 2025-09-24 00:01_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1241

- support `.value` on single member enums
  - more context in this [pr](https://github.com/astral-sh/ruff/pull/19382) - for single-member enums, the type is an instance of the enum class. for multi-member enums, its a union of literals. handle the former
- lightweight handling of arbitrary mixins (return `Any`)

## Test Plan

new mdtests

---

_Comment by @github-actions[bot] on 2025-09-24 00:04_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @github-actions[bot] on 2025-09-24 00:04_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2025-09-24 07:44_

---

_Marked ready for review by @thejchap on 2025-09-26 02:59_

---

_Review requested from @carljm by @thejchap on 2025-09-26 02:59_

---

_Review requested from @AlexWaygood by @thejchap on 2025-09-26 02:59_

---

_Review requested from @sharkdp by @thejchap on 2025-09-26 02:59_

---

_Review requested from @dcreager by @thejchap on 2025-09-26 02:59_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/enums.rs`:168 on 2025-09-26 10:29_

hmm, I'm not sure we should be adding more special cases here. An enum class that uses `bytes` as a mixin isn't conceptually any different from an enum that uses `tuple` or `list` or `dict` as a mixin, or a custom dataclass or custom `NamedTuple` as a mixin. Why should we treat `bytes` and `float` specially here?

Rather than adding more special cases, I think we should have a generalised fallback logic that iterates through the MRO of the enum class to check whether it has a custom mixin that isn't `str` or `int`. (Those two make sense to special-case, I think, because of the presence of `StrEnum` and `IntEnum` in the stdlib. You can determine whether an element in the MRO is a "custom mixin" class by checking if it isn't `object` and isn't a subclass of `enum.Enum` -- if both those conditions hold, I think we can treat it as a mixin class.) If the enum class has a custom mixin and `auto()` is used to define the value of an enum member, we should probably infer the `.value` of that enum member as just being `Any`, which would still be more accurate than what we have on `main` (even if it isn't very precise!). It seems pretty hard to predict whether the `.value` property is going to give you an instance of the mixin or an integer:

```pycon
>>> class Foo: ...
>>> import enum
>>> class FooEnum(Foo, enum.Enum):
...     X = enum.auto()
...     
>>> FooEnum.X.value
1
>>> class BEnum(bytes, enum.Enum):
...     X = enum.auto()
...     
>>> BEnum.X.value
b'\x00'
>>> class TEnum(tuple, enum.Enum):
...     X = enum.auto()
...     
>>> TEnum.X.value
(1,)
```

---

_@AlexWaygood requested changes on 2025-09-26 10:29_

Thank you!

---

_@thejchap reviewed on 2025-09-26 16:31_

---

_Review comment by @thejchap on `crates/ty_python_semantic/src/types/enums.rs`:168 on 2025-09-26 16:31_

this makes a lot more sense - thanks - didn't feel great about the explicit special cases after going down that road...

---

_Converted to draft by @thejchap on 2025-09-26 16:31_

---

_Marked ready for review by @thejchap on 2025-11-08 23:34_

---

_Review requested from @AlexWaygood by @thejchap on 2025-11-08 23:34_

---

_Comment by @thejchap on 2025-11-08 23:34_

@AlexWaygood i think this is ready for another look!

---

_@AlexWaygood approved on 2025-11-09 23:54_

Thank you!!

---

_Label `bug` added by @AlexWaygood on 2025-11-09 23:55_

---

_Renamed from "[ty] handle more `enum.auto()` cases" to "[ty] Fix incorrect inference of `enum.auto()` for enums with non-`int` mixins, and imprecise inference of `enum.auto()` for single-member enums" by @AlexWaygood on 2025-11-09 23:56_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-09 23:57_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-10 00:06_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-10 00:06_

---

_Comment by @AlexWaygood on 2025-11-10 00:17_

@woodruffw, any idea why astral-bot isn't posting or editing any ecosystem-report comments on this PR? :-)

I'd love to double-check the ecosystem impact here before merging

---

_Comment by @woodruffw on 2025-11-10 03:22_

(Just synchronizing chat streams)

Yeah, I have a theory on what's causing this -- I've opened a tracking issue on the bot! 

---

_Comment by @woodruffw on 2025-11-10 16:39_

I've made a tentative fix for this, which is now in place -- I'm going to fast-forward this branch to confirm that it works.

Edit: appears to be working!

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-10 17:51_

---

_Merged by @AlexWaygood on 2025-11-10 17:53_

---

_Closed by @AlexWaygood on 2025-11-10 17:53_

---
