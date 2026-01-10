```yaml
number: 16804
title: "[red-knot] Check subtype relation between callable types"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/callable-subtype
created_at: 2025-03-17T14:22:21Z
updated_at: 2025-03-24T15:23:56Z
url: https://github.com/astral-sh/ruff/pull/16804
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] Check subtype relation between callable types

---

_Pull request opened by @dhruvmanila on 2025-03-17 14:22_

## Summary

Part of #15382

This PR adds support for checking the subtype relationship between the two callable types.

The main source of reference used for implementation is https://typing.python.org/en/latest/spec/callables.html#assignability-rules-for-callables.

The implementation is split into two phases:
1. Check all the positional parameters which includes positional-only, standard (positional or keyword) and variadic kind
2. Collect all the keywords in a `HashMap` to do the keyword parameters check via name lookup

For (1), there's a helper struct which is similar to `.zip_longest` (from `itertools`) except that it allows control over one of the iterator as that's required when processing a variadic parameter. This is required because positional parameters needs to be checked as per their position between the two callable types. The struct also keeps track of the current iteration element because when the loop is exited (to move on to the phase 2) the current iteration element would be carried over to the phase 2 check.

This struct is internal to the `is_subtype_of` method as I don't think it makes sense to expose it outside. It also allows me to use "self" and "other" suffixed field names as that's only relevant in that context.

## Test Plan

Add extensive tests in markdown.

Converted all of the code snippets from https://typing.python.org/en/latest/spec/callables.html#assignability-rules-for-callables to use `knot_extensions.is_subtype_of` and verified the result.


---

_Label `red-knot` added by @dhruvmanila on 2025-03-17 14:22_

---

_Renamed from "[WIP] [red-knot] Fix fully static check for callable type" to "[WIP] [red-knot] Check subtype relation between callable types" by @dhruvmanila on 2025-03-17 14:22_

---

_Comment by @github-actions[bot] on 2025-03-17 14:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:607 on 2025-03-18 00:05_

I think we need to expand on this slightly; "behaves in the same way" isn't very clear what it's referring to.

Also, the first part seems to be phrased backward? If you take a pair of callable types, each with a single standard parameter, that form a subtype relation, you can't replace the standard parameter with a keyword-only parameter in the _subtype_ and have it "behave the same way", you can only do that in the supertype.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:503 on 2025-03-18 00:06_

Might be good to add some tests with more than 0-1 parameters?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:630 on 2025-03-18 00:06_

```suggestion
And, the same is true for positional-only parameter except that the names are not required to be the
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:678 on 2025-03-18 00:07_

Don't you mean "omitted in the supertype"?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:777 on 2025-03-18 00:07_

I guess we still similar need tests for keyword-variadic with keyword-only?

---

_@carljm reviewed on 2025-03-18 00:17_

This is looking really good! I don't have any significant advice based on what's here already; happy to consider any specific problems you're having with completing the PR.

I think that when we add assignability for callable types, it will make most sense to just expand this function to handle gradual types also and rename it `is_assignable_to`. Then the function can work both for `is_subtype_of` and `is_assignable_to`, in the former case it will just never receive a non-fully-static callable type.

---

_Comment by @github-actions[bot] on 2025-03-18 10:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @dhruvmanila on 2025-03-18 12:23_

> I think that when we add assignability for callable types, it will make most sense to just expand this function to handle gradual types also and rename it `is_assignable_to`. Then the function can work both for `is_subtype_of` and `is_assignable_to`, in the former case it will just never receive a non-fully-static callable type.

Yeah, I think that makes sense. For now, I did make the assumption that we're only getting fully static types so I avoided the fallback to `Unknown` if there is no annotated type, mainly to avoid any surprise behavior but that shouldn't be too difficult to change to `unwrap_or(Type::unknown())`. And, if the entire parameter type is using a gradual form (`...`) then we can directly short-circuit the implementation.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:607 on 2025-03-19 04:53_

Yeah, thanks for catching that, I've updated this.

---

_@dhruvmanila reviewed on 2025-03-19 04:53_

