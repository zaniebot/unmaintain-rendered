```yaml
number: 21883
title: "[ty] default-specialize class-literal types in assignment to generic-alias types"
type: pull_request
state: merged
author: carljm
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: cjm/unspecassign
created_at: 2025-12-10T02:10:42Z
updated_at: 2025-12-10T16:18:11Z
url: https://github.com/astral-sh/ruff/pull/21883
synced_at: 2026-01-10T16:42:11Z
```

# [ty] default-specialize class-literal types in assignment to generic-alias types

---

_Pull request opened by @carljm on 2025-12-10 02:10_

Fixes https://github.com/astral-sh/ty/issues/1832, fixes https://github.com/astral-sh/ty/issues/1513

## Summary

A class object `C` (for which we infer an unspecialized `ClassLiteral` type) should always be assignable to the type `type[C]` (which is default-specialized, if `C` is generic). We already implemented this for most cases, but we missed the case of a generic final type, where we simplify `type[C]` to the `GenericAlias` type for the default specialization of `C`. So we also need to implement this assignability of generic `ClassLiteral` types as-if default-specialized. 

## Test Plan

Added mdtests that failed before this PR.


---

_Label `ty` added by @carljm on 2025-12-10 02:10_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-10 02:10_

---

_Comment by @astral-sh-bot[bot] on 2025-12-10 02:12_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-10 02:14_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- bson/son.py:43:31: error[invalid-assignment] Object of type `<class 'Pattern[str]'>` is not assignable to `<class 'Pattern[Any]'>`
- Found 462 diagnostics
+ Found 461 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/_vendor/typing_extensions.py:3279:21: error[invalid-argument-type] Argument to bound method `register` is incorrect: Expected `<class 'memoryview[int]'>`, found `<class 'memoryview'>`
- Found 1289 diagnostics
+ Found 1288 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_channel.py:250:16: error[invalid-argument-type] Argument to bound method `_create` is incorrect: Expected `<class 'MemorySendChannel[SendType@MemorySendChannel]'>`, found `<class 'MemorySendChannel'>`
- src/trio/_channel.py:402:16: error[invalid-argument-type] Argument to bound method `_create` is incorrect: Expected `<class 'MemoryReceiveChannel[ReceiveType@MemoryReceiveChannel]'>`, found `<class 'MemoryReceiveChannel'>`
- Found 495 diagnostics
+ Found 493 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/internal/settings/symbol_db.py:25:9: error[invalid-argument-type] Argument to bound method `d` is incorrect: Expected `<class 'Pattern[Unknown]'>`, found `<class 'Pattern'>`
- Found 8471 diagnostics
+ Found 8470 diagnostics

