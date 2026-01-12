```yaml
number: 16238
title: "[red-knot] Diagnostics for incorrect `bool` usages"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/type-iterate-bool
created_at: 2025-02-18T18:41:01Z
updated_at: 2025-02-21T18:35:23Z
url: https://github.com/astral-sh/ruff/pull/16238
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Diagnostics for incorrect `bool` usages

---

_@MichaReiser_

## Summary

The diagnostic handling isn't ideal yet but I'd like to have some first feedback on the overall aproach (and maybe you've suggestions on how to improve the diagnostics). 


This PR introduces new `Type::try_iterate` and `Type::try_bool` methods that return a `Result` instead of always assuming that the operation was successful. ~~This PR also renames the existing `call` and `dunder_call` methods to `try_call` and `try_dunder_call` to match this pattern~~ (see https://github.com/astral-sh/ruff/pull/16261)


This PR also introudces the new lint rule `unsupported-bool-conversion` that detects implicit `bool` conversions where the type doesn't implement `__bool__` correctly.

## Performance

~~Codspeed shows a 4% perf regression on the cold benchmark. I suspect that this is due to Red Knot calling `bool` much more frequently than it used to.~~

This is no longer the case, thanks to @sharkdp's performance improvement to `try_bool`

## Test Plan

There's more to do but there are some new tests.


---

_Label `red-knot` added by @MichaReiser on 2025-02-18 18:41_

---

_@MichaReiser reviewed on 2025-02-18 18:42_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:3672 on 2025-02-18 18:42_

Creating a multi-span diagnostic that shows the return type probably requires us to return an entire `binding` (or `CallOutcome`) here.

---

_@MichaReiser reviewed on 2025-02-18 18:42_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call.rs`:15 on 2025-02-18 18:42_

I should revert this

---

_Comment by @codspeed-hq[bot] on 2025-02-18 18:46_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Ftype-iterate-bool)

### Merging #16238 will **not alter performance**

<sub>Comparing <code>micha/type-iterate-bool</code> (6311d81) with <code>main</code> (3aa7ba3)</sub>



### Summary

`✅ 32` untouched benchmarks  





---

_Comment by @MichaReiser on 2025-02-18 19:13_

Huh, the perf regression is rather surprising. 

---

_@MichaReiser reviewed on 2025-02-18 21:00_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1814 on 2025-02-18 21:00_

I suspect that the regression comes from the fact that we no longer short-circuit if any union element inferred `Ambigious`. 

That would suggest that we might want to have a different API so that `Type::bool` knows whether it has to iterate over all types to produce accurate error messages or if it is fine to exit early.

---

_@MichaReiser reviewed on 2025-02-19 16:28_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/call/builtins.md`:9 on 2025-02-19 16:28_

this is more something I noticed and considered worth adding a test. It isn't itself related to making `Type::bool` faliable

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/snapshots/membership_test.md_-_Comparison:_Membership_Test_-_Not-boolable_return_type.snap`:50 on 2025-02-19 16:30_

