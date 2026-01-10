```yaml
number: 18242
title: "[ty] Synthetic function-like callables"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/function-like-callables
created_at: 2025-05-21T13:55:07Z
updated_at: 2025-05-28T09:59:05Z
url: https://github.com/astral-sh/ruff/pull/18242
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Synthetic function-like callables

---

_Pull request opened by @sharkdp on 2025-05-21 13:55_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

We create `Callable` types for synthesized functions like the `__init__` method of a dataclass. These generated functions are real functions though, with descriptor-like behavior. That is, they can bind `self` when accessed on an instance. This was modeled incorrectly so far.

## Test Plan

Updated tests


---

_Label `ty` added by @sharkdp on 2025-05-21 13:55_

---

_Comment by @github-actions[bot] on 2025-05-21 13:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- info[revealed-type] tests/dataclass_transform_example.py:13:13: Revealed type: `(a: str, b: int) -> None`
+ info[revealed-type] tests/dataclass_transform_example.py:13:13: Revealed type: `(self: Define, a: str, b: int) -> None`
- info[revealed-type] tests/dataclass_transform_example.py:21:13: Revealed type: `(with_converter: int = Unknown) -> None`
+ info[revealed-type] tests/dataclass_transform_example.py:21:13: Revealed type: `(self: DefineConverter, with_converter: int = Unknown) -> None`
- warning[possibly-unbound-attribute] tests/test_dunders.py:1008:13: Attribute `__code__` on type `Unknown | (() -> None)` is possibly unbound
- warning[possibly-unbound-attribute] tests/test_dunders.py:1020:13: Attribute `__code__` on type `Unknown | (() -> None)` is possibly unbound
- error[unresolved-attribute] tests/test_dunders.py:1032:13: Type `() -> None` has no attribute `__code__`
- Found 621 diagnostics
+ Found 618 diagnostics

```
</details>


---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:7660 on 2025-05-21 14:03_

Initially, I didn't want to add a new parameter here, and simply passed `false` below everywhere. This led to a very subtle bug that took me a long time to catch, because we use `from_overloads` to essentially create a new copy of a callable, and the is-function-like behavior was lost in that process. I therefore made it explicit here, but I'm also fine with trying to walk this back (and create a new function for copying).

---

_@sharkdp reviewed on 2025-05-21 14:03_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:2751 on 2025-05-21 14:09_

It is a shortcut to model this in `try_call_dunder_get`. If we want to be really precise, we should instead create a new method-wrapper type variant for the synthesized `__get__` method of these synthesized functions, which would be returned from `find_name_in_mro` when called on function-like `Type::Callable`s. This would allow us to correctly model the behavior of explicit `SomeDataclass.__init__.__get__` calls. The occurrence of this seems rather unlikely, so I did not introduce a new type variant just for this. But if we would rather model this completely correctly, I'm happy to make the necessary changes.

---

_@sharkdp reviewed on 2025-05-21 14:09_

---

_@sharkdp reviewed on 2025-05-21 14:12_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:6944 on 2025-05-21 14:12_

It seemed somewhat tempting to return `true` here :smile: (to make `Callable` types that are created from functions "function-like"). And maybe that would be more correct in some sense? At the moment, I think it does not make any difference, since `FunctionType::into_callable_type` is only used in type-property functions that do not seem to invoke the descriptor protocol.

---

_Marked ready for review by @sharkdp on 2025-05-21 14:14_

---

_Review requested from @carljm by @sharkdp on 2025-05-21 14:14_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-21 14:14_

---

_Review requested from @dcreager by @sharkdp on 2025-05-21 14:14_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:2751 on 2025-05-21 14:16_

Could you add that brief summary as a comment, so we know what to do if it does come up in the future?

---

_Comment by @AlexWaygood on 2025-05-21 14:17_

> ```diff
> - warning[possibly-unbound-attribute] tests/test_dunders.py:1008:13: Attribute `__code__` on type `Unknown | (() -> None)` is possibly unbound
> + warning[possibly-unbound-attribute] tests/test_dunders.py:1008:13: Attribute `__code__` on type `Unknown | ((self: C) -> None)` is possibly unbound
> ```

