```yaml
number: 17301
title: "[red-knot] Super-basic generic inference at call sites"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/infer-function-calls
created_at: 2025-04-08T21:00:29Z
updated_at: 2025-04-16T19:07:37Z
url: https://github.com/astral-sh/ruff/pull/17301
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Super-basic generic inference at call sites

---

_Pull request opened by @dcreager on 2025-04-08 21:00_

This PR adds **_very_** basic inference of generic typevars at call sites. It does not bring in a full unification algorithm, and there are a few TODOs in the test suite that are not discharged by this. But it handles a good number of useful cases! And the PR does not add anything that would go away with a more sophisticated constraint solver.

In short, we just look for typevars in the formal parameters, and assume that the inferred type of the corresponding argument is what that typevar should map to. If a typevar appears more than once, we union together the corresponding argument types.

Cases we are not yet handling:

- We are not widening literals.
- We are not recursing into parameters that are themselves generic aliases.
- We are not being very clever with parameters that are union types.

This depends on #17023.

---

_Label `red-knot` added by @dcreager on 2025-04-08 21:00_

---

_Review requested from @carljm by @dcreager on 2025-04-08 21:00_

---

_Review requested from @AlexWaygood by @dcreager on 2025-04-08 21:00_

---

_Review requested from @sharkdp by @dcreager on 2025-04-08 21:00_

---

_Comment by @github-actions[bot] on 2025-04-08 21:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:150 on 2025-04-09 04:22_

I am not actually convinced there's a strong rationale for this TODO. I don't think there is any function body for `two_params` that would type-check and allow it to return anything other than `"a"` or `"b"`. Same for a number of similar TODOs below.

But I could be wrong; not asking to have these TODOs removed in this PR, just putting it out there that I'm not convinced :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:168 on 2025-04-09 04:25_

Interesting case not covered here: `param_with_union(3, 1)` should be a valid call, and must return `Literal[1]`, not `Literal[3]`, for the same reason discussed above.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:162 on 2025-04-09 04:30_

This case is a bit subtle, I had to actually try to write a body for `param_with_union` in which we could return `1` from this call, before realizing it's not possible because `T | int` is not assignable to `T`, so before we can return `x` we have to exclude `int` from its type, meaning this call can't return `1`.

---

_@carljm reviewed on 2025-04-09 04:34_

Will review the rest of this later, just looked at the tests so far. Pretty cool the TODOs it does eliminate!

---

_@carljm reviewed on 2025-04-09 04:36_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:162 on 2025-04-09 04:36_

(Trying to write that function also highlighted a bug that currently we don't think `T & ~int` is assignable to `T`, but it should be.)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:162 on 2025-04-09 16:39_

I added some extra tests that exercise the logic I actually care about here (namely, that we try to infer the smallest specialization that satisfies all the constraints).  I went with a function with signature `(T | None) -> T`.  I think there's technically no way to correctly instantiate that signature, so for now I'm accepting the `[invalid-return-type]` error.  Once we support `list[T]`, I can make it return that.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:168 on 2025-04-09 16:42_

Yep! Added

---

_@dcreager reviewed on 2025-04-09 16:42_

---

_Comment by @dcreager on 2025-04-09 17:03_

Putting this back to draft for a bit — now that basic `__init__` support has landed I want to see if I can make this work for class construction too.

---

_Converted to draft by @dcreager on 2025-04-09 17:03_

---

_Comment by @codspeed-hq[bot] on 2025-04-09 20:38_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Finfer-function-calls)

### Merging #17301 will **not alter performance**

<sub>Comparing <code>dcreager/infer-function-calls</code> (523ddcb) with <code>main</code> (5350288)</sub>



### Summary

`✅ 33` untouched benchmarks  





---

_@T-256 reviewed on 2025-04-09 20:47_

---

_Review comment by @T-256 on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:150 on 2025-04-09 20:47_

if revealed type of return value of function most be `Literal["a"] | Literal["b"]`, then it's incorrect type checking, since there is only one type var: `T`.

in other words, it should get `Literal["a"]` only when calling by `two_params("a", "a")`. but when calling `two_params("a", "b")`, both of "a" and "b" are different shape of literal strings and it should show error. but if revealed type became `str`, the call of `two_params("a", "b")` is acceptable.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:150 on 2025-04-09 22:12_

I don't agree. There's nothing special about a union type in a set-theoretic type system. `Literal["a", "b"]` is a well-defined single type, just as much as `str` is, and the single typevar `T` can bind to the union type just as well as it can bind to any other type. 

---

_@carljm reviewed on 2025-04-09 22:12_

---

_@T-256 reviewed on 2025-04-09 22:44_

---

_Review comment by @T-256 on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:150 on 2025-04-09 22:44_

> and the single typevar `T` can bind to the union type just as well as it can bind to any other type.

IIUC, `T` would be `Literal["a", "b"]` when call it by `reveal_type(two_params("a", "b"))`.
It now makes sense to me, thanks for clarification.

What I was thinking is that when first parameter passed (`"a"`), then `T` typevar locks to `Literal["a"]` and by second parameter which is `Literal["b"]` it will invalidate the `T`. Actually, I had rust backgrounding of generics in my mind:
```rs
fn two_params<T>(a:T, b:T) -> T {
    a
}

