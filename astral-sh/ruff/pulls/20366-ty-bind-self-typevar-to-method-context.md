```yaml
number: 20366
title: "[ty] Bind Self typevar to method context"
type: pull_request
state: merged
author: Glyphack
labels:
  - ty
assignees: []
merged: true
base: main
head: shaygan/self-binding
created_at: 2025-09-12T16:31:31Z
updated_at: 2025-09-17T19:10:22Z
url: https://github.com/astral-sh/ruff/pull/20366
synced_at: 2026-01-12T15:57:00Z
```

# [ty] Bind Self typevar to method context

---

_@Glyphack_

Fixes: https://github.com/astral-sh/ty/issues/1173

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR will change the logic of binding Self type variables to bind self to the immediate function that it's used on.
Since we are binding `self` to methods and not the class itself we need to ensure that we bind self consistently.

The fix is to traverse scopes containing the self and find the first function inside a class and use that function to bind the typevar for self.

If no such scope is found we fallback to the normal behavior. Using Self outside of a class scope is not legal anyway.

## Test Plan

Added a new mdtest.

Checked the diagnostics that are not emitted anymore in [primer results](https://github.com/astral-sh/ruff/pull/20366#issuecomment-3289411424). It looks good altough I don't completely understand what was wrong before.


---

_Label `ty` added by @ntBre on 2025-09-12 16:50_

---

_Renamed from "Bind Self typevar to method context" to "[ty] Bind Self typevar to method context" by @Glyphack on 2025-09-12 21:25_

---

_Comment by @github-actions[bot] on 2025-09-14 10:00_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-17 08:07:59.886695268 +0000
+++ new-output.txt	2025-09-17 08:08:02.947708879 +0000
@@ -408,13 +408,13 @@
 generics_self_advanced.py:18:1: error[type-assertion-failure] Argument does not have asserted type `ParentA`
 generics_self_advanced.py:19:1: error[type-assertion-failure] Argument does not have asserted type `ChildA`
 generics_self_advanced.py:28:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@method1`
-generics_self_advanced.py:35:9: error[type-assertion-failure] Argument does not have asserted type `typing.Self`
-generics_self_advanced.py:36:9: error[type-assertion-failure] Argument does not have asserted type `list[typing.Self]`
-generics_self_advanced.py:37:9: error[type-assertion-failure] Argument does not have asserted type `typing.Self`
-generics_self_advanced.py:38:9: error[type-assertion-failure] Argument does not have asserted type `typing.Self`
-generics_self_advanced.py:43:9: error[type-assertion-failure] Argument does not have asserted type `list[typing.Self]`
-generics_self_advanced.py:44:9: error[type-assertion-failure] Argument does not have asserted type `typing.Self`
-generics_self_advanced.py:45:9: error[type-assertion-failure] Argument does not have asserted type `typing.Self`
+generics_self_advanced.py:35:9: error[type-assertion-failure] Argument does not have asserted type `Self@method2`
+generics_self_advanced.py:36:9: error[type-assertion-failure] Argument does not have asserted type `list[Self@method2]`
+generics_self_advanced.py:37:9: error[type-assertion-failure] Argument does not have asserted type `Self@method2`
+generics_self_advanced.py:38:9: error[type-assertion-failure] Argument does not have asserted type `Self@method2`
+generics_self_advanced.py:43:9: error[type-assertion-failure] Argument does not have asserted type `list[Self@method3]`
+generics_self_advanced.py:44:9: error[type-assertion-failure] Argument does not have asserted type `Self@method3`
+generics_self_advanced.py:45:9: error[type-assertion-failure] Argument does not have asserted type `Self@method3`
 generics_self_attributes.py:26:33: error[invalid-argument-type] Argument is incorrect: Expected `typing.Self | None`, found `LinkedList[int]`
 generics_self_attributes.py:29:5: error[invalid-assignment] Object of type `OrdinalLinkedList` is not assignable to attribute `next` of type `typing.Self | None`
 generics_self_attributes.py:32:5: error[invalid-assignment] Object of type `LinkedList[int]` is not assignable to attribute `next` of type `typing.Self | None`
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-14 10:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
discord.py (https://github.com/Rapptz/discord.py)
- discord/client.py:331:27: error[invalid-argument-type] Argument to class `ConnectionState` is incorrect: Expected `Client`, found `typing.Self`
- discord/ui/view.py:226:30: error[invalid-argument-type] Argument to class `Item` is incorrect: Expected `BaseView`, found `typing.Self`
- discord/ui/view.py:918:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 518 diagnostics
+ Found 515 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/coordinates.py:855:16: error[invalid-return-type] Return type does not match returned value: expected `Coordinates`, found `typing.Self`
- Found 1613 diagnostics
+ Found 1612 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/awair/config_flow.py:129:16: error[unresolved-attribute] Type `typing.Self` has no attribute `source`
- homeassistant/components/awair/config_flow.py:131:20: error[unresolved-attribute] Type `typing.Self` has no attribute `context`
- homeassistant/components/awair/config_flow.py:132:21: error[unresolved-attribute] Type `typing.Self` has no attribute `host`
- homeassistant/components/bluetooth/passive_update_processor.py:509:28: error[invalid-argument-type] Argument to class `PassiveBluetoothProcessorEntity` is incorrect: Expected `PassiveBluetoothDataProcessor[Any, Any]`, found `typing.Self`
- Found 13469 diagnostics
+ Found 13465 diagnostics

CPython (cases_generator) (https://github.com/python/cpython)
- Lib/test/test_typing.py:278:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@bar`
+ Lib/test/test_typing.py:278:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@test_basics`
- Lib/test/test_typing.py:280:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@bar`
+ Lib/test/test_typing.py:280:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@test_basics`
- Lib/test/test_typing.py:282:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@bar`
+ Lib/test/test_typing.py:282:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@test_basics`

CPython (peg_generator) (https://github.com/python/cpython)
- Lib/test/test_typing.py:278:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@bar`
+ Lib/test/test_typing.py:278:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@test_basics`
- Lib/test/test_typing.py:280:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@bar`
+ Lib/test/test_typing.py:280:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@test_basics`
- Lib/test/test_typing.py:282:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@bar`
+ Lib/test/test_typing.py:282:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@test_basics`

CPython (Argument Clinic) (https://github.com/python/cpython)
- Lib/test/test_typing.py:278:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@bar`
+ Lib/test/test_typing.py:278:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@test_basics`
- Lib/test/test_typing.py:280:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@bar`
+ Lib/test/test_typing.py:280:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@test_basics`
- Lib/test/test_typing.py:282:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@bar`
+ Lib/test/test_typing.py:282:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@test_basics`

```
</details>
No memory usage changes detected ✅


---

_Comment by @Glyphack on 2025-09-14 10:22_

@dcreager I hope I understood your suggestion correctly about
> Fixing this should hopefully just require adding a check at the beginning of that function to immediately bind any Self typevar to typevar_binding_context.

This means that the `self` is now bound to any typvar_binding_context that it's used. So this would be the result:

```py
class C:
    def f(self: "C"):
        def b(x: Self):
            reveal_type(x)  # revealed: Self@b
```

I feel we should bind this to the method (so reveal `Self@f`) otherwise we emit diagnostic in the following case:

```py
from typing import Self

class Shape:
    def nested_func(self: Self) -> Self:
        def inner() -> Self:
            return self  # [invalid-return-type] "Return type does not match returned value: expected `Self@inner`, found `Self@nested_func`"
        return inner()
```

Or is this a separate problem with assignability of two type vars that have similar definitions?

Pyright and Mypy don't report diagnostic here:
- https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDKApgDbABQ5AxiQIYDO9hAFrQkQFzlQ9QAmRYFBRF6MInwD6wAK4oqACnqlgHQioCUUALQA%2BdWS69j-QZhQiQCrXoOruJxyCIA3IrRKT47JZp4BiKGc3Dwk1YjIAARExCWk5KgdHY2cYGRAUKGUyJJNU9MzUS2tyIA
- https://mypy-play.net/?mypy=latest&python=3.12&gist=0b6c973d8370193ae52dc305e62ff5a1

---

_Comment by @dcreager on 2025-09-15 14:34_

> This means that the `self` is now bound to any typvar_binding_context that it's used. So this would be the result:

Ah, that's a good catch! `typevar_binding_context` will always be the innermost generic scope. I guess we want this to bind to the function scope immediately inside the innermost class scope. It should be possible to add another helper method like `enclosing_generic_context` that would walk the enclosing scopes in the same way, looking for that pattern. [`Itertools::tuple_windows`](https://docs.rs/itertools/latest/itertools/trait.Itertools.html#method.tuple_windows) will be helpful there, since it will make it easier to look at adjacent pairs of scopes to find the first occurence of `function, class`.

---

_Marked ready for review by @Glyphack on 2025-09-15 20:14_

---

_Review requested from @carljm by @Glyphack on 2025-09-15 20:14_

---

_Review requested from @AlexWaygood by @Glyphack on 2025-09-15 20:14_

---

_Review requested from @sharkdp by @Glyphack on 2025-09-15 20:14_

---

_Review requested from @dcreager by @Glyphack on 2025-09-15 20:14_

---

_Review request for @carljm removed by @carljm on 2025-09-15 20:37_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:240 on 2025-09-16 02:04_

nit

```suggestion
In nested functions `self` binds to the method. So in the following example the `self` in `C.b` is
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:252 on 2025-09-16 02:05_

```suggestion
Even if the `Self` annotation appears first in the nested function, it is the method that binds `Self`.
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:269 on 2025-09-16 02:15_

I think this conflates `self` and `Self` a bit. You're right that a parameter named `self` doesn't cause a function to become generic, but a `Self` annotation should. So I expect you _would_ get a generic context for `C.b`. And once implicit `Self` annotations land, many `self` parameters would start to yield a generic context, because of that implicit annotation. In this example, `C.f` would still not get a generic context even with implicit `Self`, because of the explicit `"C"` annotation.

So concretely, I'd suggest adding a test for `generic_context(C.B)` as well (to make sure I'm not wrong in my analysis!) and also to put some of this detail into the description.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:87 on 2025-09-16 02:18_