The error messages here (and in other places where `bool` is called implicitly could be better but I'm not sure how they should look exactly. What makes them not very useful right now is that it's unclear where the `NotBoolable` comes from. 

---

_@MichaReiser reviewed on 2025-02-19 16:30_

---

_@MichaReiser reviewed on 2025-02-19 16:42_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1454 on 2025-02-19 16:42_

The main change here is that this now returns a `Result`. The other changes are  how `Type::Instnace` and `Union` are  handled.

---

_Marked ready for review by @MichaReiser on 2025-02-19 16:54_

---

_Review requested from @carljm by @MichaReiser on 2025-02-19 16:54_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-19 16:54_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-19 16:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:496 on 2025-02-19 17:34_

I don't find the `e.g.` clause very helpful here, and we give detailed examples below

```suggestion
    /// Checks for objects that can't be converted to a boolean but are used in boolean contexts.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:499 on 2025-02-19 17:36_

```suggestion
    /// If an exception is raised when you attempt to evaluate the truthiness of an object,
    /// using the object in a boolean context will fail at runtime.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:512 on 2025-02-19 17:38_

```suggestion
    /// b1, b2 = NotBoolable(), NotBoolable()
    ///
    /// if b1:  # exception raised here
    ///     pass
    ///
    /// b1 and b2  # exception raised here
    /// not b1  # exception raised here
    /// b1 < b2 < b1  # exception raised here
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4898 on 2025-02-19 17:42_

I almost wonder if these methods that don't return a `Result` should be `unsafe`... we probably want to encourage `SAFETY` methods like this next to them if they're used in `infer.rs`!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3212 on 2025-02-19 17:44_

```suggestion
                            "Object of type `{not_boolable}` cannot be converted to a bool because the return type of its `__bool__` method (`{return_type}`) isn't assignable to `bool",
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3203 on 2025-02-19 17:44_

```suggestion
                            "Object of type `{}` cannot be converted to a bool",
```

I also don't love "converted to a bool" -- it could be confusing... with an expression like `a or b`, if `a == "foo"` and `b == ""`, `foo` has its truthiness evaluated (`bool()` is called on it and the result of that expression is inspected by the interpreter in order to evaluate the expression), but the result of the expression is not a boolean value (which is what would, to me, be implied by "converted to a bool"). The result of the expression is `"foo"`.

"Evaluted in a boolean context" might be better than "converted to a bool". That's obviously more verbose, though

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3224 on 2025-02-19 17:53_

```suggestion
                        "Object of type `{}` cannot be converted to a bool because its `__bool__` attribute isn't callable.",
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3234 on 2025-02-19 17:53_

```suggestion
                        "Object of type `{}` cannot be converted to a bool.",
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1731 on 2025-02-19 17:55_

```suggestion
                //   right signature and return a binding error.
```

Does this comment also maybe belong a bit lower down, closer to the `KnownClass::Bool` branch?



---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3093 on 2025-02-19 17:59_

```suggestion
    /// The type isn't iterable because it doesn't implement the new-style or old-style iteration protocol
    ///
    /// The new-style iteration protocol requires a type being iterated over to have an `__iter__`
    /// method that returns something with a `__next__` method. The old-style iteration
    /// protocol requies a type being iterated over to have a `__getitem__` method that accepts
    /// a positive-integer argument.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3712 on 2025-02-19 18:09_

hrm, I think I'd prefer to add a `Truthiness::from_bool_error()` function and use that

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2432 on 2025-02-19 18:10_

this is a very nice API :-)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3398 on 2025-02-19 18:12_

...did they remove it again? :P

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4670 on 2025-02-19 18:14_

```suggestion
                                *ty,
                                range,
                            ).expect("infer_binary_type_comparison should never return None for `CmpOp::Eq`");
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:180 on 2025-02-19 18:20_

I think ideally the code and error message here would be something like