fn main() {
    two_params("a", 1);  // error[E0308]: mismatched types
}
```

BTW, it's valid in Python generics.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:150 on 2025-04-11 15:12_

> What I was thinking is that when first parameter passed (`"a"`), then `T` typevar locks to `Literal["a"]` and by second parameter which is `Literal["b"]` it will invalidate the `T`.

This is a good way to think about how the implementation works here, and how it will need to grow to support some of the other mdtests that still have TODOs.

Right now, the only "solving" that we do is to see that the param is a typevar, and the argument is "some type", and add to the pending specialization that the typevar maps to the type. But if we've already seen that typevar in a different parameter, instead of replacing the previous type, or requiring the previous and new types to be the same (as you thought might be the case), we merge them together into a union. This is the only "unification" that the implementation in this PR does.

This first example in [this section](https://github.com/astral-sh/ruff/blob/836d02cbbed6f56a78358880aba82d277e4cd833/crates/red_knot_python_semantic/resources/mdtest/generics/classes.md#inferring-generic-class-parameters) is one that this PR doesn't addres, where we'll need to not just blindly union everything together — the type annotation is meant to be an actual restriction that we should enforce.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:3846 on 2025-04-11 15:14_

Callers must now provide the full policy that they want to use, and `NO_INSTANCE_FALLBACK` is not implicitly added. That allows us to use `try_call_dunder_with_policy` (instead of copy/pasting it) down below in `try_call_constructor`, to find and call the `__new__` class method.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:3848 on 2025-04-11 15:15_

Taking in a `mut` reference here let's us use `with_self` down below, which reuses and modifies a `CallArgumentTypes` in place.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:4085 on 2025-04-11 15:16_

This the lookup policy that does _not_ include `NO_INSTANCE_FALLBACK`, where we can now use `try_call_dunder_with_policy` because of the change described above

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:5744 on 2025-04-11 15:26_

One thing that this PR is not yet modeling well is nested generic contexts.  Tests are passing for a generic method inside of a generic class, but I think we might need to actually encode nesting of `GenericContext`s to be able to accurate display e.g. the still-generic method of a specialized generic class.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:114 on 2025-04-11 15:27_

This is redundant with `with_self`, as long as you can reuse the `CallArgumentTypes` instance

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/class.rs`:298 on 2025-04-11 15:28_

This is where we might consider having _all_ class methods inherit the class's generic context, if we want to e.g. support inferring a specialization for a factory method.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/display.rs`:265 on 2025-04-11 15:29_

This isn't actually used in the PR, but I _was_ using it for some `printf` debugging at one point, and it seemed worth keeping.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/generics.rs`:223 on 2025-04-11 15:30_