Can you add a comment explaining the need for this? Something like

> `typing.Self` is treated like a legacy typevar, but doesn't follow the same scoping rules. It is always bound to the outermost method in the containing class.

---

_@dcreager approved on 2025-09-16 02:18_

Logic looks good, just a couple of documentation suggestions!

---

_@Glyphack reviewed on 2025-09-16 14:15_

---

_Review comment by @Glyphack on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:269 on 2025-09-16 14:15_

I see your point. I wanted to add a case for `b` but since `b` is in `C.f` the expression `generic_context(C.f.b)` did not work. So I extract this part into it's own section with three examples:

1. Method with implicit self
2. Method with explicit `Self` annotation
3. Method annotated with class body

It's not final yet but I tried to annotate methods that are not generic with the class name in [here](https://github.com/astral-sh/ruff/pull/18007/commits/62a4954cf30cee682414ea30ce6cf3ab028d6421) ([discussion](https://discord.com/channels/1039017663004942429/1414685112167305216)) and it yielded some performance benefit and it's in my method call PR right now.
I will add test cases for 2 and 3 in this PR and leave the first one in my other PR.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:269 on 2025-09-16 14:23_

> I see your point. I wanted to add a case for `b` but since `b` is in `C.f` the expression `generic_context(C.f.b)` did not work.