---

_@dhruvmanila reviewed on 2025-03-19 04:53_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:503 on 2025-03-19 04:53_

Added tests with multiple parameters.

---

_@dhruvmanila reviewed on 2025-03-19 04:54_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:678 on 2025-03-19 04:54_

Yes 🤦 

---

_@dhruvmanila reviewed on 2025-03-19 04:55_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:777 on 2025-03-19 04:55_

Yeah, I've added them, they were still WIP at that moment.

---

_Renamed from "[WIP] [red-knot] Check subtype relation between callable types" to "[red-knot] Check subtype relation between callable types" by @dhruvmanila on 2025-03-19 06:42_

---

_Marked ready for review by @dhruvmanila on 2025-03-19 10:54_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-03-19 10:54_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-03-19 10:54_

---

_Review requested from @dcreager by @dhruvmanila on 2025-03-19 10:54_

---

_@dhruvmanila reviewed on 2025-03-19 13:06_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:4912 on 2025-03-19 13:06_

I think I need to change this up because then there's no way to differentiate between keyword-variadic parameter being absent and that the annotated type is absent.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:4912 on 2025-03-19 13:10_

Done in https://github.com/astral-sh/ruff/pull/16804/commits/12ff6b9243317fed7ac7a947d3bf6951caf5b84b

---

_@dhruvmanila reviewed on 2025-03-19 13:10_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:545 on 2025-03-19 15:35_

```suggestion
The subtype can include any number of positional-only parameters as long as they have the default value:
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:663 on 2025-03-19 15:42_

Do we have any test that the reverse does not work? (Keyword-only in subtype cannot substitute for standard in supertype)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:701 on 2025-03-19 15:43_

Similar to above -- do we have any test that you cannot substitute a standard parameter in supertype with a positional-only in subtype?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:741 on 2025-03-19 15:47_

This describes the opposite situation from what is tested below -- below we test standard parameter in subtype and variadic or keyword-variadic in supertype.

But both directions should not work (and we should probably test both?). The only version that should work is substituting a standard parameter in supertype with _both_ a variadic and keyword-variadic parameter in subtype (both with compatible type).

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4990 on 2025-03-19 16:23_

It's possible that argument lists will tend to be short enough that it will be cheaper to use a Vec and just iterate through it than to pay the overhead of hashing. But I'm fine leaving this for a future performance investigation if we see this code as a hot spot. A mapping is semantically the appropriate data structure here.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4990 on 2025-03-19 16:26_

I think we probably should just return `false` here (with a comment that this case is only reachable with an invalid AST). It's important that we not panic on invalid AST. (It would also be worth adding a corpus test with an invalid function definition that triggers this branch, if that's possible.)

---

_@carljm approved on 2025-03-19 16:27_

This looks really good!! Quite a complex feature, thanks for working through it so thoroughly.

---

_@dhruvmanila reviewed on 2025-03-20 11:45_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:663 on 2025-03-20 11:45_

Yes, https://github.com/astral-sh/ruff/blob/12ff6b9243317fed7ac7a947d3bf6951caf5b84b/crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md#keyword-only-with-standard

This combination `(keyword-only, positional-or-keyword)` is handled by the catch-all branch in the first loop.

---

_@dhruvmanila reviewed on 2025-03-20 11:47_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:701 on 2025-03-20 11:47_

Yes, https://github.com/astral-sh/ruff/blob/12ff6b9243317fed7ac7a947d3bf6951caf5b84b/crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md#positional-only-with-other-kinds

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:4990 on 2025-03-20 11:58_

Yeah, another option is to use [`smallmap`](https://docs.rs/smallmap/latest/smallmap/index.html) crate.

---

_@dhruvmanila reviewed on 2025-03-20 11:58_

---

_@dhruvmanila reviewed on 2025-03-20 12:09_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:4990 on 2025-03-20 12:09_

I'm not able to get any syntax to trigger this branch this can only occur if there are any other parameter after a keyword-only or a keyword-variadic parameter. I think a lot of the invalid cases are handled by the catch-all case in the previous loop and the parser will never be able to construct positional-only / standard parameter after a keyword-only / keyword-variadic parameter is encountered. But, I'll still update this to return false as we want red-knot to be resilient here.

---

_@dhruvmanila reviewed on 2025-03-20 12:11_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:741 on 2025-03-20 12:11_

> This describes the opposite situation from what is tested below -- below we test standard parameter in subtype and variadic or keyword-variadic in supertype.

Thanks, I've fixed the sentence.

> But both directions should not work (and we should probably test both?). The only version that should work is substituting a standard parameter in supertype with _both_ a variadic and keyword-variadic parameter in subtype (both with compatible type).

Yes, there was a test for the opposite direction but it was limited to standard parameter, I expanded it to consider other kinds as well here: https://github.com/astral-sh/ruff/blob/a519c2e011e0c2468fbcbc392a359c766f05d064/crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md#variadic-with-other-kinds

---

_@carljm approved on 2025-03-20 23:06_

This is really excellent!!

---

_@carljm reviewed on 2025-03-20 23:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:665 on 2025-03-20 23:08_

Seems like we can get rid of this TODO now? Or at least replace it with something more specific?
```suggestion
                // TODO: Implement subtyping between general callable types and other types (function literals, class literals, `type[]`...)
