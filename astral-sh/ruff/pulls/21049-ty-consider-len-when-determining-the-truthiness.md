```yaml
number: 21049
title: "[ty] Consider `__len__` when determining the truthiness of an instance of a tuple class or a `@final` class"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/final-always-truthy
created_at: 2025-10-23T16:59:58Z
updated_at: 2025-10-24T16:28:44Z
url: https://github.com/astral-sh/ruff/pull/21049
synced_at: 2026-01-12T15:57:15Z
```

# [ty] Consider `__len__` when determining the truthiness of an instance of a tuple class or a `@final` class

---

_@AlexWaygood_

## Summary

At runtime, if a class does not define `__bool__` but does define `__len__`, the result of the `__len__` call will be used to determine the truthiness of that object. The most common examples of this are various builtin containers such as tuple, list and others:

```pycon
>>> bool(())
False
>>> bool((1,))
True
>>> list.__bool__
Traceback (most recent call last):
  File "<python-input-6>", line 1, in <module>
    list.__bool__
AttributeError: type object 'list' has no attribute '__bool__'. Did you mean: '__doc__'?
>>> bool([])
False
>>> bool([1])
True
```

Up till now, we haven't attempted to represent this in ty: if a class `N` defines a custom `__len__` method, we've ignored it for the purposes of determining the truthiness of that class's instances. This is because `__bool__` always takes priority over `__len__`, and it's always possible that a subclass of `N` could override `__bool__` (thus invalidating, for some subtypes of `N`, the assumptions regarding truthiness that we made from looking at the return type of `N.__len__`).

However, this approach has several problems:
- It ignores the fact that if a `@final` class defines `__len__` but does not define `__bool__`, it's perfectly safe to look at the return type of `__len__`
- It ignores the fact that if a `@final` class defines neither `__len__` nor `__bool__`, it's perfectly safe to consider the class as being always truthy
- It's led us to synthesize `__bool__` methods for tuple classes that actually don't exist at runtime

This PR fixes these problems. It still leaves several issues unresolved: arguably, if we see that the runtime implicitly calls a badly defined `__len__` method, we should emit diagnostics warning the user about this. But these false negatives already exist on `main`, and it would take quite some time to write out all the code and tests for this kind of diagnostic. It doesn't seem to me to be a priority right now, so I chose to cut scope here.

Helps with https://github.com/astral-sh/ty/issues/1420. It doesn't close that issue (our behaviour will remain the same for the snippet the author posted in that issue!), but it'll make it easier for me to justify our current behaviour to the author of that issue. I have trouble justifying our current behaviour, since it just doesn't emulate the semantics at runtime fully enough IMO 😄

## Test plan

Added mdtests

---

_Label `ty` added by @AlexWaygood on 2025-10-23 16:59_

---