```suggestion
# error: [operator] "Cannot use `in` operator on object of type `WithContains` as its `__contains__` method returns `NotBoolable`, which cannot be evaluated in a boolean context"
10 in WithContains() and True
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:353 on 2025-02-19 18:21_

```suggestion
Python implicitly calls `bool` on the comparison result of preceding elements (but not for the last
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:370 on 2025-02-19 18:22_

Could we also test that we _don't_ emit a diagnostic if there are only two elements?

```suggestion
10 < Comparable() < Comparable()

Comparable() < Comparable()  # fine
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:371 on 2025-02-19 18:23_

```suggestion
a < b < b

a < b  # fine
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/expression/boolean.md`:126 on 2025-02-19 18:24_

really nice error message :-)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:110 on 2025-02-19 18:25_

not sure I understand the TODO comment here?

---

_@AlexWaygood approved on 2025-02-19 18:26_

I have a bunch of nits but the overall approach looks great to me

---

_@MichaReiser reviewed on 2025-02-19 18:31_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:4898 on 2025-02-19 18:31_

I don't think we should use `unsafe` for this. `unsafe` signifies something different. There are also many uses cases where using `bool` is correct and requiring `unsafe` and a SAFETY comment would undo any ergonomics which this method brings. 

I'm not too much concerned about us using the correct method. My main confusion in the past was that there were 3 different methods that I could call on `CallOutcome` without any recommendation on when to use what. There are now only two methods and they have documentation. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:3712 on 2025-02-19 18:31_

I prefer it this way because it is consistent with `CallError`, `IterateError`, `CallDunderError` etc. They all have a method to get the fallback type. 

---

_@MichaReiser reviewed on 2025-02-19 18:31_

---

_@MichaReiser reviewed on 2025-02-19 18:33_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3398 on 2025-02-19 18:33_

Why am I so terrible lately with rebasing/stashing my changes. No, I figured out how to avoid the `Vec::from` but the change got lost. Thanks for catching this.

---

_@MichaReiser reviewed on 2025-02-19 18:35_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:180 on 2025-02-19 18:35_

Slightly too long for my taste but certainly more helpful. Thank you

---

_@MichaReiser reviewed on 2025-02-19 18:36_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:110 on 2025-02-19 18:36_

Oh, this is outdated. That was a comment from before you helped me fix the `bool` implementation.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:180 on 2025-02-19 18:39_

> Slightly too long for my taste

For me too... but once we have the ability to add notes to diagnostics, I'd imagine it could become something like

```
error: [operator] cannot use `in` operator on object of type `WithContains`
    note: This is because the `in` operator implicitly calls `WithContains.__contains__`, but `WithContains.__contains__` is invalidly defined
    note: `WithContains.__contains__` is invalidly defined because it returns an instance of `NotBoolable`, which cannot be evaluated in a boolean context
    note: `NotBoolable` cannot be evaluated in a boolean context because its `__bool__` attribute is not callable
```


---

_@AlexWaygood reviewed on 2025-02-19 18:39_

---

_@AlexWaygood reviewed on 2025-02-19 18:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3712 on 2025-02-19 18:43_

Yeah, I see the consistency argument... I still feel like I'd prefer something inconsistent here than having a method that "pretends to be an instance method" but doesn't actually use `self`. That said, I don't feel very strongly.

Could we at least add a comment about why we're deliberately leaving `self` unused here, if we leave it as-is, though?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:180 on 2025-02-19 22:40_

I like @AlexWaygood 's suggested message, except that even in the absence of detailed notes about why the `__bool__` method is invalid, I'd rather say "has an invalid `__bool__` method" rather than "cannot be evaluated in a boolean context".

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:354 on 2025-02-19 22:41_

```suggestion
element) of a chained comparison.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:349 on 2025-02-19 22:42_

```suggestion
## Chained comparisons with not-boolable types
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/expression/boolean.md`:126 on 2025-02-19 22:45_

It is, but it's unfortunate that we don't get this helpful message in lots of other cases above where it is equally true (e.g. with `__bool__ = 3`), I presume because of the union-with-Unknown

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:510 on 2025-02-19 22:49_

I know we discussed `not-boolable` as an option, but after thinking it through more I would much prefer `invalid-bool-method` because it puts the focus on what is actually wrong (we've encountered an invalid definition of `__bool__`, which is an error) rather than suggesting that there is some legitimate category of "not-boolable" objects which can be modeled consistently and soundly in the type system (there isn't).

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:511 on 2025-02-19 22:50_

```suggestion
        summary: "detects calls to invalid definitions of the `__bool__` method",
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:354 on 2025-02-19 22:54_

```suggestion
## Operations involving types with invalid `__bool__` method

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1504 on 2025-02-19 23:16_

The descriptors PR already does this and will conflict here, but so it goes...

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1539 on 2025-02-19 23:33_

We are losing a lot of diagnostic message usefulness in the tests due to union-with-Unknown and the fact that we don't dig into unions here; let's at least add a TODO for that.
```suggestion
                                // TODO: better diagnostics, in particular for `Unknown | T` which comes up often
                                | CallError::Union { .. },
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2287 on 2025-02-19 23:39_

```suggestion
    /// Returns an `Err` if the dunder method can't be called, or the given arguments are not valid.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1809 on 2025-02-19 23:48_

The `?` here and below doesn't seem right if `allow_short_circuit` is false. It will mean in a union case we emit a specific diagnostic related to just one arbitrary element of the union with bad `__bool__`, even if there are multiple.