```

---

_Merged by @dhruvmanila on 2025-03-21 03:27_

---

_Closed by @dhruvmanila on 2025-03-21 03:27_

---

_Branch deleted on 2025-03-21 03:27_

---

_Comment by @sharkdp on 2025-03-24 14:10_

Very nice work! One minor question: should `Callable`s be a subtype of `object`? Should this pass?

```py
from knot_extensions import static_assert, is_subtype_of
from typing import Callable

static_assert(is_subtype_of(Callable[[int], str], object))
```

https://playknot.ruff.rs/2fd03ad6-c4d9-45e5-bde5-8f2d1ec55abc


---

_Comment by @dhruvmanila on 2025-03-24 14:56_

> One minor question: should `Callable`s be a subtype of `object`? Should this pass?

Hmm, good question. I think not because `object` is not callable.

---

_Comment by @AlexWaygood on 2025-03-24 14:59_

> Hmm, good question. I think not because object is not callable.

That's opposite to the way you should think about it -- all fully static types should be a subtype of `object`.

Consider the way that all instance types are a subtype of `object`. For example, the `bool` type extends the features and properties of `object` (it is an (indirect) subtype of `object`); thus `bool` is a subtype of `object` even though not all instances of `object` are instances of `bool`.

---

_Comment by @carljm on 2025-03-24 15:00_

Callables (fully static ones) should be a subtype of `object`. The type `object` is inhabited by all Python objects, so all (fully static) types are subtypes of `object`.

It is true that all objects are not callable, but this means that object is not a subtype of Callable. 

In my understanding this PR wasn't aiming to be comprehensive yet, it was just handling the core calculation of signature subtyping. We have other relations missing as well, eg a FunctionLiteral can be a subtype of a Callable type. Also an Instance type with a `__call__` method. And a ClassLiteral type (based on its constructor signature.) And arguably SubclassOf as well, though this one is not sound since we won't enforce Liskov on constructor signatures. 

---

_Comment by @dhruvmanila on 2025-03-24 15:15_

> Callables (fully static ones) should be a subtype of `object`. The type `object` is inhabited by all Python objects, so all (fully static) types are subtypes of `object`.

Yeah, I think I got it backwards, thanks.

> In my understanding this PR wasn't aiming to be comprehensive yet, it was just handling the core calculation of signature subtyping. We have other relations missing as well, eg a FunctionLiteral can be a subtype of a Callable type. Also an Instance type with a `__call__` method. And a ClassLiteral type (based on its constructor signature.) And arguably SubclassOf as well, though this one is not sound since we won't enforce Liskov on constructor signatures.

Yes, that is correct. I'll open an issue to keep track of the remaining relationship, it might be something that a contributor could pick up.

Edit: https://github.com/astral-sh/ruff/issues/16953

---