Here is where (as discussed above) we are unioning together the mappings for a typevar that appears in multiple parameters

---

_@dcreager reviewed on 2025-04-11 15:31_

@carljm, the "identity specialization" worked well, and let me remove the new `InstanceType` enum variant as we discussed.

---

_Marked ready for review by @dcreager on 2025-04-11 15:31_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:150 on 2025-04-12 00:31_

That example will also require type context (bidirectional checking).

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:163 on 2025-04-12 00:51_

This is a valid body with the current signature, but it currently runs into the fact that we don't consider `T & ~None` to be a subtype of `T` (filed https://github.com/astral-sh/ruff/issues/17364 for that):

```suggestion
def union_param[T](x: T | None) -> T:
    if x is None:
        raise ValueError("give me none of that")
    return x
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:160 on 2025-04-12 00:54_

Can/should this be `Never`? (Just musing, no need for it to be more than a TODO in this PR.)

I don't think it's possible to write a correct implementation of `union_param` that returns `None`? Which suggests that if you pass in `None`, the return type should be `Never`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:150 on 2025-04-12 00:57_

I know I said in the first comment here that I wasn't actually saying we should remove all these TODOs asking for wider types... but I kind of think we should? Unless someone wants to argue that we definitely want this change. Having TODOs around suggests to a contributor that a PR to widen all these types would be welcomed, when I'm not sure it would/should be.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4128 on 2025-04-12 01:23_

Should we also have a test showing specialization from `__new__`? And even some tests showing how specialization works in cases where both `__new__` and `__init__` are present?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3860 on 2025-04-12 01:23_

This can totally be a TODO for now, but I wonder if there are cases where we'll need to specialize from `__init__` even if `__new__` exists? Like if `__new__` just has an `*args, **kwargs` signature but `__init__` actually binds some type vars?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:5744 on 2025-04-12 01:24_

Is it worth a TODO comment somewhere to capture this?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:8 on 2025-04-12 01:27_

This doesn't seem to be used in this module?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:298 on 2025-04-12 01:33_

I think this could be a comment in the code, for better access later.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:305 on 2025-04-12 01:38_

I'm a bit confused by this. So we still have the specialized function, and we don't remove that specialization, but we attach the class' generic context to the function? What effect does having the specialization still attached have?

I guess I'm having some trouble following the semantics of the "generic context" field on a function type; maybe this is also related to your comment just above. I wonder if it would be clearer to have a separate dedicated field for this special case of "already-specialized method, but we want call binding to infer a specialization from it"?

I guess a maybe related question is, what about if the class is explicitly specialized already (e.g. `C[int](...)` constructor call)? In that case, it seems like we don't want/need to infer a specialization from the constructor, we just want to use the existing specialization and check the arguments against it. How is that handled currently?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:293 on 2025-04-12 01:47_

Why do we have to implement the special case for `__init__` and `__new__` both here and in `ClassLiteralType::own_class_member`, when this method delegates to `ClassLiteralType::own_class_member`?

---

_@carljm approved on 2025-04-12 01:53_

Fantastic.

---

_@mtshiba reviewed on 2025-04-12 09:40_

---

_Review comment by @mtshiba on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:150 on 2025-04-12 09:40_

Here is an example, which is somewhat artificial, but demonstrates that it is not always good to infer the "minimal" type of function arguments.

```python
from typing import Callable

def two_params[T](x: T, y: T) -> T:
    return x
def curry_two_params[T](x: T) -> Callable[[T], T]:
    return lambda y: two_params(x, y)

f = curry_two_params("a")
reveal_type(f)  # revealed: (Literal["a"], /) -> Literal["a"]  # but this too strict in some cases

# This is OK
reveal_type(two_params("a", "b"))  # revealed: Literal["a", "b"]
# So, this should also be OK
reveal_type(f("b"))  # revealed: Literal["a"]
```

This causes the following error:

```
error: lint:invalid-argument-type: Argument to this function is incorrect
  --> generic.py:14:15
   |