egglog-python (https://github.com/egraphs-good/egglog-python)
- python/egglog/exp/any_expr.py:643:11: error[invalid-argument-type] Argument to function `converter` is incorrect: Expected `<class 'slice[Any, Any, Any]'>`, found `<class 'slice'>`
- python/egglog/exp/array_api.py:989:5: error[invalid-argument-type] Argument to function `converter` is incorrect: Expected `<class 'slice[Any, Any, Any]'>`, found `<class 'slice'>`
- Found 1492 diagnostics
+ Found 1490 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @carljm on 2025-12-10 02:19_

Looks like this fixes a few current false positives on the ecosystem, but the main point of this is to unblock type-of-cls on classmethods, where this starts to come up a lot more.

---

_Marked ready for review by @carljm on 2025-12-10 02:25_

---

_Review requested from @AlexWaygood by @carljm on 2025-12-10 02:25_

---

_Review requested from @sharkdp by @carljm on 2025-12-10 02:25_

---

_Review requested from @dcreager by @carljm on 2025-12-10 02:25_

---

_Review requested from @ibraheemdev by @carljm on 2025-12-10 03:49_

---

_@dhruvmanila approved on 2025-12-10 05:42_

Can you also add similar tests for `ParamSpec`? I think that should just work but if not, you can add a TODO comment and I can look at it later:

```py
from typing import final


@final
class Foo[**P]: ...


def expects_type_foo(f: type[Foo]): ...
def expects_type_foo_of_int(x: type[Foo[int]]): ...


expects_type_foo(Foo)  # ok
expects_type_foo_of_int(Foo[int])  # ok
expects_type_foo_of_int(Foo[str])  # not ok


@final
class FooDefault[**P = [int, str]]: ...


def expects_type_foo_default(f: type[FooDefault]): ...
def expects_type_foo_default_of_int(f: type[FooDefault[int]]): ...
def expects_type_foo_default_of_int_str(f: type[FooDefault[int, str]]): ...


expects_type_foo_default(FooDefault)  # ok
expects_type_foo_default(FooDefault[int, str])  # ok
expects_type_foo_default_of_int_str(FooDefault)  # ok

expects_type_foo_default(FooDefault[int])  # not ok
expects_type_foo_default_of_int(FooDefault[str])  # not ok
expects_type_foo_default_of_int_str(FooDefault[str, int])  # not ok
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2713 on 2025-12-10 07:55_

`C[...]` is the display we use for "instance of a specialised version of `C`", not "the specialised version of `C` itself"

```suggestion
            // Similarly, `<class 'C'>` is assignable to `<class 'C[...]'>` (a generic-alias type)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2716 on 2025-12-10 07:57_

```suggestion
            // generic-alias type `<class 'C[...]'>`, due to the fact that `C[...]` has no subclasses.
```

---

_@AlexWaygood approved on 2025-12-10 07:58_

---

_@sharkdp reviewed on 2025-12-10 10:12_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_of/generics.md`:357 on 2025-12-10 10:12_

I wanted to add two tests here and noticed that they were not succeeding, but I think they should?
```py
expects_type_p(P[int])
expects_type_p(P[str])
```

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-12-10 10:26_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-12-10 10:26_

---

_Comment by @sharkdp on 2025-12-10 10:36_

@dhruvmanila @AlexWaygood could one of you please review my changes here?

(@dhruvmanila, I noticed that some of the `ParamSpec` tests were correctly issuing errors before I added that new `match` branch, but I guess that was just an artifact? Something seems to be missing there for `ParamSpec` specifically)

---

_Comment by @astral-sh-bot[bot] on 2025-12-10 10:36_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 6 | 0 |
| `invalid-assignment` | 0 | 1 | 0 |
| `unsupported-base` | 0 | 1 | 0 |
| **Total** | **0** | **8** | **0** |

**[Full report with detailed diff](https://cjm-unspecassign.ecosystem-663.pages.dev/diff)** ([timing results](https://cjm-unspecassign.ecosystem-663.pages.dev/timing))




---

_Comment by @AlexWaygood on 2025-12-10 10:41_

Does this now also fix https://github.com/astral-sh/ty/issues/1513? Or do we also need to make some changes to `is_disjoint_from` for that?

Are the new ecosystem hits on `pandas` expected?

---

_Comment by @sharkdp on 2025-12-10 10:43_

> Does this now also fix [astral-sh/ty#1513](https://github.com/astral-sh/ty/issues/1513)? Or do we also need to make some changes to `is_disjoint_from` for that?

It looks like it does!



> Are the new ecosystem hits on `pandas` expected?

Looking into it

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2730 on 2025-12-10 10:43_

nit

```suggestion
                ClassType::Generic(self_alias).has_relation_to_impl(
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2732 on 2025-12-10 10:44_

```suggestion
                    ClassType::Generic(target_alias),
```

---

_@AlexWaygood reviewed on 2025-12-10 10:44_

---

_Comment by @AlexWaygood on 2025-12-10 10:45_

> > Does this now also fix [astral-sh/ty#1513](https://github.com/astral-sh/ty/issues/1513)? Or do we also need to make some changes to `is_disjoint_from` for that?
> 
> It looks like it does!

Amazing! Is it worth adding some tests for that specifically?



---

_Comment by @AlexWaygood on 2025-12-10 10:48_

Might want to double-check our alias<->class disjointness logic anyway, in case it's now inconsistent with our subtyping logic for these types 

---

_Comment by @dhruvmanila on 2025-12-10 11:30_

> I noticed that some of the `ParamSpec` tests were correctly issuing errors before I added that new `match` branch, but I guess that was just an artifact? Something seems to be missing there for `ParamSpec` specifically)

I tried out this PR (without the new branch that you added) and it seems to be the following branch that's handling it:

```rs
            (Type::GenericAlias(alias), _) => ClassType::from(alias)
                .metaclass_instance_type(db)
                .has_relation_to_impl(
                    db,
                    target,
                    inferable,
                    relation,
                    relation_visitor,
                    disjointness_visitor,
                ),
```

I might have to spend some time here to understand if there's anything missing or not.

---

_Comment by @dhruvmanila on 2025-12-10 13:50_

Ah, I think it's because the inferred variance of a `ParamSpec` is always `Bivariant`. We'll need to change this up to either use `Invariant` (which is what Pyright uses) or `Covariant` (which is that mypy uses) based on the following example:

```py
from typing import final

class A: ...
class B(A): ...
class C(B): ...
class D: ...

@final
class Foo[**P]: ...

def expects_type_foo_of_b(x: type[Foo[B]]): ...

expects_type_foo_of_b(Foo[A])  # only Pyright error
expects_type_foo_of_b(Foo[B])  # ok
expects_type_foo_of_b(Foo[C])  # both Pyright and mypy error
expects_type_foo_of_b(Foo[D])  # both Pyright and mypy error
```

I can do it as a follow-up unless you want to add it in this PR.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-12-10 13:59_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-12-10 13:59_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2720 on 2025-12-10 14:17_

```suggestion
                    ClassType::Generic(target_alias),
```

---

_@AlexWaygood approved on 2025-12-10 14:17_

LGTM, though I'd still love to double-check https://github.com/astral-sh/ruff/pull/21883#issuecomment-3636475909 (do the property tests pass on this branch?)

---

_Comment by @codspeed-hq[bot] on 2025-12-10 14:18_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Funspecassign?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21883 will **not alter performance**

<sub>Comparing <code>cjm/unspecassign</code> (1138855) with <code>main</code> (7bf50e7)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/cjm%2Funspecassign?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_of/generics.md`:453 on 2025-12-10 14:20_

This test should probably go in https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/narrow/issubclass.md ? Might also be nice to have a test that uses a TypeVar with a default, since I initially encountered this weirdness with `memoryview` 

---

_@AlexWaygood reviewed on 2025-12-10 14:20_

---

_Comment by @sharkdp on 2025-12-10 14:34_

> LGTM, though I'd still love to double-check [#21883 (comment)](https://github.com/astral-sh/ruff/pull/21883#issuecomment-3636475909) (do the property tests pass on this branch?)

Yes, sorry. This was not a "please review this again" push, but rather just a "I want to see if the pandas false positives are gone now" push. I didn't want to move Carl's PR to draft :smile: 

---

_Comment by @sharkdp on 2025-12-10 15:40_

> I'd still love to double-check [#21883 (comment)](https://github.com/astral-sh/ruff/pull/21883#issuecomment-3636475909) (do the property tests pass on this branch?)

The property tests pass on this branch, which might just be because we don't have the necessary "ingredients" in the type pool. I added some tests for `is_assignable_to` (all of which pass), and some tests for `is_disjoint_from` (some of which fail, but they also fail on `main`). I tried for a while to implement the necessary disjointness relations, but didn't get there. I'd like to postpone this (I'll create a ticket).

---

_Comment by @AlexWaygood on 2025-12-10 15:44_

> I'd like to postpone this

Fine by me!

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_of/generics.md`:399 on 2025-12-10 15:49_

Looking at this now it feels like we should also have this test:
```suggestion
expects_type_p_of_int(P[str])  # error: [invalid-argument-type]
expects_type_p_of_int(P)  # error: [invalid-argument-type]
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_of/generics.md`:394 on 2025-12-10 15:49_

I guess we could also test this:
```suggestion
expects_type_p_of_str(P)
expects_type_p_of_str(P[str])
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_of/generics.md`:426 on 2025-12-10 15:50_

Do we know what we are lacking here? Is there an issue to track it?

---

_@carljm reviewed on 2025-12-10 15:52_

Looks good, thank you for the additional fixes!

---

_@sharkdp reviewed on 2025-12-10 15:54_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_of/generics.md`:399 on 2025-12-10 15:54_

Done, it passes.

---

_@sharkdp reviewed on 2025-12-10 15:54_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_of/generics.md`:394 on 2025-12-10 15:54_

Done, it passes.

---

_@sharkdp reviewed on 2025-12-10 15:55_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_of/generics.md`:426 on 2025-12-10 15:55_

@dhruvmanila said he'll look into it.

---

_Comment by @carljm on 2025-12-10 15:57_

> I think it's because the inferred variance of a `ParamSpec` is always `Bivariant`. We'll need to change this up to either use `Invariant` (which is what Pyright uses) or `Covariant` (which is that mypy uses)

I think inferring an "unused" ParamSpec type parameter (as in your example, and as in the current tests here) as bivariant is correct, according to our current variance inference logic. We do have an issue open to change this and always infer covariant for unused typevars: https://github.com/astral-sh/ty/issues/1728. When we do this, it should naturally impact ParamSpec type parameters also, I think.

If we want to match pyright and always infer all ParamSpec as invariant, regardless of use, that would be a separate change specific to ParamSpec. But I'm not sure I see the rationale for that.

---

_@carljm reviewed on 2025-12-10 15:58_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_of/generics.md`:426 on 2025-12-10 15:58_

I think we should just add a use of `P` in the body of `C`, and then we will infer `P` as covariant (or invariant, depending what kind of use we add) and these should be fixed. That's what we do in the non-ParamSpec tests here already.

---

_@sharkdp reviewed on 2025-12-10 16:17_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_of/generics.md`:426 on 2025-12-10 16:17_

I didn't manage to add `P` in an invariant or covariant position, and adding it contravariantly does not fix the tests. After talking to Carl, we'll postpone this as well. I'll open an issue.

---

_Merged by @sharkdp on 2025-12-10 16:18_

---

_Closed by @sharkdp on 2025-12-10 16:18_

---

_Branch deleted on 2025-12-10 16:18_

---