We don't necessarily need to fix that in this PR, but I think we should at least add a test for that case with a TODO on the incomplete diagnostic.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3624 on 2025-02-19 23:53_

If I'm reading the current implementation right, "type is a union and not all variants support `__bool__`" will not usually result in `BoolError::Other`, it will result in a specific error relating to `__bool__` of some arbitrary one of the union elements that has a bad `__bool__`.

`Other` mostly seems to occur in cases where a type has a `__bool__` method which itself resolves to a union (usually a union with `Unknown` because it's not declared).

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3212 on 2025-02-19 23:56_

```suggestion
                            "Object of type `{not_boolable}` has an invalid `__bool__` method; its return type (`{return_type}`) isn't assignable to `bool`",
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3648 on 2025-02-19 23:57_

Would it be worth separating `ArgumentsError` from `ReturnTypeError` variants so we don't have to repeat this assignability check when rendering diagnostics?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3203 on 2025-02-19 23:59_

I would prefer "has an invalid `__bool__` method" over either of "cannot be converted to a bool" or "cannot be evaluated in a boolean context." It is less vague and points more directly at the real problem.

Also, in this case don't we know the problem is argument binding, so we could at least say that? Of course in future we'll want to give more details, I think.

```suggestion
                            "Object of type `{}` has an invalid `__bool__` method; it does not accept the correct arguments.",
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3234 on 2025-02-20 00:01_

```suggestion
                        "Object of type `{}` has an invalid `__bool__` and cannot be converted to a bool.",
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2377 on 2025-02-20 00:07_

I think this isn't quite right, and there should be a TODO here (though a low-priority one). If the `for` statement is not a definition, because the target is not a `Name` (Python allows e.g. `for x.attr in ...` and `for x[i] in ...`, strange as they look), we will never call `infer_for_statement_definition`, and we should ideally check the iterable for iterability and emit a diagnostic if it's not iterable.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3206 on 2025-02-20 00:10_

This is a common pattern and it would be nice if it could be a bit less verbose, but I guess there's no obvious way to do that short of a custom `Result` like type.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4173 on 2025-02-20 00:16_

I think this logic and the match below could be adjusted so we can avoid the unnecessary `try_bool` call on the last comparison entirely, instead of just skipping the diagnostic from it. Skipping the diagnostic is a tell that we are calling `try_bool` when we shouldn't need to because runtime doesn't.

If `is_last`, we always return `ty` and we don't actually care about `truthiness`.

I haven't checked the surrounding logic to see whether we enter this path even on single (non chained) comparisons, but if we do, fixing this could result in significantly fewer useless `try_bool` calls in real codebases.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4898 on 2025-02-20 00:33_

This comment is not right, we are mimicking a runtime `bool` call here:

```
>>> class A:
...     def __eq__(self, other):
...         return B()
...
>>> class B:
...     def __bool__(self):
...         raise RuntimeError("boom")
...
>>> (A(),) == (A(),)
Traceback (most recent call last):
  File "<python-input-16>", line 1, in <module>
    (A(),) == (A(),)
  File "<python-input-13>", line 3, in __bool__
    raise RuntimeError("boom")
RuntimeError: boom
```

Because `__eq__` is allowed to return any object, Python at runtime has to check that object for truthiness, which could fail; just like we have to do here.

(Not saying we have to fix this in this PR, just that the comment here should be a TODO instead of an incorrect rationale.)

---

_@carljm approved on 2025-02-20 00:33_

This is great!! I left a lot of comments, but many of them are wording bikeshedding :)

---

_@AlexWaygood reviewed on 2025-02-20 07:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3203 on 2025-02-20 07:47_

> I would prefer "has an invalid `__bool__` method" over either of "cannot be converted to a bool" or "cannot be evaluated in a boolean context." It is less vague and points more directly at the real problem.

But that wording implies that the only way for the user to resolve the diagnostic is to go and fix the invalid `__bool__` definition. It's not possible for the user to do that if it comes from some third-party library like numpy or pandas. I would say that in the majority of cases, the fix for the diagnostic is to stop using objects of that type in a boolean context rather than to go and fix the invalid `__bool__` method — and that's partly why we flag the usages of the method rather than the definition of the method with our diagnostics

---

_@AlexWaygood reviewed on 2025-02-20 07:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:180 on 2025-02-20 07:54_

See my comment at https://github.com/astral-sh/ruff/pull/16238#discussion_r1963014361. I think it's pretty important that we foreground *what's wrong with the specific code we're highlighting* in our diagnostic messages, and *what the user needs to do to resolve the error*. The invalid method definition is the underlying cause here, but the specific thing that's incorrect in the code we're highlighting with the diagnostic range is that the object with the invalid method is being used in a boolean context, and the specific way for the user to resolve that error is for them to stop doing that. Fixing the invalid method definition may be out of their power if it comes from a third-party library.

---

_@MichaReiser reviewed on 2025-02-20 09:05_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1809 on 2025-02-20 09:05_

I'd argue it isn't incorrect, but the error is less helpful. `allow_short_circuit` is to return early on an ambiguous result (maybe I should rename the variable) but that could mean that the method returns `Ok` for a union even if one variant can't be booled.

I agree, we should have a todo/test

---

_@MichaReiser reviewed on 2025-02-20 09:08_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3206 on 2025-02-20 09:08_

I had `fallback_element_type_with_diagnostic` but it just felt sooo long. I don't mind the repetitiveness here. It's simple and more flexible, e.g. you don't have to call `fallback_element_type` if you only want to report a diagnostic. 

Overall, I don't think we should spend much time on this: While repetitive, the call sites are very limited.

---

_@MichaReiser reviewed on 2025-02-20 09:09_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:4898 on 2025-02-20 09:09_

Nice catch. I tried to come up with a code example where we have to emit a diagnostic here but wasn't able to. Let me revisit this.

---

_@AlexWaygood reviewed on 2025-02-20 09:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:180 on 2025-02-20 09:26_

To be clear: I don't think my disagreement with Carl should block this PR being merged. We can iterate on the error message and/or continue discussing this in an issue or on Discord.

---

_@MichaReiser reviewed on 2025-02-20 10:49_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/expression/boolean.md`:126 on 2025-02-20 10:49_

Yes, the union with `Unknown` messes everything up. 

---

_@MichaReiser reviewed on 2025-02-20 15:02_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:510 on 2025-02-20 15:02_

I don't like the `invalid-bool-method` too much because I'd expect this diagnostic to be raised on the definition side and not where `__bool__` is used. 

For example, the following suppression comments feels out of place:

```
if NotBoolable(): # red-knot: ignore[invalid-bool-method]
```

I agree that there's a concern with `Boolable` not being a concept in typeshed.

How about `bool-object-with-invalid-bool-method` or `call-invalid-bool-method`

---

_@MichaReiser reviewed on 2025-02-20 15:15_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:510 on 2025-02-20 15:15_

I think this also comes back to our discussion here https://github.com/astral-sh/ruff/pull/16238#discussion_r1963014361 and I agree with @AlexWaygood that the most common fix is to not use this object in a bool context. Tricky

---

_@AlexWaygood reviewed on 2025-02-20 15:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:510 on 2025-02-20 15:19_

I liked Carl's earlier suggestion on Discord of `invalid-boolean-conversion` for the error code (or I guess `invalid-bool-conversion` is a bit shorter?)

---

_@MichaReiser reviewed on 2025-02-20 18:28_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:512 on 2025-02-20 18:28_

Thanks. I didn't use the unpacking syntax to keep the example simple.

---

_@MichaReiser reviewed on 2025-02-20 18:29_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:3203 on 2025-02-20 18:29_

I went with *Boolean conversion is unsupported for `{type}`* in the spirit of our unary/binary etc messages

---

_@MichaReiser reviewed on 2025-02-20 18:31_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:3093 on 2025-02-20 18:31_

Nice! 

---

_@MichaReiser reviewed on 2025-02-20 18:35_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:4670 on 2025-02-20 18:35_

Hmm thanks. `infer` and `types.rs` are a bit much for Rustfmt. 

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3568 on 2025-02-20 19:24_

```suggestion
    /// The new-style iteration protocol requires a type being iterated over to have an `__iter__`
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3570 on 2025-02-20 19:25_

```suggestion
    /// protocol requires a type being iterated over to have a `__getitem__` method that accepts
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/builtins.md`:12 on 2025-02-20 19:26_

```suggestion
# TODO: We should emit a `unsupported-bool-conversion` error here because the argument doesn't implement `__bool__` correctly.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:182 on 2025-02-20 19:29_

(I still think that `operator` might actually be a better error code here specifically -- the invalid `__bool__` definition here means that you can't use the `in` operator with the `WithContains` type)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:1 on 2025-02-20 19:30_

I agree with Carl's suggested changes in this file

---

_@AlexWaygood approved on 2025-02-20 19:32_

---

_Converted to draft by @MichaReiser on 2025-02-21 07:04_

---

_Comment by @MichaReiser on 2025-02-21 09:36_

Putting this back to draft or Alex keeps reviewing faster than I can address the comments ;)

---

_@MichaReiser reviewed on 2025-02-21 10:03_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:510 on 2025-02-21 10:03_

I'll go with `unsupported-bool-conversion` for now. We had an extensive discussion on Discord and the TLDR is: We haven't found the best name yet but we can iterate on the exact naming in the future.

---

_@MichaReiser reviewed on 2025-02-21 11:09_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:182 on 2025-02-21 11:09_

Closing as duplicate

---

_@MichaReiser reviewed on 2025-02-21 14:04_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1539 on 2025-02-21 14:04_

I added some specific handling for unions where exactly one variant fails. 

---

_@MichaReiser reviewed on 2025-02-21 14:06_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:3624 on 2025-02-21 14:06_

That's correct. I updated the comment.

---

_@MichaReiser reviewed on 2025-02-21 14:13_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:180 on 2025-02-21 14:13_

I agree that this would be much better. 

I'm leaning towards not implementing this now. A) This feels like a task on its own because it isn't sufficient to prepend a message to every diagnostic; making it sound natural requires repeating much of the diagnostic code, and b) ideally, we'd have support from the diagnostic system to do this correctly.