Oh of course, you're right! `C.f.b` isn't a valid way to reference `b`. You could do `reveal_type(generic_context(b))` inside the definition of `f` though.

---

_@dcreager reviewed on 2025-09-16 14:23_

---

_@Glyphack reviewed on 2025-09-16 14:26_

---

_Review comment by @Glyphack on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:269 on 2025-09-16 14:26_

You're right I somehow didn't think of adding the line in the function 😄

---

_@Glyphack reviewed on 2025-09-16 15:17_

---

_Review comment by @Glyphack on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:269 on 2025-09-16 15:17_

Okay I changed it a bit to only assert the generic context of the inner function here.

But the result was not what I expected in the case above both `b` and `C.f` have no generic context.
https://github.com/astral-sh/ruff/pull/20366/commits/89b4594b7873194becea0e4a3d882788b00fcfe9

---

_@Glyphack reviewed on 2025-09-16 15:20_

---

_Review comment by @Glyphack on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:269 on 2025-09-16 15:20_

In this case we want the `Self` to bind to the method so it binds to `C.f` and not `b` as a result now `b` does not have it's generic context anymore.

I am thinking if I make a change so we place `Self` in generic context of `b` is that desirable? `Self` is bound to `f` not `b`.
On the other hand I am not sure current behavior is fine because no member gets the generic context now.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:251 on 2025-09-17 01:42_

