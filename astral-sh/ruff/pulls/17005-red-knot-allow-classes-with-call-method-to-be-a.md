```yaml
number: 17005
title: "[red-knot] Allow classes with __call__ method to be a subtype of Callable"
type: pull_request
state: closed
author: MatthewMckee4
labels:
  - ty
assignees: []
base: main
head: class-with-call-subtype-callable
created_at: 2025-03-26T23:39:09Z
updated_at: 2025-04-01T23:49:30Z
url: https://github.com/astral-sh/ruff/pull/17005
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Allow classes with __call__ method to be a subtype of Callable

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Partially fixes #16953 

I am aware this maybe doesn't align with some patterns, so i am very open to criticism. 

## Test Plan

Updated is_subtype_of.md


---

_Review requested from @carljm by @MatthewMckee4 on 2025-03-26 23:39_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-03-26 23:39_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-03-26 23:39_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-03-26 23:39_

---

_Comment by @github-actions[bot] on 2025-03-26 23:45_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
rich (https://github.com/Textualize/rich)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/tests/test_progress.py:292:60: Object of type `MockClock` cannot be assigned to parameter `get_time` of function `track`; expected type `(() -> int | float) | None`
- Found 582 diagnostics
+ Found 581 diagnostics

```
</details>


---

_Label `red-knot` added by @MichaReiser on 2025-03-27 12:09_

---

_Comment by @MatthewMckee4 on 2025-03-27 14:09_

Perhaps implementing a method like
https://github.com/python/mypy/blob/b1be379f88e1e3735e04931f8253bbde4b387602/mypy/typeops.py#L366

would be more useful in the long run?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1048 on 2025-03-27 21:34_

Seems like these two tests don't belong in `is_subtype_of.md`, since they are tests of assignability, not subtyping.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1045 on 2025-03-27 21:37_