I expect that the code for doing this right might look fairly different depending on what the diagnostic system support. 

CC: @BurntSushi for another use case where we need some form of chaining/inspection/enriching of diagnostics. 

 



---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:625 on 2025-02-21 14:28_

Not that it really matters much, but one reason why it is quite nice to stick with only using ```` ```py ```` or ```` ```pyi ```` for Python code blocks is that currently all the tags we use exactly map to the file extension that the embedded file will be saved as in the `MemoryFileSystem`. If you use ```` ```py ````, it saves it with the `.py` file extension; if you use ```` ```pyi ````, it saves it with the `.pyi` file extension; if you use ```` ```toml ````, it saves it as a `.toml` file... but ```` ```python ```` here isn't going to cause mdtest to save the snippet with a `.python` extension.

---

_@AlexWaygood reviewed on 2025-02-21 14:28_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:625 on 2025-02-21 14:32_

Stop looking at my PR, it's in draft ;)

I was told yesterday that this wouldn't be hard to change but I precisely haven't changed it before because I was afraid of the push back and discussions it involves. 

I can revert the change here and open a separate PR but I'm very used to writing `python` and `RustRover` suggestions `python` but never `py`, making it very annoying because tests always fail.


I'd find this a quality-of-life improvement. I agree, it leads to less consistency but it's common that markdown editors/renderers support both: names and extension. So I don't think this would come at too much of a suprise.