12 | reveal_type(two_params("a", "b"))  # revealed: Literal["a", "b"]
13 | # So, this should also be OK
14 | reveal_type(f("b"))  # revealed: Literal["a"]
   |               ^^^ Expected `Literal["a"]`, found `Literal["b"]`
```

Such an error does not occur in mypy, pyright (because they simply infer the type of `f` as `str -> str`).
Therefore, it may be that red-knot should also infer the type of `curry_two_params("a")` as `str -> str`. Alternatively, it might be possible to infer it as `Literal["a", "b"] -> Literal["a", "b"]` by using bidirectional type checking?

In any case, the current behavior may be inconvenient because users cannot intentionally widen the types of function arguments.

---

_@dcreager reviewed on 2025-04-14 13:22_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:150 on 2025-04-14 13:22_

Ooh, I like this example! I think it's not restricted to the "should we widen literals to the 'real' type" discussion. It would also apply to any parameters where we would infer a union when calling `two_params` directly.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:150 on 2025-04-14 19:16_

Yes, I think this is a very useful example, thank you! But (to rephrase what @dcreager already mentioned), no Python type checker actually supports the general principle that if `two_params(T1, T2)` is OK, therefore `f = curry_two_params(T1); f(T2)` should also be OK. In both mypy and pyright, `two_params("a", 1)` is fine (`T` is solved to `int | str`), but `f = curry_two_params("a"); f(1)` is a type error (`f` is `(str) -> str`).

In other words, there's no difference in principle here, just a difference in semi-arbitrary widening heuristics, that makes the same issue appear at a different level of type granularity.

Perhaps the mypy/pyright approach is the best one available, in practice! But it would certainly be satisfying if we can find a more general answer that doesn't depend on heuristics.

One approach to solve this kind of issue is unification of a constraint system across the entire scope. But this is quite hard to reconcile with flow typing.

---

_@carljm reviewed on 2025-04-14 19:16_

---

_@carljm reviewed on 2025-04-14 19:40_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:150 on 2025-04-14 19:40_

I think this is another form of the same problem:

```py
def f(x: T) -> list[T]:
    return [x]

