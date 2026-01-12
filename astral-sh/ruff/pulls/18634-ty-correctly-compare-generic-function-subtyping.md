```yaml
number: 18634
title: "[ty] Correctly compare generic function subtyping/assignability regardless of TypeVar identity"
type: pull_request
state: closed
author: LaBatata101
labels:
  - ty
assignees: []
base: main
head: fix-typevar-identity
created_at: 2025-06-11T20:06:21Z
updated_at: 2025-08-15T16:30:04Z
url: https://github.com/astral-sh/ruff/pull/18634
synced_at: 2026-01-12T15:56:23Z
```

# [ty] Correctly compare generic function subtyping/assignability regardless of TypeVar identity

---

_@LaBatata101_



<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Attempt to fix [#95](https://github.com/astral-sh/ty/issues/95).

For this fix, I've tried to follow the suggestion made in [this comment](https://github.com/astral-sh/ty/issues/95#issuecomment-2923876231). I've added a new function `with_specialized_generic_context` -- the name here could probably be better -- that is used to create a new specialized `Signature` using the `SpecializationBuilder`, this function is called before the subtyping/assignability/equivalency comparisons. Then we use this new `Signature` to do the actual comparison.  Not sure if this was what @dcreager had in mind.

There's a bit of code duplication because I had to create a carbon copy of `is_assignable_to_impl` function to call the `SpecializationBuilder::infer` on the signature types.

This PR also creates a generic context for the `Callable` type. And finally, I didn't fix the issue in the implementation of `SpecializationBuilder::infer` that was mentioned on Discord.

Let me know if this is heading in the right direction or if you'd prefer a different approach. 
No hard feelings if this gets rejected.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Add more mdtest.
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-11 20:09_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
python-chess (https://github.com/niklasf/python-chess)
- chess/pgn.py:1857:35: error[invalid-argument-type] Argument to function `read_game` is incorrect: Expected `() -> BaseVisitor[Unknown]`, found `<class 'SkipVisitor'>`
- Found 16 diagnostics
+ Found 15 diagnostics

Expression (https://github.com/cognitedata/Expression)
- expression/collections/seq.py:637:41: error[invalid-argument-type] Argument to function `init_infinite` is incorrect: Expected `(int, /) -> Unknown`, found `def identity(value: _A) -> _A`
- expression/extra/result/traversable.py:50:21: error[invalid-argument-type] Argument to function `traverse` is incorrect: Expected `(Result[_TSource, _TError], /) -> Result[Unknown, Unknown]`, found `def identity(value: _A) -> _A`
- tests/test_map.py:49:19: error[invalid-argument-type] Argument to function `pipe` is incorrect: Expected `(Map[str, int], /) -> Unknown`, found `def to_seq(table: Map[_Key, _Value]) -> Iterable[tuple[_Key, _Value]]`
- tests/test_parser.py:232:9: error[invalid-argument-type] Argument to function `pipe` is incorrect: Expected `(Parser[str], /) -> Unknown`, found `def many(parser: Parser[_A]) -> Parser[Block[_A]]`
- tests/test_result.py:487:24: error[invalid-argument-type] Argument to bound method `pipe` is incorrect: Expected `(Result[Literal[42], Any], /) -> Unknown`, found `def swap(result: Result[_TSource, _TError]) -> Result[_TError, _TSource]`
- tests/test_result.py:573:24: error[invalid-argument-type] Argument to bound method `pipe` is incorrect: Expected `(Result[Literal[42], Any], /) -> Unknown`, found `def merge(result: Result[_TSource, _TSource]) -> _TSource`
- tests/test_seq.py:308:20: error[invalid-argument-type] Argument to bound method `choose` is incorrect: Expected `(None | Literal[42], /) -> Option[Unknown]`, found `def of_optional(value: _TSource | None) -> Option[_TSource]`
- Found 227 diagnostics
+ Found 220 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/datastructures/mixins.py:238:28: error[invalid-argument-type] Argument is incorrect: Expected `Self`, found `UpdateDictMixin[Any, Any]`
- src/werkzeug/local.py:526:24: error[invalid-return-type] Return type does not match returned value: expected `T`, found `Unknown | LocalProxy[Any] | T`
+ src/werkzeug/local.py:526:24: error[invalid-return-type] Return type does not match returned value: expected `T`, found `Unknown | LocalProxy[Any]`
- Found 370 diagnostics
+ Found 369 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/functional_serializers.py:298:12: error[invalid-return-type] Return type does not match returned value: expected `((_FieldWrapSerializerT, /) -> _FieldWrapSerializerT) | ((_FieldPlainSerializerT, /) -> _FieldPlainSerializerT)`, found `def dec(f: @Todo(Support for `typing.TypeAlias`)) -> PydanticDescriptorProxy[Any]`
+ pydantic/functional_serializers.py:298:12: error[invalid-return-type] Return type does not match returned value: expected `(_FieldWrapSerializerT, /) -> _FieldWrapSerializerT`, found `def dec(f: @Todo(Support for `typing.TypeAlias`)) -> PydanticDescriptorProxy[Any]`
- pydantic/functional_serializers.py:416:16: error[invalid-return-type] Return type does not match returned value: expected `_ModelPlainSerializerT | ((_ModelWrapSerializerT, /) -> _ModelWrapSerializerT) | ((_ModelPlainSerializerT, /) -> _ModelPlainSerializerT)`, found `def dec(f: @Todo(Support for `typing.TypeAlias`)) -> PydanticDescriptorProxy[Any]`
+ pydantic/functional_serializers.py:416:16: error[invalid-return-type] Return type does not match returned value: expected `_ModelPlainSerializerT | ((_ModelWrapSerializerT, /) -> _ModelWrapSerializerT)`, found `def dec(f: @Todo(Support for `typing.TypeAlias`)) -> PydanticDescriptorProxy[Any]`
- pydantic/functional_serializers.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `_ModelPlainSerializerT | ((_ModelWrapSerializerT, /) -> _ModelWrapSerializerT) | ((_ModelPlainSerializerT, /) -> _ModelPlainSerializerT)`, found `PydanticDescriptorProxy[Any]`
+ pydantic/functional_serializers.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `_ModelPlainSerializerT | ((_ModelWrapSerializerT, /) -> _ModelWrapSerializerT)`, found `PydanticDescriptorProxy[Any]`

discord.py (https://github.com/Rapptz/discord.py)
- discord/ext/commands/core.py:2103:12: error[invalid-return-type] Return type does not match returned value: expected `(T, /) -> T`, found `Check[Unknown]`
- discord/ext/commands/core.py:2136:12: error[invalid-return-type] Return type does not match returned value: expected `(T, /) -> T`, found `Check[Unknown]`
- discord/ext/commands/core.py:2165:12: error[invalid-return-type] Return type does not match returned value: expected `(T, /) -> T`, found `Check[Unknown]`
- Found 527 diagnostics
+ Found 524 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- test/__init__.py:119:12: error[invalid-return-type] Return type does not match returned value: expected `(_TestFuncT, /) -> _TestFuncT`, found `Unknown | MarkDecorator`
- test/__init__.py:126:12: error[invalid-return-type] Return type does not match returned value: expected `(_TestFuncT, /) -> _TestFuncT`, found `Unknown | MarkDecorator`
- test/__init__.py:132:12: error[invalid-return-type] Return type does not match returned value: expected `(_TestFuncT, /) -> _TestFuncT`, found `Unknown | MarkDecorator`
- test/__init__.py:138:12: error[invalid-return-type] Return type does not match returned value: expected `(_TestFuncT, /) -> _TestFuncT`, found `Unknown | MarkDecorator`
- test/__init__.py:145:12: error[invalid-return-type] Return type does not match returned value: expected `(_TestFuncT, /) -> _TestFuncT`, found `Unknown | MarkDecorator`
- test/__init__.py:205:12: error[invalid-return-type] Return type does not match returned value: expected `(_TestFuncT, /) -> _TestFuncT`, found `Unknown | MarkDecorator`
- Found 397 diagnostics
+ Found 391 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/dbg/gdb/__init__.py:1663:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T, /) -> (...) -> T`, found `def exit(func: () -> T, **kwargs: Any) -> () -> T`
- pwndbg/dbg/gdb/__init__.py:1665:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T, /) -> (...) -> T`, found `def cont(func: () -> T, **kwargs: Any) -> () -> T`
- pwndbg/dbg/gdb/__init__.py:1667:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T, /) -> (...) -> T`, found `def start(func: () -> T, **kwargs: Any) -> () -> T`
- pwndbg/dbg/gdb/__init__.py:1669:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T, /) -> (...) -> T`, found `def stop(func: () -> T, **kwargs: Any) -> () -> T`
- pwndbg/dbg/gdb/__init__.py:1671:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T, /) -> (...) -> T`, found `def new_objfile(func: () -> T, **kwargs: Any) -> () -> T`
- pwndbg/dbg/gdb/__init__.py:1673:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T, /) -> (...) -> T`, found `def mem_changed(func: () -> T, **kwargs: Any) -> () -> T`
- pwndbg/dbg/gdb/__init__.py:1675:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T, /) -> (...) -> T`, found `def reg_changed(func: () -> T, **kwargs: Any) -> () -> T`
- Found 2260 diagnostics
+ Found 2253 diagnostics

meson (https://github.com/mesonbuild/meson)
- unittests/helpers.py:228:16: error[invalid-return-type] Return type does not match returned value: expected `((...) -> R, /) -> (...) -> R`, found `def expectedFailure(test_item: _FT) -> _FT`
- Found 757 diagnostics
+ Found 756 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/utilities/collections.py:395:36: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, VT]`, found `dict[str, VT] | None`
+ src/prefect/utilities/collections.py:395:36: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Unknown]`, found `dict[str, Unknown] | None`

zulip (https://github.com/zulip/zulip)
- zerver/lib/rest.py:189:40: error[invalid-argument-type] Argument is incorrect: Expected `_F`, found `(...) -> HttpResponse`
- zerver/lib/rest.py:205:40: error[invalid-argument-type] Argument is incorrect: Expected `_F`, found `(...) -> HttpResponse`
- zerver/views/auth.py:1178:1: error[invalid-argument-type] Argument is incorrect: Expected `_F`, found `def api_get_server_settings(request: HttpRequest) -> HttpResponse`
- zerver/views/development/email_log.py:52:1: error[invalid-argument-type] Argument is incorrect: Expected `_F`, found `def generate_all_emails(request: HttpRequest) -> HttpResponse`
- zerver/views/realm.py:572:1: error[invalid-argument-type] Argument is incorrect: Expected `_F`, found `def check_subdomain_available(request: HttpRequest, subdomain: str) -> HttpResponse`
- zerver/views/report.py:14:1: error[invalid-argument-type] Argument is incorrect: Expected `_F`, found `(...) -> Unknown`
- zerver/views/video_calls.py:298:1: error[invalid-argument-type] Argument is incorrect: Expected `_F`, found `(...) -> Unknown`
- Found 7322 diagnostics
+ Found 7315 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @LaBatata101 on 2025-06-11 20:31_

The implementation is probably wrong, given the mypy_primer results

---

_Label `ty` added by @AlexWaygood on 2025-06-11 21:16_

---

_Comment by @dcreager on 2025-06-12 00:23_

I'm just about to sign off for the night, but I can take a look at this first thing tomorrow!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:470 on 2025-06-23 14:37_

nit: to reduce indentation you can do this as

```suggestion
        if let (Some(self_gc), Some(other_gc)) = (self.generic_context, other.generic_context) {
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:474 on 2025-06-23 14:41_

nit: add a `use std::borrow::Cow` up top. You're using `Cow` enough here that it will look cleaner to not have to qualify it each time

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:479 on 2025-06-23 14:44_

Since you've pulled this type out and are using it multiple places, I think it deserves a `new` constructor method

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:487 on 2025-06-23 14:48_

Can you copy over the comment for this match arm from `is_assignable_to_impl`? That's helpful context to explain why some of these fall through and others immediately return

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:550 on 2025-06-23 14:48_

ditto re copying over comment

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:494 on 2025-06-23 14:53_

The comment for this arm in `is_assignable_to_impl` says this:

```
// For `self <: other` to be valid, if there are no more parameters in
// `other`, then the non-variadic parameters in `self` must have a default
// value.
```

We're not doing a subtype/assignability check here, so I think the `default_type` check isn't needed. Any parameters that don't "line up" should just be skipped, since there aren't respective typevars that we want to infer to be the same.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:506 on 2025-06-23 14:55_

Somewhat similar comment here — if both parameters aren't annotated, we can't possibly have any lining-up typevars to infer are the same. Instead of checking that they're both `Some` each time below, you can do the check once here:

```suggestion
                    let (Some(self_type), Some(other_type)) = (self_parameter.annotated_type(), other_parameter.annotated_type()) else {
                        continue;
                    };
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:533 on 2025-06-23 14:59_

hmm, although maybe my comment above isn't correct, since here (at least as you have it written) there's more work to do before you `continue` if there aren't annotations for both params

---

_@dcreager reviewed on 2025-06-23 15:00_

Here are some initial comments. I think it would be good to get @dhruvmanila's thoughts on this, too, since he was the original author of the `is_assignable_to_impl` method that you're basing this on

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:479 on 2025-06-24 03:36_

Agreed. I think we should also rename the `_self` and `_other` suffix to something else, it's mainly relevant when the type was private to the `is_assignable_to_impl` method because of the "self" and "other" references that already existed there. Maybe `_first` / `_second` or `_left` / `_right`?

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:507 on 2025-06-24 04:25_

The comment that Doug made above regarding the default value -- will it apply here as well?

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:728 on 2025-06-24 04:26_

nit: `specialized_signature` and in other places or combine the two lines if the variable isn't going to be used elsewhere

---

_@dhruvmanila reviewed on 2025-06-24 04:34_

It's a bit unfortunate that we need to copy the entire parameter loop over as it's quite complicated and will get even more so when adding support for `TypedDict` and `Unpack`.

I'm not exactly sure if there's an easy way to avoid duplicating the logic. We could accept a closure which is called for each pair of the parameter but then it will need to retain information that's required for both assignability/subtyping and specialization. There are also subtle differences between the two like the handling of default values.

I don't think it's worth spending time to de-duplicate right now but might be useful to do a time-blocked exploration in the future.

---

_Review comment by @LaBatata101 on `crates/ty_python_semantic/src/types/signatures.rs`:494 on 2025-06-24 22:30_

Should I remove that check and this one too?
https://github.com/astral-sh/ruff/blob/4ad855cc082c5a1c823223f4419eb1645e55b1ad/crates/ty_python_semantic/src/types/signatures.rs#L706-L710

---

_@LaBatata101 reviewed on 2025-06-24 22:30_

---

_Marked ready for review by @LaBatata101 on 2025-06-24 22:39_

---

_Review requested from @carljm by @LaBatata101 on 2025-06-24 22:39_

---

_Review requested from @AlexWaygood by @LaBatata101 on 2025-06-24 22:39_

---

_Review requested from @sharkdp by @LaBatata101 on 2025-06-24 22:39_

---

_Review requested from @dcreager by @LaBatata101 on 2025-06-24 22:40_

---

_Comment by @MichaReiser on 2025-06-25 06:31_

> It's a bit unfortunate that we need to copy the entire parameter loop over as it's quite complicated and will get even more so when adding support for TypedDict and Unpack.

Given the size of it (and that the divergences are very subtle), I do think it would make sense to invest some time into avoiding this duplication now

---

_@MichaReiser reviewed on 2025-06-25 06:33_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/signatures.rs`:452 on 2025-06-25 06:33_

I can't find `is_assignable_to_impl`

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-25 08:04_

---

_@dhruvmanila reviewed on 2025-06-25 08:21_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:619 on 2025-06-25 08:21_

Sorry if I was unclear but I didn't mean for you to change the "self" and "other" references in this method. It should only be changed in the `ParametersZip` struct and methods. We should keep using "self" and "other" in this method as they correspond to the parameter in the `self` and `other` signature and it makes it easier to follow along in the context of this method.

---

_@dhruvmanila reviewed on 2025-06-25 08:23_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:470 on 2025-06-25 08:23_

Same here, it's useful to have "self" and "other" in this context instead of "left" and "right" as we can easily infer which generic context this belongs to.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:452 on 2025-06-25 08:24_

It's down below on line 849

---

_@dhruvmanila reviewed on 2025-06-25 08:24_

---

_@MichaReiser reviewed on 2025-06-25 08:29_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/signatures.rs`:452 on 2025-06-25 08:29_

Oh, I think it got renamed on main (we should also double check if there are no behavior changes to it that need to be accounted for here)

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:452 on 2025-06-25 08:40_

Oh right, it's now `has_relation_to` as detected by the merge conflict

---

_@dhruvmanila reviewed on 2025-06-25 08:40_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:366 on 2025-06-25 20:12_

```suggestion
For generic callables, the identity of a TypeVar is not relevant for equivalence, as long
as the signatures are structurally compatible and the TypeVar bounds and constraints are equivalent.
Two callables that differ only in the names of their TypeVars should be equivalent.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:975 on 2025-06-25 20:13_

This test looks great. I think ideally we'd have a version of it using PEP 695 typevars also?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1585 on 2025-06-25 20:14_

```suggestion
For generic callables, the identity of a TypeVar is not relevant for subtyping, as long
as the signatures are structurally compatible and the TypeVar bounds and constraints are equivalent.
Two callables that differ only in the names of their TypeVars should be subtypes of each other.
```

---

_@carljm reviewed on 2025-06-25 20:16_

---

_@LaBatata101 reviewed on 2025-06-25 20:52_

---

_Review comment by @LaBatata101 on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:975 on 2025-06-25 20:52_

Thanks for the reminder, I will add them later. Now I have to figure it out how to apply my changes to the new changes made in `signatures.rs`, a lot has changed.

---

_@carljm reviewed on 2025-06-26 00:14_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:975 on 2025-06-26 00:14_

Sorry about all the conflicts!

---

_@LaBatata101 reviewed on 2025-06-26 14:04_

---

_Review comment by @LaBatata101 on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:975 on 2025-06-26 14:04_

No worries

---

_Converted to draft by @carljm on 2025-06-26 16:03_

---

_Comment by @carljm on 2025-06-26 16:04_

Marking as draft since there have been some reviews and some updates (and conflict resolutions) are needed. Please mark ready for review when you would like us to take another look!

---

_Comment by @LaBatata101 on 2025-06-30 23:03_

I've merged the implementation of `has_relation_to` and `infer_generic_specialization`, it wasn't possible to use a callback for the deduplication due to subtle differences in the implementation of the functions, so instead I used an enum to switch between the implementations. 

And, I've also added new tests, but there are two cases that are failing that I don't if they are supposed to be failing or not.
```python
from ty_extensions import static_assert, is_equivalent_to, CallableTypeOf
from typing import TypeVar, Callable

T = TypeVar("T")
T_bound = TypeVar("T_bound", bound=str)

def f[T](x: T) -> T:
    return x


def f_bound[T: str](x: T) -> T:
    return x

# These are failing
static_assert(is_equivalent_to(CallableTypeOf[f], Callable[[T], T]))
static_assert(is_equivalent_to(CallableTypeOf[f_bound], Callable[[T_bound], T_bound]))
```

---

_Marked ready for review by @LaBatata101 on 2025-06-30 23:03_

---

_Review requested from @carljm by @LaBatata101 on 2025-06-30 23:04_

---

_Review requested from @dhruvmanila by @LaBatata101 on 2025-06-30 23:04_

---

_Review requested from @MichaReiser by @LaBatata101 on 2025-06-30 23:05_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:449 on 2025-07-01 06:02_

Should we inline this method as I think it's used in a single place and the body only has one method call?

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:474 on 2025-07-01 06:03_

This also is something that seems like we could inline as we now don't have `is_gradual_equivalent_to` anymore. The main reason for this `_impl` method was to share the common logic between equivalence and gradual-equivalence check.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:566 on 2025-07-01 06:18_

If I understand this correctly, the `Ok` variant of the return type corresponds to the `SignatureCheckMode::Relation` variant while the `Err` variant corresponds to the `SignatureCheckMode::InferSpecialization` variant, is that correct?

If so, it's a bit unfortunate that we cannot encode this in the type system and the fact that the `Err` variant is only for a specific mode. I don't have a good solution to this apart from the fact that we use `.unwrap` in `has_relation_to` because it should always return `Ok(bool)` and if that is `Err` then I think it would be a logic error.

Maybe we should instead use a custom enum to reflect the return type which is according to the `mode` parameter:

```rs
enum SignatureCheckResult {
	Relation(bool),
	InferSpecialization(Result<(), SpecializationError>)
}

impl SignatureCheckResult {
	fn expect_relation(&self) -> bool {
		match self {
			Self::Relation(result) => result,
			Self::InferSpecialization(_) => panic!(...),
		}
	}

	fn expect_infer_specialization(&self) -> bool {
		match self {
			Self::Relation(_) => panic!(...),
			Self::InferSpecialization(result) => result,
		}
	}
}
```

But, I'm not too sold on this idea either, it just makes it explicit on what the relation is between the return type and the mode parameter.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:666 on 2025-07-01 06:25_

I think I'd prefer to not use this and instead just use `.annotated_type` method directly. It has the benefit that it's obvious what the type corresponds to (annotated parameter type) as compared to `self_type` which sounds a bit generic.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:1632 on 2025-07-01 06:27_

nit:

```suggestion
    /// Perform type relation check between two signatures.
```

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:1634 on 2025-07-01 06:31_

```suggestion
    /// Perform type inference between two annotated parameter types of the signatures.
```

---

_@dhruvmanila reviewed on 2025-07-01 06:34_

Thank you for combining the implementation of the parameter check loop!

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:155 on 2025-07-01 06:47_

Does this mean that we're potentially looping over all the parameters twice - first two infer the specialization and second to perform the type relation check?

---

_@dhruvmanila reviewed on 2025-07-01 06:47_

---

_Comment by @dhruvmanila on 2025-07-01 06:50_

> And, I've also added new tests, but there are two cases that are failing that I don't if they are supposed to be failing or not.

This is a bit hand wavy but could this be because the actual parameter loop of subtyping/assignability is different than the one for equivalence check but the parameter loop of constructing the specialization is the same for both?

---

_Comment by @LaBatata101 on 2025-07-01 13:58_

> > And, I've also added new tests, but there are two cases that are failing that I don't if they are supposed to be failing or not.
> 
> This is a bit hand wavy but could this be because the actual parameter loop of subtyping/assignability is different than the one for equivalence check but the parameter loop of constructing the specialization is the same for both?

Hmm, I don't know. It only happens when comparing the legacy typevar with the PEP695 typevar, the other equivalence cases are okay. But I'll take a look at that. 

---

_@LaBatata101 reviewed on 2025-07-01 14:00_

---

_Review comment by @LaBatata101 on `crates/ty_python_semantic/src/types/signatures.rs`:155 on 2025-07-01 14:00_

Yup, unfortunately.

---

_@LaBatata101 reviewed on 2025-07-01 14:06_

---

_Review comment by @LaBatata101 on `crates/ty_python_semantic/src/types/signatures.rs`:566 on 2025-07-01 14:06_

> If I understand this correctly, the Ok variant of the return type corresponds to the SignatureCheckMode::Relation variant while the Err variant corresponds to the SignatureCheckMode::InferSpecialization variant, is that correct?

yes

---

_Comment by @LaBatata101 on 2025-07-20 20:33_

Sorry for the delay on this. I've done some debugging and for this failing test case:
```python
from ty_extensions import (CallableTypeOf, is_equivalent_to,  static_assert)

T = TypeVar("T")


def f[T](x: T) -> T:
    return x


static_assert(is_equivalent_to(CallableTypeOf[f], Callable[[T], T]))
``` 

In the `is_equivalent_to` function the code execute this branch
https://github.com/astral-sh/ruff/blob/f026dbf9a17ed32b689002f261075431c6b77a06/crates/ty_python_semantic/src/types/signatures.rs#L516-L518

with these parameters
```
[crates/ty_python_semantic/src/types/signatures.rs:523:21] a = PositionalOrKeyword {
    name: Name("x"),
    default_type: None,
}
[crates/ty_python_semantic/src/types/signatures.rs:523:21] b = PositionalOnly {
    name: None,
    default_type: None,
}
```

how should they be handled?

---

_Comment by @carljm on 2025-07-22 01:55_

I think that test is simply incorrect? One of those callable types is a subtype of the other, but they are not equivalent, because one of them can accept the call `f(x=1)` and the other cannot (since it has a positional-only argument that can't be named as a keyword in a call).

I think modifying the test to assert that the one specified with `CallableTypeOf` is a subtype of the one specified with `Callable` would be a useful test (it would show that we apply this understanding of typevar equivalence not only in type equivalence but also in subtyping), and would be correct.

---

_Comment by @LaBatata101 on 2025-07-22 02:26_

> I think modifying the test to assert that the one specified with CallableTypeOf is a subtype of the one specified with Callable would be a useful test (it would show that we apply this understanding of typevar equivalence not only in type equivalence but also in subtyping), and would be correct.


I think I've put one like that in [crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md](https://github.com/astral-sh/ruff/pull/18634/files#diff-61ec926098dfa97912375f660390d53f75d61a8e0824f0a3afdaaf8b0442ded1)

---

_Assigned to @dcreager by @carljm on 2025-07-24 20:05_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:463 on 2025-07-28 21:43_

style nit to remove indentation:

```suggestion
        let (Some(self_gc), Some(other_gc)) = (self.generic_context, other.generic_context) else {
            return Cow::Borrowed(self);
        };
```

then all of the work to infer the specialization is unindented and looks like the "main" bit of work that the method does

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:566 on 2025-07-28 21:49_

You could also put the return value into the `SignatureCheckMode` enum:

```rust
enum SignatureCheckMode<'a, 'db> {
    Relation {
        relation: TypeRelation,
        result: bool,
    },
    InferSpecialization {
        builder: &'a mut SpecializationBuilder<'db>,
        result: Result<(), SpecializationError<'db>>,
    },
}
```

Then the signature of this method would become

```rust
    fn compare_with<'a>(
        &self,
        db: &'db dyn Db,
        other: &Signature<'db>,
        mode: &mut SignatureCheckMode<'a, 'db>,
    ) {
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:619 on 2025-07-28 21:51_

This is repeated a lot; I think it should become a method on `SignatureCheckMode`. Ideally you could get this down to where there are no `match mode` or equivalent checks inside of here, and all of that moves into logic on `SignatureCheckMode` itself

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:624 on 2025-07-28 21:51_

(This check, for instance, is morally equivalent to a `match mode` and so should ideally move into `SignatureMatchMode` as well)

---

_@dcreager reviewed on 2025-07-28 21:52_

A couple of comments that should help clean up `compare_with` and friends some more. Otherwise this is getting very close!

---

_Comment by @LaBatata101 on 2025-07-30 22:08_

I'll be working on the feedbacks in the weekend

---

_Comment by @LaBatata101 on 2025-08-03 23:04_

@dcreager I've applied your suggestions, but I think the code hasn't improved that much honestly. The code is currently failing to build because I don't know how to get the function definition to pass it to `GenericContext::from_function_params`, here:
https://github.com/astral-sh/ruff/blob/0d9b6951a3e2a4979135f2565d626a72f687158e/crates/ty_python_semantic/src/types/infer.rs#L10344-L10359

---

_Comment by @carljm on 2025-08-05 03:28_

Will leave review here to @dcreager, unless it seems like another pair of eyes would be useful.

---

_Review request for @carljm removed by @carljm on 2025-08-05 03:28_

---

_Review requested from @dcreager by @carljm on 2025-08-05 03:30_

---

_Comment by @dcreager on 2025-08-06 12:23_

> The code is currently failing to build because I don't know how to get the function definition to pass it to `GenericContext::from_function_params`, here:

To resolve this part, I think we should update `from_function_params` to take in `containing_scope: FileScopeId` instead of `definition: Definition`. In your new code in `infer.rs`, you'd get that `FileScopeId` via `self.scope().file_scope_id(self.db())`.

---

_Comment by @github-actions[bot] on 2025-08-06 19:40_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-06 19:40:29.595655223 +0000
+++ new-output.txt	2025-08-06 19:40:29.653655585 +0000
@@ -1,174 +1,58 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-_directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-_directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
-_directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@Spam`
-_directives_deprecated_library.py:41:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | float`
-_directives_deprecated_library.py:45:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
-aliases_explicit.py:41:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
-aliases_explicit.py:45:10: error[invalid-type-form] Variable of type `Literal["int | str"]` is not allowed in a type expression
-aliases_explicit.py:49:5: error[type-assertion-failure] Argument does not have asserted type `int | str`
-aliases_explicit.py:50:5: error[type-assertion-failure] Argument does not have asserted type `int | None`
-aliases_explicit.py:51:5: error[type-assertion-failure] Argument does not have asserted type `list[int | None]`
-aliases_explicit.py:52:5: error[type-assertion-failure] Argument does not have asserted type `list[int]`
-aliases_explicit.py:53:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, ...] | list[str]`
-aliases_explicit.py:54:5: error[type-assertion-failure] Argument does not have asserted type `tuple[int, int, int, str]`
-aliases_explicit.py:55:5: error[type-assertion-failure] Argument does not have asserted type `(...) -> int`
-aliases_explicit.py:56:5: error[type-assertion-failure] Argument does not have asserted type `(int, str, /) -> str`
-aliases_explicit.py:57:5: error[type-assertion-failure] Argument does not have asserted type `(int, str, str, /) -> None`
-aliases_explicit.py:59:5: error[type-assertion-failure] Argument does not have asserted type `int | str | None | list[list[int]]`
-aliases_explicit.py:60:5: error[type-assertion-failure] Argument does not have asserted type `(...) -> None`
-aliases_explicit.py:61:5: error[type-assertion-failure] Argument does not have asserted type `int | str`
-aliases_explicit.py:63:5: error[type-assertion-failure] Argument does not have asserted type `Literal[3, 4, 5] | None`
-aliases_explicit.py:98:1: error[type-assertion-failure] Argument does not have asserted type `list[str]`
-aliases_explicit.py:101:6: error[call-non-callable] Object of type `UnionType` is not callable
-aliases_implicit.py:54:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
-aliases_implicit.py:60:5: error[type-assertion-failure] Argument does not have asserted type `int | str`
-aliases_implicit.py:61:5: error[type-assertion-failure] Argument does not have asserted type `int | None`
-aliases_implicit.py:62:5: error[type-assertion-failure] Argument does not have asserted type `list[int | None]`
-aliases_implicit.py:63:5: error[type-assertion-failure] Argument does not have asserted type `list[int]`
-aliases_implicit.py:64:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, ...] | list[str]`
-aliases_implicit.py:65:5: error[type-assertion-failure] Argument does not have asserted type `tuple[int, int, int, str]`
-aliases_implicit.py:66:5: error[type-assertion-failure] Argument does not have asserted type `(...) -> int`
-aliases_implicit.py:67:5: error[type-assertion-failure] Argument does not have asserted type `(int, str, /) -> str`
-aliases_implicit.py:68:5: error[type-assertion-failure] Argument does not have asserted type `(int, str, str, /) -> None`
-aliases_implicit.py:70:5: error[type-assertion-failure] Argument does not have asserted type `int | str | None | list[list[int]]`
-aliases_implicit.py:71:5: error[type-assertion-failure] Argument does not have asserted type `list[bool]`
-aliases_implicit.py:72:5: error[type-assertion-failure] Argument does not have asserted type `(...) -> None`
-aliases_implicit.py:107:9: error[invalid-type-form] Variable of type `list[Unknown]` is not allowed in a type expression
-aliases_implicit.py:108:9: error[invalid-type-form] Variable of type `tuple[tuple[<class 'int'>, <class 'str'>]]` is not allowed in a type expression
-aliases_implicit.py:110:9: error[invalid-type-form] Variable of type `dict[Unknown, Unknown]` is not allowed in a type expression
-aliases_implicit.py:114:9: error[invalid-type-form] Variable of type `Literal[3]` is not allowed in a type expression
-aliases_implicit.py:115:10: error[invalid-type-form] Variable of type `Literal[True]` is not allowed in a type expression
-aliases_implicit.py:116:10: error[invalid-type-form] Variable of type `Literal[1]` is not allowed in a type expression
-aliases_implicit.py:118:10: error[invalid-type-form] Variable of type `Literal["int"]` is not allowed in a type expression
-aliases_implicit.py:119:10: error[invalid-type-form] Variable of type `Literal["int | str"]` is not allowed in a type expression
-aliases_implicit.py:128:1: error[type-assertion-failure] Argument does not have asserted type `list[str]`
-aliases_implicit.py:133:6: error[call-non-callable] Object of type `UnionType` is not callable
-aliases_newtype.py:15:1: error[type-assertion-failure] Argument does not have asserted type `int`
-aliases_newtype.py:18:1: error[invalid-assignment] Object of type `NewType` is not assignable to `type`
-aliases_newtype.py:26:21: error[invalid-base] Invalid class base with type `NewType`
-aliases_newtype.py:63:43: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 3, got 4
-aliases_type_statement.py:17:1: error[unresolved-attribute] Type `typing.TypeAliasType` has no attribute `bit_count`
-aliases_type_statement.py:19:1: error[call-non-callable] Object of type `TypeAliasType` is not callable
-aliases_type_statement.py:23:7: error[unresolved-attribute] Type `typing.TypeAliasType` has no attribute `other_attrib`
-aliases_type_statement.py:26:18: error[invalid-base] Invalid class base with type `typing.TypeAliasType`
-aliases_type_statement.py:37:22: error[invalid-type-form] Function calls are not allowed in type expressions
-aliases_type_statement.py:38:22: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
-aliases_type_statement.py:39:22: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression
-aliases_type_statement.py:39:23: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
-aliases_type_statement.py:40:22: error[invalid-type-form] List comprehensions are not allowed in type expressions
-aliases_type_statement.py:41:22: error[invalid-type-form] Dict literals are not allowed in type expressions
-aliases_type_statement.py:42:22: error[invalid-type-form] Function calls are not allowed in type expressions
-aliases_type_statement.py:43:28: error[invalid-type-form] Int literals are not allowed in this context in a type expression
-aliases_type_statement.py:44:22: error[invalid-type-form] `if` expressions are not allowed in type expressions
-aliases_type_statement.py:45:22: error[invalid-type-form] Variable of type `Literal[1]` is not allowed in a type expression
-aliases_type_statement.py:46:23: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
-aliases_type_statement.py:47:23: error[invalid-type-form] Int literals are not allowed in this context in a type expression
-aliases_type_statement.py:48:23: error[invalid-type-form] Boolean operations are not allowed in type expressions
-aliases_type_statement.py:49:23: error[fstring-type-annotation] Type expressions cannot use f-strings
-aliases_type_statement.py:80:37: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
-aliases_typealiastype.py:32:7: error[unresolved-attribute] Type `typing.TypeAliasType` has no attribute `other_attrib`
-aliases_typealiastype.py:39:26: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
-aliases_typealiastype.py:52:40: error[invalid-type-form] Function calls are not allowed in type expressions
-aliases_typealiastype.py:53:40: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
-aliases_typealiastype.py:54:42: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression
-aliases_typealiastype.py:54:43: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
-aliases_typealiastype.py:55:42: error[invalid-type-form] List comprehensions are not allowed in type expressions
-aliases_typealiastype.py:56:42: error[invalid-type-form] Dict literals are not allowed in type expressions
-aliases_typealiastype.py:57:42: error[invalid-type-form] Function calls are not allowed in type expressions
-aliases_typealiastype.py:58:48: error[invalid-type-form] Int literals are not allowed in this context in a type expression
-aliases_typealiastype.py:59:42: error[invalid-type-form] `if` expressions are not allowed in type expressions
-aliases_typealiastype.py:60:42: error[invalid-type-form] Variable of type `Literal[3]` is not allowed in a type expression
-aliases_typealiastype.py:61:42: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
-aliases_typealiastype.py:62:42: error[invalid-type-form] Int literals are not allowed in this context in a type expression
-aliases_typealiastype.py:63:42: error[invalid-type-form] Boolean operations are not allowed in type expressions
-aliases_typealiastype.py:64:42: error[invalid-type-form] F-strings are not allowed in type expressions
-aliases_typealiastype.py:66:47: error[unresolved-reference] Name `BadAlias21` used when not defined
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/generics_scoping.py`: `FunctionType < 'db >::signature_(Id(60800)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/dataclasses_transform_converter.py`: `FunctionType < 'db >::signature_(Id(69c00)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/constructors_callable.py`: `FunctionType < 'db >::signature_(Id(4f401)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/generics_self_advanced.py`: `FunctionType < 'db >::signature_(Id(49800)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/directives_cast.py`: `FunctionType < 'db >::signature_(Id(34006)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/generics_upper_bound.py`: `FunctionType < 'db >::signature_(Id(34007)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/callables_annotation.py`: `FunctionType < 'db >::signature_(Id(49800)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_implicit.py`: `FunctionType < 'db >::signature_(Id(3cc10)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/enums_members.py`: `FunctionType < 'db >::signature_(Id(32818)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/specialtypes_type.py`: `FunctionType < 'db >::signature_(Id(3400a)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/narrowing_typeis.py`: `FunctionType < 'db >::signature_(Id(66800)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/protocols_generic.py`: `FunctionType < 'db >::signature_(Id(4f409)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `FunctionType < 'db >::signature_(Id(3cc10)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/constructors_call_init.py`: `FunctionType < 'db >::signature_(Id(72415)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/generics_paramspec_semantics.py`: `FunctionType < 'db >::signature_(Id(53413)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/generics_paramspec_components.py`: `FunctionType < 'db >::signature_(Id(49800)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/directives_assert_type.py`: `FunctionType < 'db >::signature_(Id(49800)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/constructors_call_type.py`: `FunctionType < 'db >::signature_(Id(49c18)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/directives_reveal_type.py`: `FunctionType < 'db >::signature_(Id(4f400)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `FunctionType < 'db >::signature_(Id(3cc10)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/enums_expansion.py`: `FunctionType < 'db >::signature_(Id(49800)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/protocols_recursive.py`: `FunctionType < 'db >::signature_(Id(6081a)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/_directives_deprecated_library.py`: `FunctionType < 'db >::signature_(Id(49c1b)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/annotations_methods.py`: `FunctionType < 'db >::signature_(Id(87801)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/generics_defaults_specialization.py`: `FunctionType < 'db >::signature_(Id(49800)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/annotations_typeexpr.py`: `FunctionType < 'db >::signature_(Id(3cc10)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/literals_literalstring.py`: `FunctionType < 'db >::signature_(Id(49800)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_explicit.py`: `FunctionType < 'db >::signature_(Id(3cc10)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/dataclasses_usage.py`: `FunctionType < 'db >::signature_(Id(4980e)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/generics_basic.py`: `FunctionType < 'db >::signature_(Id(35c03)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/callables_protocol.py`: `FunctionType < 'db >::signature_(Id(72431)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/narrowing_typeguard.py`: `FunctionType < 'db >::signature_(Id(55020)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_newtype.py`: `FunctionType < 'db >::signature_(Id(69c18)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/generics_base_class.py`: `FunctionType < 'db >::signature_(Id(3cc25)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/generics_defaults.py`: `FunctionType < 'db >::signature_(Id(5e83a)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/classes_classvar.py`: `FunctionType < 'db >::signature_(Id(2ec1b)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/protocols_self.py`: `FunctionType < 'db >::signature_(Id(35c0f)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/annotations_coroutines.py`: `FunctionType < 'db >::signature_(Id(49800)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/generics_typevartuple_concat.py`: `FunctionType < 'db >::signature_(Id(5341d)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/enums_behaviors.py`: `FunctionType < 'db >::signature_(Id(8082c)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/tuples_unpacked.py`: `FunctionType < 'db >::signature_(Id(49800)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/generics_typevartuple_basic.py`: `FunctionType < 'db >::signature_(Id(69c18)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/dataclasses_transform_func.py`: `FunctionType < 'db >::signature_(Id(3402e)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/directives_deprecated.py`: `FunctionType < 'db >::signature_(Id(49c1b)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/dataclasses_kwonly.py`: `FunctionType < 'db >::signature_(Id(4980e)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py`: `FunctionType < 'db >::signature_(Id(87814)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/qualifiers_annotated.py`: `FunctionType < 'db >::signature_(Id(69c22)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/overloads_evaluation.py`: `FunctionType < 'db >::signature_(Id(49800)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b121ee4/src/function/execute.rs:202:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/annotations_generators.py`: `FunctionType < 'db >::signature_(Id(a703a)): execute: too many cycle iterations`
 aliases_variance.py:18:24: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[T_co]'>` with no `__class_getitem__` method
 aliases_variance.py:28:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[T_co]'>` with no `__class_getitem__` method
 aliases_variance.py:44:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassB[T_co, T_contra]'>` with no `__class_getitem__` method
-annotations_forward_refs.py:22:7: error[unresolved-reference] Name `ClassA` used when not defined
-annotations_forward_refs.py:23:12: error[unresolved-reference] Name `ClassA` used when not defined
-annotations_forward_refs.py:49:10: error[invalid-type-form] Variable of type `Literal[1]` is not allowed in a type expression
-annotations_forward_refs.py:54:11: error[fstring-type-annotation] Type expressions cannot use f-strings
-annotations_forward_refs.py:55:11: error[invalid-type-form] Variable of type `<module 'types'>` is not allowed in a type expression
-annotations_forward_refs.py:66:26: error[unresolved-reference] Name `ClassB` used when not defined
-annotations_forward_refs.py:80:14: error[unresolved-reference] Name `ClassF` used when not defined
-annotations_forward_refs.py:82:11: error[invalid-type-form] Variable of type `Literal[""]` is not allowed in a type expression
-annotations_forward_refs.py:87:9: error[invalid-type-form] Variable of type `def int(self) -> None` is not allowed in a type expression
-annotations_forward_refs.py:89:8: error[invalid-type-form] Variable of type `def int(self) -> None` is not allowed in a type expression
-annotations_forward_refs.py:95:1: error[type-assertion-failure] Argument does not have asserted type `str`
-annotations_forward_refs.py:96:1: error[type-assertion-failure] Argument does not have asserted type `int`
-annotations_generators.py:86:21: error[invalid-return-type] Return type does not match returned value: expected `int`, found `types.GeneratorType`
-annotations_generators.py:91:27: error[invalid-return-type] Return type does not match returned value: expected `int`, found `types.AsyncGeneratorType`
-annotations_generators.py:193:1: error[type-assertion-failure] Argument does not have asserted type `() -> AsyncIterator[int]`
-annotations_methods.py:31:1: error[type-assertion-failure] Argument does not have asserted type `A`
-annotations_methods.py:36:1: error[type-assertion-failure] Argument does not have asserted type `B`
-annotations_methods.py:42:1: error[type-assertion-failure] Argument does not have asserted type `A`
-annotations_methods.py:48:1: error[type-assertion-failure] Argument does not have asserted type `A`
-annotations_methods.py:49:1: error[type-assertion-failure] Argument does not have asserted type `B`
-annotations_methods.py:50:1: error[type-assertion-failure] Argument does not have asserted type `B`
-annotations_methods.py:51:1: error[type-assertion-failure] Argument does not have asserted type `A`
-annotations_typeexpr.py:88:9: error[invalid-type-form] Function calls are not allowed in type expressions
-annotations_typeexpr.py:89:9: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
-annotations_typeexpr.py:90:9: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
-annotations_typeexpr.py:91:9: error[invalid-type-form] List comprehensions are not allowed in type expressions
-annotations_typeexpr.py:92:9: error[invalid-type-form] Dict literals are not allowed in type expressions
-annotations_typeexpr.py:93:9: error[invalid-type-form] Function calls are not allowed in type expressions
-annotations_typeexpr.py:94:15: error[invalid-type-form] Int literals are not allowed in this context in a type expression
-annotations_typeexpr.py:95:9: error[invalid-type-form] `if` expressions are not allowed in type expressions
-annotations_typeexpr.py:96:9: error[invalid-type-form] Variable of type `Literal[3]` is not allowed in a type expression
-annotations_typeexpr.py:97:10: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
-annotations_typeexpr.py:98:10: error[invalid-type-form] Int literals are not allowed in this context in a type expression
-annotations_typeexpr.py:99:10: error[invalid-type-form] Unary operations are not allowed in type expressions
-annotations_typeexpr.py:100:10: error[invalid-type-form] Boolean operations are not allowed in type expressions
-annotations_typeexpr.py:101:10: error[fstring-type-annotation] Type expressions cannot use f-strings
-annotations_typeexpr.py:102:10: error[invalid-type-form] Variable of type `<module 'types'>` is not allowed in a type expression
-callables_annotation.py:25:5: error[missing-argument] No argument provided for required parameter 2
-callables_annotation.py:26:11: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Literal[2]`
-callables_annotation.py:27:15: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
-callables_annotation.py:29:5: error[missing-argument] No arguments provided for required parameters 1, 2
-callables_annotation.py:29:8: error[unknown-argument] Argument `a` does not match any known parameter
-callables_annotation.py:29:13: error[unknown-argument] Argument `b` does not match any known parameter
-callables_annotation.py:35:8: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
-callables_annotation.py:55:5: error[invalid-type-form] Special form `typing.Callable` expected exactly two arguments (parameter types and return type)
-callables_annotation.py:55:14: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
-callables_annotation.py:56:14: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
-callables_annotation.py:57:18: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `list[int]`?
-callables_annotation.py:58:5: error[invalid-type-form] Special form `typing.Callable` expected exactly two arguments (parameter types and return type)
-callables_annotation.py:58:14: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
-callables_kwargs.py:24:5: error[type-assertion-failure] Argument does not have asserted type `int`
-callables_kwargs.py:32:9: error[type-assertion-failure] Argument does not have asserted type `str`
-callables_kwargs.py:35:5: error[type-assertion-failure] Argument does not have asserted type `str`
-callables_kwargs.py:41:5: error[type-assertion-failure] Argument does not have asserted type `TD1`
-callables_kwargs.py:52:11: error[too-many-positional-arguments] Too many positional arguments to function `func1`: expected 0, got 3
-callables_kwargs.py:62:5: error[missing-argument] No argument provided for required parameter `v3` of function `func2`
-callables_kwargs.py:64:11: error[invalid-argument-type] Argument to function `func2` is incorrect: Expected `str`, found `Literal[1]`
-callables_kwargs.py:65:5: error[missing-argument] No argument provided for required parameter `v3` of function `func2`
-callables_protocol.py:97:1: error[invalid-assignment] Object of type `def cb4_bad1(x: int) -> None` is not assignable to `Proto4`
-callables_protocol.py:121:1: error[invalid-assignment] Object of type `def cb6_bad1(*vals: bytes, *, max_len: int | None = None) -> list[bytes]` is not assignable to `NotProto6`
-callables_protocol.py:179:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `R@__call__`
 callables_subtyping.py:26:5: error[invalid-assignment] Object of type `(int, /) -> int` is not assignable to `(int | float, /) -> int | float`
 callables_subtyping.py:29:5: error[invalid-assignment] Object of type `(int | float, /) -> int | float` is not assignable to `(int, /) -> int`
-classes_classvar.py:38:11: error[invalid-type-form] Type qualifier `typing.ClassVar` expected exactly 1 argument, got 2
-classes_classvar.py:39:14: error[invalid-type-form] Int literals are not allowed in this context in a type expression
-classes_classvar.py:40:14: error[unresolved-reference] Name `var` used when not defined
-classes_classvar.py:52:5: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to `list[str]`
-classes_classvar.py:55:17: error[invalid-type-form] Type qualifier `typing.ClassVar` is not allowed in type expressions (only in annotation expressions)
-classes_classvar.py:69:23: error[invalid-type-form] `ClassVar` is not allowed in function parameter annotations
-classes_classvar.py:70:12: error[invalid-type-form] `ClassVar` annotations are only allowed in class-body scopes
-classes_classvar.py:71:18: error[invalid-type-form] `ClassVar` annotations are not allowed for non-name targets
-classes_classvar.py:73:26: error[invalid-type-form] `ClassVar` is not allowed in function return type annotations
-classes_classvar.py:77:8: error[invalid-type-form] `ClassVar` annotations are only allowed in class-body scopes
-classes_classvar.py:111:1: error[invalid-attribute-access] Cannot assign to ClassVar `stats` from an instance of type `Starship`
-classes_classvar.py:140:1: error[invalid-assignment] Object of type `ProtoAImpl` is not assignable to `ProtoA`
-constructors_call_init.py:21:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `float`
-constructors_call_init.py:25:1: error[type-assertion-failure] Argument does not have asserted type `Class1[int | float]`
-constructors_call_init.py:56:1: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Class4[int]`, found `Class4[str]`
-constructors_call_init.py:75:1: error[type-assertion-failure] Argument does not have asserted type `Class5[int | float]`
-constructors_call_init.py:91:1: error[type-assertion-failure] Argument does not have asserted type `Class6[int, str]`
-constructors_call_init.py:99:1: error[type-assertion-failure] Argument does not have asserted type `Class7[str, int]`
-constructors_call_init.py:130:9: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
 constructors_call_metaclass.py:23:1: error[type-assertion-failure] Argument does not have asserted type `Never`
 constructors_call_metaclass.py:23:13: error[missing-argument] No argument provided for required parameter `x` of function `__new__`
 constructors_call_metaclass.py:36:1: error[type-assertion-failure] Argument does not have asserted type `int | Meta2`
@@ -190,32 +74,6 @@
 constructors_call_new.py:117:1: error[type-assertion-failure] Argument does not have asserted type `Class8[list[str]]`
 constructors_call_new.py:125:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@Class9`
 constructors_call_new.py:140:47: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Class11[int]`
-constructors_call_type.py:40:5: error[missing-argument] No arguments provided for required parameters `x`, `y` of function `__new__`
-constructors_call_type.py:50:5: error[missing-argument] No arguments provided for required parameters `x`, `y` of bound method `__init__`
-constructors_call_type.py:59:9: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
-constructors_callable.py:36:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:37:1: error[type-assertion-failure] Argument does not have asserted type `Class1`
-constructors_callable.py:49:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:50:1: error[type-assertion-failure] Argument does not have asserted type `Class2`
-constructors_callable.py:57:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@Class3`
-constructors_callable.py:63:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:64:1: error[type-assertion-failure] Argument does not have asserted type `Class3`
-constructors_callable.py:73:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-constructors_callable.py:77:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:78:1: error[type-assertion-failure] Argument does not have asserted type `int`
-constructors_callable.py:97:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:100:5: error[type-assertion-failure] Argument does not have asserted type `Never`
-constructors_callable.py:105:5: error[type-assertion-failure] Argument does not have asserted type `Never`
-constructors_callable.py:125:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:126:1: error[type-assertion-failure] Argument does not have asserted type `Class6Proxy`
-constructors_callable.py:142:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:162:5: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:164:1: error[type-assertion-failure] Argument does not have asserted type `Class7[int]`
-constructors_callable.py:165:1: error[type-assertion-failure] Argument does not have asserted type `Class7[str]`
-constructors_callable.py:182:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:183:1: error[type-assertion-failure] Argument does not have asserted type `Class8[str]`
-constructors_callable.py:193:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:194:1: error[type-assertion-failure] Argument does not have asserted type `Class9`
 dataclasses_descriptors.py:23:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | Desc1`
 dataclasses_descriptors.py:50:63: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `list[T@Desc2] | T@Desc2`
 dataclasses_descriptors.py:66:1: error[type-assertion-failure] Argument does not have asserted type `int`
@@ -227,20 +85,7 @@
 dataclasses_final.py:38:1: error[invalid-assignment] Cannot assign to final attribute `final_with_default` on type `<class 'D'>`
 dataclasses_frozen.py:16:1: error[invalid-assignment] Property `a` defined in `DC1` is read-only
 dataclasses_frozen.py:17:1: error[invalid-assignment] Property `b` defined in `DC1` is read-only
-dataclasses_kwonly.py:23:11: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
-dataclasses_kwonly.py:32:1: error[missing-argument] No argument provided for required parameter `a`
-dataclasses_kwonly.py:32:5: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["hi"]`
-dataclasses_kwonly.py:35:1: error[missing-argument] No argument provided for required parameter `a`
-dataclasses_kwonly.py:35:5: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["hi"]`
-dataclasses_kwonly.py:35:11: error[parameter-already-assigned] Multiple values provided for parameter `b`
-dataclasses_kwonly.py:38:5: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["hi"]`
-dataclasses_kwonly.py:38:11: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Literal[1]`
-dataclasses_kwonly.py:61:1: error[missing-argument] No argument provided for required parameter `c`
-dataclasses_kwonly.py:61:9: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `float`
-dataclasses_kwonly.py:61:14: error[parameter-already-assigned] Multiple values provided for parameter `b`
 dataclasses_order.py:50:4: error[unsupported-operator] Operator `<` is not supported for types `DC1` and `DC2`
-dataclasses_postinit.py:28:7: error[unresolved-attribute] Type `DC1` has no attribute `x`
-dataclasses_postinit.py:29:7: error[unresolved-attribute] Type `DC1` has no attribute `y`
 dataclasses_slots.py:56:1: error[unresolved-attribute] Type `<class 'DC5'>` has no attribute `__slots__`
 dataclasses_slots.py:57:1: error[unresolved-attribute] Type `DC5` has no attribute `__slots__`
 dataclasses_slots.py:66:1: error[unresolved-attribute] Type `<class 'DC6'>` has no attribute `__slots__`
@@ -266,61 +111,14 @@
 dataclasses_transform_class.py:106:24: error[unknown-argument] Argument `id` does not match any known parameter of bound method `__init__`
 dataclasses_transform_class.py:119:18: error[unknown-argument] Argument `id` does not match any known parameter of bound method `__init__`
 dataclasses_transform_class.py:119:24: error[unknown-argument] Argument `name` does not match any known parameter of bound method `__init__`
-dataclasses_transform_converter.py:25:6: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@model_field`
-dataclasses_transform_converter.py:48:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Unknown, /) -> Unknown`, found `def bad_converter1() -> int`
-dataclasses_transform_converter.py:49:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Unknown, /) -> Unknown`, found `def bad_converter2(*, x: int) -> int`
-dataclasses_transform_converter.py:107:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 6
-dataclasses_transform_converter.py:108:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 6
-dataclasses_transform_converter.py:109:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 6
-dataclasses_transform_converter.py:112:11: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 6
-dataclasses_transform_converter.py:114:1: error[invalid-assignment] Object of type `Literal["f1"]` is not assignable to attribute `field0` of type `int`
-dataclasses_transform_converter.py:115:1: error[invalid-assignment] Object of type `Literal["f6"]` is not assignable to attribute `field3` of type `ConverterClass`
-dataclasses_transform_converter.py:116:1: error[invalid-assignment] Object of type `Literal[b"f6"]` is not assignable to attribute `field3` of type `ConverterClass`
-dataclasses_transform_converter.py:119:1: error[invalid-assignment] Object of type `Literal[1]` is not assignable to attribute `field3` of type `ConverterClass`
-dataclasses_transform_converter.py:121:11: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 7
-dataclasses_transform_converter.py:130:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Literal[1], /) -> Unknown`, found `def converter_simple(s: str) -> int`
 dataclasses_transform_field.py:49:43: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> @Todo`
-dataclasses_transform_func.py:57:1: error[invalid-assignment] Object of type `Literal[3]` is not assignable to attribute `name` of type `str`
-dataclasses_transform_func.py:61:6: error[unsupported-operator] Operator `<` is not supported for types `Customer1` and `Customer1`
-dataclasses_transform_func.py:65:36: error[unknown-argument] Argument `salary` does not match any known parameter
-dataclasses_transform_func.py:77:36: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@create_model_frozen`
-dataclasses_transform_func.py:97:1: error[invalid-assignment] Property `id` defined in `Customer3` is read-only
 dataclasses_transform_meta.py:60:36: error[unknown-argument] Argument `other_name` does not match any known parameter
 dataclasses_transform_meta.py:73:6: error[unsupported-operator] Operator `<` is not supported for types `Customer1` and `Customer1`
 dataclasses_transform_meta.py:79:6: error[unsupported-operator] Operator `<` is not supported for types `Customer2` and `Customer2`
-dataclasses_usage.py:50:6: error[missing-argument] No argument provided for required parameter `unit_price`
-dataclasses_usage.py:51:28: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Literal["price"]`
-dataclasses_usage.py:52:36: error[too-many-positional-arguments] Too many positional arguments: expected 3, got 4
-dataclasses_usage.py:83:13: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
-dataclasses_usage.py:88:5: error[invalid-assignment] Object of type `dataclasses.Field[Literal[""]]` is not assignable to `int`
-dataclasses_usage.py:127:8: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
-dataclasses_usage.py:130:1: error[missing-argument] No argument provided for required parameter `y` of bound method `__init__`
-dataclasses_usage.py:179:6: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
-directives_assert_type.py:27:5: error[type-assertion-failure] Argument does not have asserted type `int`
-directives_assert_type.py:28:5: error[type-assertion-failure] Argument does not have asserted type `int`
-directives_assert_type.py:29:5: error[type-assertion-failure] Argument does not have asserted type `int`
-directives_assert_type.py:31:5: error[missing-argument] No arguments provided for required parameters `value`, `type` of function `assert_type`
-directives_assert_type.py:32:5: error[type-assertion-failure] Argument does not have asserted type `int`
-directives_assert_type.py:33:31: error[too-many-positional-arguments] Too many positional arguments to function `assert_type`: expected 2, got 3
-directives_cast.py:15:8: error[missing-argument] No arguments provided for required parameters `typ`, `val` of function `cast`
-directives_cast.py:16:13: error[invalid-type-form] Int literals are not allowed in this context in a type expression
-directives_cast.py:17:22: error[too-many-positional-arguments] Too many positional arguments to function `cast`: expected 2, got 3
-directives_deprecated.py:18:44: warning[deprecated] The class `Ham` is deprecated: Use Spam instead
-directives_deprecated.py:24:9: warning[deprecated] The function `norwegian_blue` is deprecated: It is pining for the fjords
-directives_deprecated.py:25:13: warning[deprecated] The function `norwegian_blue` is deprecated: It is pining for the fjords
-directives_deprecated.py:34:7: warning[deprecated] The class `Ham` is deprecated: Use Spam instead
-directives_deprecated.py:69:1: warning[deprecated] The function `lorem` is deprecated: Deprecated
-directives_deprecated.py:98:7: warning[deprecated] The function `foo` is deprecated: Deprecated
 directives_no_type_check.py:15:5: error[invalid-assignment] Object of type `Literal[""]` is not assignable to `int`
 directives_no_type_check.py:29:7: error[invalid-argument-type] Argument to function `func1` is incorrect: Expected `int`, found `Literal[b"invalid"]`
 directives_no_type_check.py:29:19: error[invalid-argument-type] Argument to function `func1` is incorrect: Expected `str`, found `Literal[b"arguments"]`
 directives_no_type_check.py:32:1: error[missing-argument] No arguments provided for required parameters `a`, `b` of function `func1`
-directives_reveal_type.py:14:17: info[revealed-type] Revealed type: `int | str`
-directives_reveal_type.py:15:17: info[revealed-type] Revealed type: `list[int]`
-directives_reveal_type.py:16:17: info[revealed-type] Revealed type: `Any`
-directives_reveal_type.py:17:17: info[revealed-type] Revealed type: `ForwardReference`
-directives_reveal_type.py:19:5: error[missing-argument] No argument provided for required parameter `obj` of function `reveal_type`
-directives_reveal_type.py:20:20: error[too-many-positional-arguments] Too many positional arguments to function `reveal_type`: expected 1, got 2
 directives_type_checking.py:11:5: error[invalid-assignment] Object of type `Literal[""]` is not assignable to `int`
 directives_type_checking.py:18:1: error[type-assertion-failure] Argument does not have asserted type `list[int]`
 directives_type_ignore_file2.py:14:1: error[invalid-assignment] Object of type `Literal[""]` is not assignable to `int`
@@ -332,11 +130,6 @@
 directives_version_platform.py:36:5: error[invalid-assignment] Object of type `Literal[""]` is not assignable to `int`
 directives_version_platform.py:40:5: error[invalid-assignment] Object of type `Literal[""]` is not assignable to `int`
 directives_version_platform.py:45:5: error[invalid-assignment] Object of type `Literal[""]` is not assignable to `int`
-enums_behaviors.py:27:1: error[type-assertion-failure] Argument does not have asserted type `Color`
-enums_behaviors.py:28:1: error[type-assertion-failure] Argument does not have asserted type `Literal[Color.RED]`
-enums_behaviors.py:32:1: error[type-assertion-failure] Argument does not have asserted type `Literal[Color.BLUE]`
-enums_behaviors.py:44:21: error[subclass-of-final-class] Class `ExtendedShape` cannot inherit from final class `Shape`
-enums_expansion.py:52:9: error[type-assertion-failure] Argument does not have asserted type `CustomFlags`
 enums_member_names.py:21:1: error[type-assertion-failure] Argument does not have asserted type `Literal["RED"]`
 enums_member_names.py:22:1: error[type-assertion-failure] Argument does not have asserted type `Literal["RED"]`
 enums_member_names.py:26:5: error[type-assertion-failure] Argument does not have asserted type `Literal["RED", "BLUE"]`
@@ -348,117 +141,19 @@
 enums_member_values.py:54:1: error[type-assertion-failure] Argument does not have asserted type `Literal[1]`
 enums_member_values.py:68:1: error[type-assertion-failure] Argument does not have asserted type `Literal[1]`
 enums_member_values.py:96:1: error[type-assertion-failure] Argument does not have asserted type `int`
-enums_members.py:128:21: info[revealed-type] Revealed type: `Unknown | Literal[2]`
-enums_members.py:129:9: error[type-assertion-failure] Argument does not have asserted type `Unknown`
-enums_members.py:129:43: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
-enums_members.py:146:1: error[type-assertion-failure] Argument does not have asserted type `int`
-enums_members.py:147:1: error[type-assertion-failure] Argument does not have asserted type `int`
-exceptions_context_managers.py:50:5: error[type-assertion-failure] Argument does not have asserted type `int | str`
-exceptions_context_managers.py:57:5: error[type-assertion-failure] Argument does not have asserted type `int | str`
-generics_base_class.py:26:26: error[invalid-argument-type] Argument to function `takes_dict_incorrect` is incorrect: Expected `dict[str, list[object]]`, found `SymbolTable`
-generics_base_class.py:29:14: error[invalid-type-form] `typing.Generic` is not allowed in type expressions
-generics_base_class.py:30:8: error[invalid-type-form] `typing.Generic` is not allowed in type expressions
-generics_base_class.py:45:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[int]`
-generics_base_class.py:49:22: error[too-many-positional-arguments] Too many positional arguments to class `LinkedList`: expected 1, got 2
-generics_base_class.py:61:18: error[too-many-positional-arguments] Too many positional arguments to class `MyDict`: expected 1, got 2
-generics_basic.py:34:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `AnyStr@concat` and `AnyStr@concat`
-generics_basic.py:139:5: error[type-assertion-failure] Argument does not have asserted type `int`
-generics_basic.py:140:5: error[type-assertion-failure] Argument does not have asserted type `int`
-generics_basic.py:157:5: error[invalid-argument-type] Method `__getitem__` of type `bound method MyMap1[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `Literal[0]` on object of type `MyMap1[str, int]`
-generics_basic.py:158:5: error[invalid-argument-type] Method `__getitem__` of type `bound method MyMap2[int, str].__getitem__(key: str, /) -> int` cannot be called with key of type `Literal[0]` on object of type `MyMap2[int, str]`
-generics_basic.py:162:12: error[invalid-argument-type] `<class 'int'>` is not a valid argument to `Generic`
-generics_basic.py:163:12: error[invalid-argument-type] `<class 'int'>` is not a valid argument to `Protocol`
-generics_basic.py:171:1: error[invalid-generic-class] `Generic` base class must include all type variables used in other base classes
-generics_basic.py:172:1: error[invalid-generic-class] `Generic` base class must include all type variables used in other base classes
-generics_basic.py:199:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[Any]`
-generics_defaults.py:30:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
-generics_defaults.py:31:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
-generics_defaults.py:32:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
-generics_defaults.py:38:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
-generics_defaults.py:45:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
-generics_defaults.py:46:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
-generics_defaults.py:50:1: error[missing-argument] No argument provided for required parameter `T2` of class `AllTheDefaults`
-generics_defaults.py:52:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
-generics_defaults.py:55:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
-generics_defaults.py:59:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
-generics_defaults.py:63:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
-generics_defaults.py:79:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
-generics_defaults.py:80:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
-generics_defaults.py:91:26: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
-generics_defaults.py:94:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
-generics_defaults.py:95:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
-generics_defaults.py:127:32: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T4@func1`
-generics_defaults.py:141:12: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
-generics_defaults.py:151:12: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
-generics_defaults.py:154:49: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int | float, bool]`?
-generics_defaults.py:155:58: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `list[bytes]`?
-generics_defaults.py:169:1: error[type-assertion-failure] Argument does not have asserted type `(Foo7[int], /) -> Foo7[int]`
 generics_defaults_referential.py:23:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
 generics_defaults_referential.py:36:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Literal[""]`
 generics_defaults_referential.py:37:10: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Literal[""]`
 generics_defaults_referential.py:94:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
 generics_defaults_referential.py:95:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
-generics_defaults_specialization.py:26:5: error[type-assertion-failure] Argument does not have asserted type `SomethingWithNoDefaults[int, str]`
-generics_defaults_specialization.py:27:5: error[type-assertion-failure] Argument does not have asserted type `SomethingWithNoDefaults[int, bool]`
-generics_defaults_specialization.py:30:1: error[non-subscriptable] Cannot subscript object of type `<class 'SomethingWithNoDefaults[int, DefaultStrT]'>` with no `__class_getitem__` method
-generics_defaults_specialization.py:45:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
 generics_paramspec_basic.py:27:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-generics_paramspec_components.py:83:18: error[parameter-already-assigned] Multiple values provided for parameter 1 (`x`) of function `foo`
-generics_paramspec_semantics.py:13:56: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> str`
-generics_paramspec_semantics.py:17:40: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-generics_paramspec_semantics.py:22:1: error[type-assertion-failure] Argument does not have asserted type `(str, bool, /) -> str`
-generics_paramspec_semantics.py:30:56: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> bool`
-generics_paramspec_semantics.py:34:28: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-generics_paramspec_semantics.py:38:28: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-generics_paramspec_semantics.py:53:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-generics_paramspec_semantics.py:57:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-generics_paramspec_semantics.py:76:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
-generics_paramspec_semantics.py:82:5: error[type-assertion-failure] Argument does not have asserted type `@Todo`
-generics_paramspec_semantics.py:82:28: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `list[int]`?
-generics_paramspec_semantics.py:84:5: error[type-assertion-failure] Argument does not have asserted type `(int, /) -> str`
-generics_paramspec_semantics.py:87:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-generics_paramspec_semantics.py:91:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> bool`
-generics_paramspec_semantics.py:101:54: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> bool`
-generics_paramspec_semantics.py:113:6: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> bool`
-generics_paramspec_semantics.py:128:20: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-generics_paramspec_semantics.py:133:23: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-generics_paramspec_semantics.py:138:29: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-generics_paramspec_semantics.py:143:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_specialization.py:32:27: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, bool]`?
 generics_paramspec_specialization.py:40:27: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
 generics_paramspec_specialization.py:40:31: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
 generics_paramspec_specialization.py:52:22: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str, bool]`?
-generics_scoping.py:14:1: error[type-assertion-failure] Argument does not have asserted type `int`
-generics_scoping.py:15:1: error[type-assertion-failure] Argument does not have asserted type `str`
-generics_scoping.py:42:1: error[type-assertion-failure] Argument does not have asserted type `str`
-generics_scoping.py:43:1: error[type-assertion-failure] Argument does not have asserted type `bytes`
-generics_self_advanced.py:11:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@ParentA`
-generics_self_advanced.py:18:1: error[type-assertion-failure] Argument does not have asserted type `ParentA`
-generics_self_advanced.py:19:1: error[type-assertion-failure] Argument does not have asserted type `ChildA`
-generics_self_advanced.py:28:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@ParentB`
-generics_self_advanced.py:35:9: error[type-assertion-failure] Argument does not have asserted type `Self@ChildB`
-generics_self_advanced.py:36:9: error[type-assertion-failure] Argument does not have asserted type `list[Self@ChildB]`
-generics_self_advanced.py:37:9: error[type-assertion-failure] Argument does not have asserted type `Self@ChildB`
-generics_self_advanced.py:38:9: error[type-assertion-failure] Argument does not have asserted type `Self@ChildB`
-generics_self_advanced.py:43:9: error[type-assertion-failure] Argument does not have asserted type `list[Self@ChildB]`
-generics_self_advanced.py:44:9: error[type-assertion-failure] Argument does not have asserted type `Self@ChildB`
-generics_self_advanced.py:45:9: error[type-assertion-failure] Argument does not have asserted type `Self@ChildB`
 generics_self_attributes.py:26:33: error[invalid-argument-type] Argument is incorrect: Expected `Self@LinkedList | None`, found `LinkedList[int]`
 generics_self_attributes.py:29:5: error[invalid-assignment] Object of type `OrdinalLinkedList` is not assignable to attribute `next` of type `Self@LinkedList | None`
 generics_self_attributes.py:32:5: error[invalid-assignment] Object of type `LinkedList[int]` is not assignable to attribute `next` of type `Self@LinkedList | None`
-generics_self_basic.py:14:9: error[type-assertion-failure] Argument does not have asserted type `Self@Shape`
-generics_self_basic.py:20:16: error[invalid-return-type] Return type does not match returned value: expected `Self@Shape`, found `Shape`
-generics_self_basic.py:33:16: error[invalid-return-type] Return type does not match returned value: expected `Self@Shape`, found `Shape`
-generics_self_basic.py:51:1: error[type-assertion-failure] Argument does not have asserted type `Shape`
-generics_self_basic.py:52:1: error[type-assertion-failure] Argument does not have asserted type `Circle`
-g...*[Comment body truncated]*

---

_Comment by @LaBatata101 on 2025-08-06 20:46_

I'm getting some tests failing with this error
```
unreachable.md - Unreachable code - No (incorrect) diagn… - Exhaustive check of … (7a81433212273398)

  crates/ty_python_semantic/resources/mdtest/unreachable.md:287 Attempting to "pull types" caused a panic at /home/labatata/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/86ca4a9/src/function/execute.rs:202:25
  crates/ty_python_semantic/resources/mdtest/unreachable.md:287 Note: either fix the panic or add the `<!-- pull-types:skip -->` directive to this test
  crates/ty_python_semantic/resources/mdtest/unreachable.md:287
  crates/ty_python_semantic/resources/mdtest/unreachable.md:287 FunctionType < 'db >::signature_(Id(1c0a)): execute: too many cycle iterations
  crates/ty_python_semantic/resources/mdtest/unreachable.md:287
  crates/ty_python_semantic/resources/mdtest/unreachable.md:287 run with `RUST_BACKTRACE=1` environment variable to display a backtrace
  crates/ty_python_semantic/resources/mdtest/unreachable.md:287 query stacktrace:
  crates/ty_python_semantic/resources/mdtest/unreachable.md:287    0: infer_scope_types(Id(801g1)) -> (R81, Durability::LOW)
  crates/ty_python_semantic/resources/mdtest/unreachable.md:287              at crates/ty_python_semantic/src/types/infer.rs:134
  crates/ty_python_semantic/resources/mdtest/unreachable.md:287

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='unreachable.md - Unreachable code - No (incorrect) diagn… - Exhaustive check of … (7a81433212273398)'
MDTEST_TEST_FILTER='unreachable.md - Unreachable code - No (incorrect) diagn… - Exhaustive check of … (7a81433212273398)' cargo test -p ty_python_semantic --test mdtest -- mdtest__unreachable
```

I don't know what's the cause of this, but I remember running the tests before https://github.com/astral-sh/ruff/pull/18634/commits/befb08e125bbcf95f2f8754e2d3fd77bc2e5fa22 and everything ran fine.  

---

_Comment by @dcreager on 2025-08-15 14:30_

First off, my apologies that this PR has dragged on as long as it has, @LaBatata101.

I discussed this a bit with the team over the past couple of days, and we no longer think the approach that I suggested in https://github.com/astral-sh/ty/issues/95#issuecomment-2923876231 is the right way to tackle this. Partly because of how tedious it has been to implement while fitting in with the rest of the type inference code. But more importantly, because we realized that this is a special case of a more general specialization constraint solver that we are implementing. https://github.com/astral-sh/ruff/pull/19838 will move us in the direction of finding typevar specializations _as part of_ doing an assignability check, instead of it being a standalone operation like it is at the moment. And I think that will end up subsuming https://github.com/astral-sh/ty/issues/95, since we should be able to infer the `{T = U}` specialization while doing the assignability check on `Callable[[T], T]` and `Callable[[U], U]`.

Given that, I think we should close this PR in favor of the more general constraint solver. If the new solver were many months out, it might be worth finishing this up and landing it as a stopgap. But we want to have the new solver in place for the beta, and it doesn't seem worth asking you to sink more time into this if will end up being replaced soon anyway.

---

_Closed by @carljm on 2025-08-15 16:30_

---