---

_@MichaReiser reviewed on 2025-02-21 14:32_

---

_Comment by @MichaReiser on 2025-02-21 14:33_

Okay, I think I addressed all feedback with the exception of special casing `in`. I decided to defer it because I suspect it's a fair bit of work and the right approach might depend on the functionality of our diagnostic system. 

I did improve the error messages for unions. 

---

_Marked ready for review by @MichaReiser on 2025-02-21 14:34_

---

_@AlexWaygood reviewed on 2025-02-21 14:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:625 on 2025-02-21 14:35_

> Stop looking at my PR, it's in draft ;)

heh, you're one to talk ;) but, sorry!

I honestly don't care that much about this, so if it's a big quality of life improvement for you then I'm fine for it to go in :-)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:354 on 2025-02-21 14:37_

```suggestion
## Operations involving types with invalid `__bool__` methods
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/builtins.md`:12 on 2025-02-21 14:37_

```suggestion
# TODO: We should emit an `unsupported-bool-conversion` error here because the argument doesn't implement `__bool__` correctly.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:193 on 2025-02-21 14:40_

I don't think we need the `and` and `or` here, do we? Just `10 in WithContains()` fails at runtime and should be enough to cause us to emit the error:

```suggestion
10 in WithContains()
# error: [unsupported-bool-conversion]
10 not in WithContains()
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:179 on 2025-02-21 14:52_