list_of_what = f(1)
reveal_type(list_of_what)  # mypy/pyright say `list[int]`
list_of_what.append(2)  # mypy/pyright are ok with this
list_of_what.append("foo")  # but not this, which is sort of arbitrary
```

The common thread in the examples is that a type is returned from a generic function in which a type parameter appears in an invariant/contravariant position.

---

_@dcreager reviewed on 2025-04-14 19:57_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:150 on 2025-04-14 19:57_

Is this another place where we'd lean on `OneOf`?  The type of `list_of_what` (and maybe the type of a literal in general?) would be `list[OneOf[Literal[1], int]]`.  i.e. `OneOf` might be shaping up to be the way that we handle "cross-expression" constraints in general.

---

_@carljm reviewed on 2025-04-14 22:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:150 on 2025-04-14 22:22_

I do think it's plausible that we can leverage gradual typing here, though I'm not sure we need `OneOf` in this case, I think "union with `Unknown`" suffices. (Using `OneOf` still requires us to make the arbitrary judgment that you aren't supposed to add strings to this list.) The union `Unknown | Literal[1]` expresses the gradual type "some type at least as large as `Literal[1]` but possibly larger", which has the nice property that if you ever try to use it somewhere you can't use an integer, it'll error -- but it'll allow things other than `Literal[1]` to be assigned to it.

I think at least two things we would need to make this approach work would be narrowing such that if you have a list `l` of type `list[Unknown | Literal[1]]` and you do `l.append("foo")`, we subsequently consider it to be of type `list[Unknown | Literal[1] | Literal["foo"]`.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:150 on 2025-04-15 20:21_

> I know I said in the first comment here that I wasn't actually saying we should remove all these TODOs asking for wider types... but I kind of think we should? Unless someone wants to argue that we definitely want this change. Having TODOs around suggests to a contributor that a PR to widen all these types would be welcomed, when I'm not sure it would/should be.

Removed the TODOs. If we decide to make this change, test failures will tell us all the places that need to be updated.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:5744 on 2025-04-15 20:23_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/class.rs`:298 on 2025-04-15 20:26_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:163 on 2025-04-15 20:36_

Done.  And closed #17364 so that there's no error

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:160 on 2025-04-15 20:36_

Added TODO

---

_@dcreager reviewed on 2025-04-15 20:54_

---

_Converted to draft by @dcreager on 2025-04-15 20:55_

---

_Comment by @dcreager on 2025-04-15 20:55_

Still working on more thorough tests of `__new__` and `__init__`.  Will move it back out of draft when ready for more review

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call.rs`:8 on 2025-04-16 14:51_

It's reexported here so that [this line](https://github.com/astral-sh/ruff/pull/17301/files#diff-42314c006689490bbdfbeeb973de64046b3e069e3d88f67520aeba375f20e655R35) can pull it in from the `call` module instead of from `call::bind`.  I _think_ that's so that we can use `pub(super)` here to limit it to the `types::*` module tree, and not let it leak to the entire crate?  (We'd need a `pub(grandparent)` to do that on its definition in `bind`.)

I honestly don't _love_ that pattern. (Do we really need that level of control over visibility? And if so, would it be better to use more crates to achieve it? If we don't want more crates, is that not an argument that we don't really need that level of control?) But I don't feel _that_ strongly, and it's the pattern currently being used, so I didn't want to conflate an extra thing into this PR.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/class.rs`:305 on 2025-04-16 15:00_

> I'm a bit confused by this. So we still have the specialized function, and we don't remove that specialization, but we attach the class' generic context to the function? What effect does having the specialization still attached have?

This is related to your [question below](https://github.com/astral-sh/ruff/pull/17301#discussion_r2040498114): we shouldn't be doing this special-case lookup here at all, it should only happen down in `ClassLiteralType`, where neither the generic class nor its `__new__` method have been specialized yet.  So I've removed this.

I think the equivalent logic in `ClassLiteralType` (which we keep) is correct, at least for `__new__` methods that are not further specialized.  (For ones that are, we run afoul of my comment above about not handling that nested generic context especially well yet.)  You would have:

```py
class C[T]:
    def __new__(cls, x: T) -> "C"[T]: ...
```

In `ClassLiteralType`, we would looking up `C.__new__`. `C` is not yet specialized, so the method inherits the class's generic context and (via the `with_generic_context` call) becomes `C.__new__[T]`.  Then when we call it, a specialization gets inferred, and that specialization gets applied to the class as well, resulting in e.g. `C.__new__[int]` → `C[int]`.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/class.rs`:293 on 2025-04-16 15:05_

See above; we shouldn't have this logic in both places, only in `ClassLiteralType`, where the generic class has not yet been specialized

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:4128 on 2025-04-16 16:51_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:3860 on 2025-04-16 16:52_

Added these as test cases, and added a `combine` method for specializations that handles this case.  (I had though that the `Option::or` could would handle it, but in the `*args, **kwargs` case, we do still infer a specialization — the default one!)

---

_@dcreager reviewed on 2025-04-16 16:53_

---

_Marked ready for review by @dcreager on 2025-04-16 17:04_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:5744 on 2025-04-16 17:11_

I added a failing test case for this too, showing how we're not correctly modeling e.g. a generic `__init__` method inside of a generic class.

---

_@dcreager reviewed on 2025-04-16 17:11_

---

_Comment by @dcreager on 2025-04-16 19:07_

Going to go ahead and merge this; if anyone sees anything that needs to be addressed as follow-on work, please let me know!

---

_Merged by @dcreager on 2025-04-16 19:07_

---

_Closed by @dcreager on 2025-04-16 19:07_

---

_Branch deleted on 2025-04-16 19:07_

---