These tests should either test just `A` (like below) or `TypeOf[A()]` (they are equivalent, so I'd go with the former for simplicity), but `TypeOf[A]` is not correct. The class object `A` is not a callable with signature `(int) -> int`. As defined above, it is a callable with signature `() -> A`.

We should (in a separate PR) implement support for considering class-literal types as callables, but that support should not be based on `__call__`, it should be based on the signatures of `__new__` and `__init__`.

We could (either in a separate PR, or this one), implement support for class objects whose metaclass has a `__call__` method. This takes priority over `__new__` and `__init__`. See this example:

```
>>> class M(type):
...     def __call__(self):
...         return "call"
...
>>> class C(metaclass=M):
...     def __init__(self):
...         print("init")
...
>>> C()
'call'
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:719 on 2025-03-27 21:42_

As mentioned above, we should not be handling `ClassLiteral` types by looking up a `__call__` method on the class itself. If we keep this branch in this PR, it should rather look up `__call__` on the metaclass of `class`. (And that should probably be handled by just having this branch delegate to checking if an instance of the metaclass is a subtype of the callable type.) In a future PR this branch would check `__init__`/`__call__` signature.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:724 on 2025-03-27 21:44_

We don't care about qualifiers here, so how about
```suggestion
                let call_symbol = class.class_member(db, "__call__").symbol;
                if call_symbol.is_unbound() {
                    false
                } else {
                    match call_symbol {
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:497 on 2025-03-27 21:47_

The name "self" is purely a convention in Python, there's nothing magical about it.

```suggestion
    /// Return a new Parameters type with the first parameter.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:500 on 2025-03-27 21:51_

We should not check the name of the parameter, it doesn't matter.

I think this method should return the removed parameter, because in the caller we should check if its annotated type (if any) is assignable from the instance type. That does matter. (We should add a test for the case where it isn't, e.g.

```py
class C:
    def __call__(self: int) -> int:
        return 1
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:935 on 2025-03-28 02:01_

As discussed above, this delegation isn't correct. A class object does not have the same call signature as an instance of that class. This branch should handle the checking of `__call__` method.

---

_@carljm reviewed on 2025-03-28 02:02_

Thanks!

---

_@MatthewMckee4 reviewed on 2025-03-28 10:02_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types.rs`:719 on 2025-03-28 10:02_

I can defer checking metaclass for `__call__` / checking class literal for `__new__` and `__init__` to another PR

---

_@MatthewMckee4 reviewed on 2025-03-28 10:10_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/signatures.rs`:500 on 2025-03-28 10:10_

How should that example be handled? It seems like this should be rejected before getting to this stage but i could be wrong.

pylance says "Type of parameter "self" must be a supertype of its class "A""
I guess since type of self is not yet dealt with, we cant do this.

I may need some more guidance on this anyway, what do you think of https://github.com/python/mypy/blob/b1be379f88e1e3735e04931f8253bbde4b387602/mypy/typeops.py#L366 and implementing a bind method (instead of into_callable_type_without_self_parameter which i think currently is not a very good function) to FunctionType that can take in the `Class` instance and return `Self` or `Type`

---

_@MatthewMckee4 reviewed on 2025-03-28 10:11_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1045 on 2025-03-28 10:11_

Can do i a separate PR

---

_@MatthewMckee4 reviewed on 2025-03-28 10:26_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:16 on 2025-03-28 10:26_

This was fixed by pre-commit, not sure why it wasnt like this before

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1068 on 2025-03-28 14:26_

On second look, I think it would also be worth adding a test that shows we are actually checking the signature of `__call__`:
```suggestion
static_assert(is_subtype_of(A, Callable[[int], int]))
static_assert(not is_subtype_of(A, Callable[[], int]))
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:713 on 2025-03-28 14:27_

```suggestion
            // An instance type with `__call__` method can be a subtype of Callable
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:500 on 2025-03-28 14:47_

Good call, thanks for pushing on whether this should be done in a better way. After looking at it, I think it should be done differently. We shouldn't need this function. Instead of using `class_member` to look up `__call__`, we should just use `.member()` on the instance type itself, and then call `.signature()` on the resulting type. In the typical case the `.member()` call will give us back a `Type::Callable(BoundMethod(_))`, and `Type::signature` already does the right thing for a bound method, including issuing a diagnostic in the case where `self` has a wrong type annotation.

I do think we'll probably want to also issue a diagnostic at the definition site of a method with a bad `self` annotation, but that's really separate and should be handled separately. Notice that [pyright issues diagnostics in both places](https://pyright-play.net/?strict=true&enableExperimentalFeatures=true&code=MYGwhgzhAEDCBcAoaLoBMCmAzaB9XwYII%2BAFBBiFvNAJYB2ALgJTQC0AfHU0qn9ACcMjAK4D60AIyJEWAQHsAttEYBPAA4MA5nUXr5AxnCLgARiAwzMOLKQAeNWCbDmMAbTcBdADTdGn5l5UdUgIGVtYUmZmGTtoAF44KKjEIA) (both the definition site of `__call__`, and the site where it is "used"). We are just worrying about the latter in this PR.

---

_@carljm reviewed on 2025-03-28 14:47_

---

_@carljm reviewed on 2025-03-28 15:16_

---

_Review comment by @carljm on `playground/shared/package-lock.json`:1 on 2025-03-28 15:16_

I don't think this PR should be changing this file?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:16 on 2025-03-28 15:19_

I think this change is because you (accidentally?) removed the link target below

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1056 on 2025-03-28 15:20_

This was removed and I don't think it should be

---

_@carljm reviewed on 2025-03-28 15:20_

---

_@MatthewMckee4 reviewed on 2025-03-28 15:21_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1056 on 2025-03-28 15:21_

Ah yeah i thought i had added that back, will do now

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/signatures.rs`:500 on 2025-03-28 15:28_

```rs
(Type::Instance(_), Type::Callable(_)) => {
                let call_symbol = self.member(db, "__call__").symbol;
                if call_symbol.is_unbound() {
                    false
                } else {
                    match call_symbol {
                        Symbol::Type(
                            Type::Callable(CallableType::BoundMethod(call_function)),
                            _,
                        ) => {
                            let callable_type = call_function.function(db).into_callable_type(db);
                            match callable_type {
                                Type::Callable(CallableType::General(_)) => {
                                    callable_type.is_subtype_of(db, target)
                                }
                                _ => false,
                            }
                        }
                        _ => false,
                    }
                }
            }
```

into_callable_type here still give a GeneralCallableType with the self parameter, it seems like maybe BoundMethodType should handle removing self from the parameters of its function?
            

---

_@MatthewMckee4 reviewed on 2025-03-28 15:28_

---

_@carljm reviewed on 2025-03-28 15:36_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:500 on 2025-03-28 15:36_

No we should not be matching on the specific type of the symbol result like that and pulling out internal details of the type. Instead we should just call `.signature()` on the type (no matter what it is) and construct a general callable type from that signature, and check if that's a subtype of `target`. `Type::signature` already takes care of correctly handling the `self` parameter. The resulting code should be quite a bit simpler and more general than what you pasted above. (And as a bonus it will handle all kinds of edge cases better where `__call__` is something other than a normal method, too).

---

_@MatthewMckee4 reviewed on 2025-03-28 15:40_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/signatures.rs`:500 on 2025-03-28 15:40_

`member()` returns SymbolAndQualifiers<'db>. I'm not sure i follow on what to call `.signature()` on

---

_@carljm reviewed on 2025-03-28 16:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:500 on 2025-03-28 16:01_

Unwrap the Symbol to a Type as you already do, but don't further destructure that type. Like this:

```rs
                let call_symbol = self.member(db, "__call__").symbol;
                match call_symbol {
                        Symbol::Type(call_method_type) => {
                            let call_method_signature = call_method_type.signature(db);
                            // wrap up the signature in a Type::Callable(General) and check if its a subtype of `target`.
                            // We could extract from `GeneralCallableType::is_assignable_to_impl`
                            // a function that directly checks subtypeness of two Signatures without having to
                            // wrap into a GeneralCallableType first, but that's probably not worth it.
                        }
                        _ => false,
                    }
                }
```

---

_@carljm reviewed on 2025-03-28 16:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:500 on 2025-03-28 16:01_

(We also don't need the separate if test for `is_unbound()`, that can just fall into the default case of the match, since it's not `Symbol::Type`.)

---

_@carljm reviewed on 2025-03-28 16:12_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:500 on 2025-03-28 16:12_

I just realized this should go one step further: `Type::signatures` already has handling for looking up `__call__` on a `Type::Instance`, so we don't even have to do that here, we can just call `.signatures(db)` directly on the Instance type.

There is some complexity about the fact that `GeneralCallableType` only holds a single `Signature`, but if you get a more complex `Signatures` back from the instance type, you can just punt on that with a TODO for now.

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/signatures.rs`:500 on 2025-03-28 16:25_

```rs
            // An instance type with `__call__` method can be a subtype of Callable
            (Type::Instance(_), Type::Callable(_)) => {
                let signatures = self.signatures(db);
                let mut iter = signatures.iter();
                match iter.next() {
                    Some(signature) => {
                        let mut overloads = signature.iter();
                        match overloads.next() {
                            Some(overload) => {
                                let callable_type = Type::Callable(CallableType::General(
                                    GeneralCallableType::new(db, overload),
                                ));
                                callable_type.is_subtype_of(db, target)
                            }
                            None => false,
                        }
                    }
                    None => false,
                }
            }
```
I may be on the wrong track here, but callable_type here still has a self parameter
            

---

_@MatthewMckee4 reviewed on 2025-03-28 16:25_

---

_@carljm reviewed on 2025-03-28 16:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:500 on 2025-03-28 16:45_

No, you're on the right track. It looks like the issue here is that the way we handle "binding self" for a bound method is not by removing the parameter, but by adding a `bound_type` to the `CallableSignature`, and then when we call it we know to bind that `bound_type` to the first parameter, and bind the provided arguments to the rest of the parameters. I think this is the right approach, as it lets us check type assignability for `self`.

The problem you're hitting here is that we set `bound_type` on the `CallableSignature` (which represents potentially a set of overloads), but `GeneralCallableType` only wraps a single `Signature`, which doesn't carry that `bound_type`.

I think really we need to resolve this issue in the context of broader support for overloads; we need to add support for callable subtyping with overloads, and with `bound_type`, and we don't have that yet. Probably ultimately `GeneralCallableType` should wrap a `CallableSignature`, not just a single `Signature`. So I guess proper resolution of this issue is a bigger task than we'd realized. It may be best to just hold off on it for now.

---

_@MatthewMckee4 reviewed on 2025-03-28 16:52_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/signatures.rs`:500 on 2025-03-28 16:52_

Okay sounds good. Thank you for the help anyway, i really appreciate it.

---

_Review request for @sharkdp removed by @sharkdp on 2025-03-31 08:27_

---

_Closed by @MatthewMckee4 on 2025-04-01 23:15_

---

_Branch deleted on 2025-04-01 23:49_

---