````suggestion
`in` and `not in` operations will fail at runtime if the object on the right-hand side of the
operation has a `__contains__` method that returns a type which is not convertible to `bool`.
This is because of the way these operations are handled by the Python interpreter at runtime. If we
assume that `y` is an object that has a  `__contains__` method, the Python expression `x in y`
desugars to a `contains(y, x)` call, where `contains` looks something like this:

```ignore
def contains(y, x):
    return bool(type(y).__contains__(y, x))
```

where the `bool()` conversion itself implicitly calls `__bool__` under the hood.

TODO: Ideally our message would explain to the user what's wrong here. E.g,
````

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:376 on 2025-02-21 15:00_

```suggestion
## Tuple equality with elements that incorrectly implement `__bool__`

Python does not generally attempt to coerce the result of `==` and `!=` operations between two
arbitrary objects to a `bool`, but a comparison of tuples will fail if the result of comparing any
pair of elements at equivalent positions cannot be converted to a `bool`:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/conditional/if_expression.md`:45 on 2025-02-21 15:03_

this should be "its" rather than "it's" because it's not a shortening of "it is"

```suggestion
# error: [unsupported-bool-conversion] "Boolean conversion is unsupported for type `NotBoolable`; its `__bool__` method isn't callable"
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:338 on 2025-02-21 15:06_

```suggestion
## Chained comparisons with elements that incorrectly implement `__bool_`
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:341 on 2025-02-21 15:13_

````suggestion

For an operation `A() < A()` to succeed at runtime, the `A.__lt__` method does not necessarily need
to return an object that is convertible to a `bool`. However, the return type _does_ need to be
convertible to a  `bool` for the operation `A() < A() < A()` (a *chained* comparison) to succeed.
This is because `A() < A() < A()` desugars to something like this, which involves several implicit
conversions to `bool`:

```ignore
def compute_chained_comparison():
		a1 = A()
		a2 = A()
		first_comparison = a1 < a2
		return first_comparison and (a2 < A())
```

````

---

_@AlexWaygood approved on 2025-02-21 15:14_

---

_@T-256 reviewed on 2025-02-21 15:26_

---

_Review comment by @T-256 on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:338 on 2025-02-21 15:26_

```suggestion
## Chained comparisons with elements that incorrectly implement `__bool__`
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/conditional/if_expression.md`:45 on 2025-02-21 18:08_

uh, this will be annoying

---

_@MichaReiser reviewed on 2025-02-21 18:08_

---

_@MichaReiser reviewed on 2025-02-21 18:10_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:341 on 2025-02-21 18:10_

I do like the examples but it makes me wonder if this is really the level we're aiming for or if re-writing the python specification is a bit beyond our immediate goal. But I do appreciate it and I do learn a lot from them.

---

_@MichaReiser reviewed on 2025-02-21 18:10_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:341 on 2025-02-21 18:10_

And I can see, you're a fan of my `ignore` file type hehe

---

_Comment by @MichaReiser on 2025-02-21 18:13_

I'll merge this before we get this to over 100 comments ;) 97 is enough

Thank you both for your excellent reviews.

---

_Merged by @MichaReiser on 2025-02-21 18:26_

---

_Closed by @MichaReiser on 2025-02-21 18:26_

---

_Branch deleted on 2025-02-21 18:26_

---

_@AlexWaygood reviewed on 2025-02-21 18:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:341 on 2025-02-21 18:35_

> And I can see, you're a fan of my `ignore` file type hehe

haha yes, it's a great idea!

---