hmm, it seems incorrect that we're emitting this diagnostic. It definitely exists at runtime:

```pycon
>>> def f(): ...
... 
>>> f.__code__
<code object f at 0x105d60b90, file "<python-input-0>", line 1>
>>> class Foo:
...     def f(): ...
...     
>>> Foo().f.__code__
<code object f at 0x105d60c60, file "<python-input-2>", line 2>
```

and it's annotated as existing on `FunctionType` instances in typeshed: https://github.com/astral-sh/ruff/blob/cb9e66927e4658247c1952a67a55a3c3b39523b4/crates/ty_vendored/vendor/typeshed/stdlib/types.pyi#L72-L76

I suppose we need to fallback to `Instance("FunctionType")` for member access on `CallableType` inhabitants if `is_function_like` is set to `true`?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:7582 on 2025-05-21 14:18_

Can you update the comment here to indicate that it creates a function-like callable? (i.e. one that bind self when participating in the descriptor protocol)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:6944 on 2025-05-21 14:19_

If it doesn't change any of the current visible behavior I think I would prefer `true` here, if we're using that field to indicate whether the callable binds self when participating in the descriptor protocol.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:7660 on 2025-05-21 14:20_

I think the parameter is fine, at least for now — it has the benefit of making us think about whether a callable is function-like each time we create one

---

_@dcreager reviewed on 2025-05-21 14:21_

---

_Comment by @AlexWaygood on 2025-05-21 14:23_

Are `CallableType` inhabitants with `is_function_like: true` modeled as (consistent) subtypes of `Instance("types.FunctionType")`? I think they should be -- could you add a test?

---

_Comment by @AlexWaygood on 2025-05-21 14:27_

I guess I'm not _totally_ sold that these should be modeled as a special case of `CallableType` rather than being modeled as a special case of `FunctionLiteral` (or being their own `Type` variant entirely) -- the subtyping/assignability and member-access questions seem to me like function-like `CallableType`s behave pretty differently to non-function-like `CallableType`s

---

_Comment by @carljm on 2025-05-21 14:34_

I think it would be important to see some new subtyping and assignability tests (and the code to make them pass, which I also don't think I'm seeing here?), which should verify several things:

* A fully-static callable type that is function-like is a subtype of the same callable type that is not function-like, but not the other way around. (Unless I'm missing something, it seems like this PR currently doesn't consider `is_function_like` in callable-subtyping at all, which I think would mean it fails to recognize the "not" part of this?)
* Same for assignability.
* A function literal is a subtype of its own `into_callable_type` (this could almost be a property test?)

---

_@carljm reviewed on 2025-05-21 14:35_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7638 on 2025-05-21 14:35_

I think we could be a bit more precise in our description here, i.e. a callable type with `is_function_like: True` is inhabited by callable objects with the given signature that also have a `__get__` method which acts like `FunctionType.__get__` (returns a bound method).

---

_Comment by @sharkdp on 2025-05-21 15:26_