_Comment by @github-actions[bot] on 2025-10-23 17:02_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-23 17:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/_code/code.py:1087:68: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | (ExceptionInfo[BaseException] & ~AlwaysTruthy & ~AlwaysFalsy)`
- Found 483 diagnostics
+ Found 482 diagnostics

apprise (https://github.com/caronc/apprise)
- apprise/plugins/fcm/color.py:103:20: error[unsupported-operator] Operator `+` is unsupported between objects of type `Literal["#"]` and `(Unknown & ~bool & ~AlwaysFalsy) | (Match[str] & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | (Unknown & ~AlwaysTruthy & ~AlwaysFalsy)`
+ apprise/plugins/fcm/color.py:103:20: error[unsupported-operator] Operator `+` is unsupported between objects of type `Literal["#"]` and `(Unknown & ~bool & ~AlwaysFalsy) | Match[str] | (str & ~AlwaysFalsy) | (Unknown & ~AlwaysTruthy & ~AlwaysFalsy)`

pywin32 (https://github.com/mhammond/pywin32)
- com/win32comext/axscript/server/axsite.py:50:13: warning[possibly-missing-attribute] Attribute `Close` may be missing on object of type `(Unknown & ~AlwaysFalsy) | (PyIUnknown & ~AlwaysFalsy)`
+ com/win32comext/axscript/server/axsite.py:50:13: warning[possibly-missing-attribute] Attribute `Close` may be missing on object of type `(Unknown & ~AlwaysFalsy) | PyIUnknown`

```
</details>
No memory usage changes detected ✅


---

_Renamed from "[ty] Treat `@final` instances with no `__bool__` or `__len__` override as `AlwaysTruthy` subtypes" to "[ty] Consider `__len__` when determining the truthiness of an instance of a tuple class or a `@final` class" by @AlexWaygood on 2025-10-23 18:14_

---

_Marked ready for review by @AlexWaygood on 2025-10-23 18:51_

---

_Review requested from @carljm by @AlexWaygood on 2025-10-23 18:51_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-23 18:51_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-23 18:51_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_compendium/always_truthy_falsy.md`:20 on 2025-10-23 19:01_

Is this spurious?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:4515 on 2025-10-23 19:02_

nit: Since this calls out an intention, can you mark it with `TODO`?

---

_@dcreager approved on 2025-10-23 19:03_

bueno

---

_@MichaReiser reviewed on 2025-10-23 19:07_

Hmm, I worry this will be painful but we'll need to also implement the same logic for the not goto definition

---

_@AlexWaygood reviewed on 2025-10-23 19:07_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_compendium/always_truthy_falsy.md`:20 on 2025-10-23 19:07_

oops, yes

---

_Comment by @AlexWaygood on 2025-10-23 19:22_

> Hmm, I worry this will be painful but we'll need to also implement the same logic for the not goto definition

Great point. I think I fixed this -- want to take another look?

For goto definition, I don't think it's so important that we do the "only respect the dunder if it's `@final` dance. We don't really need to worry so much about soundness when the user is just asking us to jump to the definition that will be used at runtime 😄

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-10-23 19:22_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto_definition.rs`:1297 on 2025-10-24 06:05_

What's the reason for changing this test?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto_definition.rs`:1516 on 2025-10-24 06:08_

This seems somewhat unfortunate. I think I'd expect the IDE to jump to either `__bool__` or `__len__` (it should be forgiving). E.g, huh... why does ty complain about `not a` not being supported. Click... ahhh, `__bool__` has the wrong signature. 

We don't need to change this here but I think we should change the documentation to make it sound less so that this is our desired behavior.

---

_@MichaReiser approved on 2025-10-24 06:09_

---

_@AlexWaygood reviewed on 2025-10-24 06:43_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/goto_definition.rs`:1516 on 2025-10-24 06:43_

Oh, I was just trying to preserve our existing behaviour here. goto already doesn't work for the other operators if the dunders are defined slightly incorrectly. I'll fix this up, thanks 

---

_@AlexWaygood reviewed on 2025-10-24 06:47_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/goto_definition.rs`:1297 on 2025-10-24 06:47_

`not` now works slightly differently to all other unary operators when it comes to goto-definition, so I felt like we should probably test with a different unary operator for "generic unary operator functionality" tests like this, and then have some different tests on top of that for the specific ways in which `not` differs from the rest 

---

_Merged by @AlexWaygood on 2025-10-24 09:29_

---

_Closed by @AlexWaygood on 2025-10-24 09:29_

---

_Branch deleted on 2025-10-24 09:29_

---

_@carljm reviewed on 2025-10-24 16:27_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:4520 on 2025-10-24 16:27_

This is implicitly depending for its correctness on our plan (not yet implemented AFAIK?) to ban overriding `__len__` or `__bool__` on a tuple subclass?

---

_@AlexWaygood reviewed on 2025-10-24 16:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:4520 on 2025-10-24 16:28_

yes, as stated in the comment immediately above this line of code

---