This looks right. Can we add a check that `generic_context(C.f)` contains `Self`?

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:267 on 2025-09-17 01:47_

Hmm, this is an interesting example. I think the current results are correct, and I don't think anything needs to change in this PR, but it affects the larger implicit `Self` work:

Here, `Self` is bound to `f`, but there aren't any annotations in `f` that actually reference `Self`. So our current call binding machinery wouldn't know how to infer a specialization of `Self` when `f` is called.

I wonder if to handle this right, the implicit `Self` logic should turn this into

```py
class C:
    def f(self: "Self & C"): ...
```

i.e. if there's an annotation, add the `Self` typevar in an intersection with the annotation. (Intersection simplification would do the right thing if there's already an annotation of `Self`.) That would give us a typevar in the formal parameter list to match against the `self` argument that's passed in at call time.

@carljm @AlexWaygood @sharkdp Do any of you have thoughts on this?

---

_@dcreager reviewed on 2025-09-17 01:49_

---

_@carljm reviewed on 2025-09-17 02:06_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:267 on 2025-09-17 02:06_

In this particular example, given no `Self` annotations in the signature of `f`, it doesn't matter if we can't infer a specialization of `Self`; there's nothing that specialization could possibly apply to. (References to `Self` inside the body of `f` won't ever be specialized anyway.)

I think your point would be relevant in a case like this:

```py
class C:
    def f(self: "C") -> Self:
        return self
```

But I also think we don't need to care about this case. For one thing, because there is no possible valid implementation of such a function (the implementation I give here should error on the return because `C` is not assignable to `Self`). And for another thing, because both mypy and pyright agree that the signature of `f` itself is invalid: they both give errors saying you can't use `Self` in a method with an explicitly annotated `self` or `cls` argument.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:267 on 2025-09-17 15:05_

> In this particular example, given no `Self` annotations in the signature of `f`, it doesn't matter if we can't infer a specialization of `Self`; there's nothing that specialization could possibly apply to.

Ah yes, that's a good point. `Self` in `b` is a non-inferable typevar already, and so we're not worried about particular specializations of it; we'll verify assignability against it for all possible specializations.

> References to `Self` inside the body of `f` won't ever be specialized anyway.

Would that change with return type inference?

> But I also think we don't need to care about this case.
>
> they both give errors saying you can't use `Self` in a method with an explicitly annotated `self` or `cls` argument.

Nice, that's even mentioned in [the spec](https://typing.python.org/en/latest/spec/generics.html#valid-locations-for-self) as an invalid use of `Self` (the `has_existing_self_annotation` example).




---

_@dcreager approved on 2025-09-17 15:05_

---

_@carljm reviewed on 2025-09-17 15:10_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:267 on 2025-09-17 15:10_

> Would that change with return type inference?

Hmm, yeah, I suppose it would. So something like this:

```py
class C:
    def f(self):
        def inner(x: Self) -> Self:
            return x
        return inner

C().f()  # should return generic `Callable[[Self], Self]` where `Self` is an inferable type var with upper bound `C`
```

---

_@dcreager reviewed on 2025-09-17 16:41_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:267 on 2025-09-17 16:41_

Yep, I think that's a good example. I don't think it needs to block this PR, but it will be good to keep in mind as we work on implicit `Self` and RTI

---

_@carljm reviewed on 2025-09-17 17:54_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:267 on 2025-09-17 17:54_

Interestingly, in this case both pyrefly and pyright specialize `Self` eagerly and return `(C) -> C`, not `(Self) -> Self` (that is, the returned callable is not generic.) (Mypy doesn't do RTI.)

---

_Comment by @dcreager on 2025-09-17 18:58_

Thanks for this, @Glyphack! The discussion above doesn't block this PR, so I'm going to go ahead and merge it.

---

_Merged by @dcreager on 2025-09-17 18:58_

---

_Closed by @dcreager on 2025-09-17 18:58_

---

_Branch deleted on 2025-09-17 19:10_

---