> ```diff
> ```diff
> + warning[possibly-unbound-attribute] tests/test_dunders.py:1008:13: Attribute `__code__` on type `Unknown | ((self: C) -> None)` is possibly unbound
> ```
>  
> hmm, it seems incorrect that we're emitting this diagnostic. It definitely exists at runtime:

Yes, thanks! At runtime, it also exists for generated methods on dataclasses. This should be fixed now.

---

_Comment by @sharkdp on 2025-05-21 15:31_

> Are `CallableType` inhabitants with `is_function_like: true` modeled as (consistent) subtypes of `Instance("types.FunctionType")`? I think they should be -- could you add a test?

Yes, thanks. Added the test, but haven't fixed it yet.

> I guess I'm not _totally_ sold that these should be modeled as a special case of `CallableType` rather than being modeled as a special case of `FunctionLiteral` (or being their own `Type` variant entirely) -- the subtyping/assignability and member-access questions seem to me like function-like `CallableType`s behave pretty differently to non-function-like `CallableType`s

I originally planned it this way, but somehow accidentally did the "reverse" today and special-cased `Type::Callable`, and wondered why it was so much easier than I had anticipated :smile:. I will re-evaluate this after quickly trying to implement subtyping with the existing approach.

---

_Comment by @sharkdp on 2025-05-21 15:35_

> I think it would be important to see some new subtyping and assignability tests (and the code to make them pass, which I also don't think I'm seeing here?), which should verify several things:
> 
> * A fully-static callable type that is function-like is a subtype of the same callable type that is not function-like, but not the other way around. (Unless I'm missing something, it seems like this PR currently doesn't consider `is_function_like` in callable-subtyping at all, which I think would mean it fails to recognize the "not" part of this?)
> 
> * Same for assignability.

So this is interesting. I added the tests, and they.. just pass. It works not because we're looking at the `is_function_like` attribute, but rather because the corresponding synthesized callable has positional-or-keyword parameters with names, whereas the equivalent callable type has positional-only parameters without any name.
```py
type DunderInitType = TypeOf[C.__init__]  # (self: C, x: int) -> None
type EquivalentCallableType = Callable[[C, int], None]  # (C, int, /) -> None
```
This is probably not a *great* way to ensure proper subtyping/assignability properties, so I can look into incorporating `is_function_like` somehow.

---

_Comment by @carljm on 2025-05-21 15:46_

> I can look into incorporating `is_function_like` somehow

I think the implementation shouldn't be too difficult? Just a check in `is_assignable_to_impl` that the two `is_function_like`, if they differ, differ only in the right direction.

Writing a failing test may be more of the challenge, but I think if you also define `EquivalentCallableType` by writing an actual function and then using `CallableTypeOf` on it, that might work?

---

_Comment by @AlexWaygood on 2025-05-21 16:10_

> somehow accidentally did the "reverse" today and special-cased `Type::Callable`, and wondered why it was so much easier than I had anticipated

it being "easier" doesn't necessarily reassure me ;) that might just mean that we've accidentally omitted some branches in some places that we would otherwise be forced to add if it was its own variant 😆

---

_Converted to draft by @sharkdp on 2025-05-21 18:51_

---

_Comment by @carljm on 2025-05-22 05:43_

If we do have this as a form of Callable type, it might mean that we could be "smart" about `method_attr: Callable[[Self], int] = function_object` vs `non_method_attr: Callable[[], int] = other_non_function_like_callable_object`, where we actually use the type of the RHS as a hint to decide whether we consider the declaration to be of a "method descriptor" or "non method descriptor" callable type. That could let us avoid some unsoundness? See https://github.com/astral-sh/ruff/pull/18167#issuecomment-2899969500

On the other hand, it's weird to interpret the same annotation differently depending on the RHS. So maybe we shouldn't do that.

---

_Marked ready for review by @sharkdp on 2025-05-23 12:28_

---

_Comment by @sharkdp on 2025-05-23 12:34_

Opening this up for review again. It passes all tests and resolves some ecosystem false positives. I am happy to spend (significantly?) more time on this, trying to rewrite it in terms of a customized `FunctionType` variant. I agree with @AlexWaygood that this might be less error-prone. But given that we currently only use this for one edge case (*explicit* calls to a synthetic dunder method on a dataclass, as in `MyDataClass.__lt__(…)`), I'm not sure it's worth it? The answer might depend on whether or not we foresee using this for purposes of resolving https://github.com/astral-sh/ty/issues/491.

---

_@carljm approved on 2025-05-27 19:31_

I think it's fine to go with this for now.

---

_Merged by @sharkdp on 2025-05-28 08:00_

---

_Closed by @sharkdp on 2025-05-28 08:00_

---

_Branch deleted on 2025-05-28 08:00_

---

_Comment by @AlexWaygood on 2025-05-28 09:59_

> given that we currently only use this for one edge case (_explicit_ calls to a synthetic dunder method on a dataclass, as in `MyDataClass.__lt__(…)`), I'm not sure it's worth it?

But I think we should use function-like callables for lambdas too, since they're also instances of `FunctionType` at runtime (they therefore have all those attributes that you can find on instances of `types.FunctionType`, and can be used as methods just fine if you assign them to symbols in class namespaces()

---
