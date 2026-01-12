```yaml
number: 13827
title: "[red-knot] Infer unary not operation for instances"
type: pull_request
state: merged
author: Glyphack
labels:
  - ty
assignees: []
merged: true
base: main
head: class-instance-bool
created_at: 2024-10-19T21:46:47Z
updated_at: 2024-11-14T05:44:10Z
url: https://github.com/astral-sh/ruff/pull/13827
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] Infer unary not operation for instances

---

_@Glyphack_

My attempt at resolving not op for instances.

The type is true unless one of the following happens:

1. We call the `__bool__` and if it is false then the result is false.

## Test Plan

I added a new test case with some examples from these resources:

- https://docs.python.org/3/library/stdtypes.html#truth-value-testing
- <https://docs.python.org/3/reference/datamodel.html#object.__len__>
- <https://docs.python.org/3/reference/datamodel.html#object.__bool__>


---

_Renamed from "[red-knot] WIP Handle class and instance cases for bool value" to "[red-knot] Handle class and instance cases for bool value" by @Glyphack on 2024-10-20 13:02_

---

_Comment by @github-actions[bot] on 2024-10-20 13:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@Glyphack reviewed on 2024-10-20 13:23_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/resources/mdtest/unary/instance.md`:62 on 2024-10-20 13:23_

Because everything is considered to be True in conditions unless specified by the __bool__ or __len__ method this will be false. The previous behavior was to say this is `bool`.

---

_@Glyphack reviewed on 2024-10-20 13:23_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:2837 on 2024-10-20 13:23_

Moving this code to https://github.com/astral-sh/ruff/pull/13827/files#diff-42314c006689490bbdfbeeb973de64046b3e069e3d88f67520aeba375f20e655R757 will cause stack overflow.

---

_Renamed from "[red-knot] Handle class and instance cases for bool value" to "[red-knot] Infer bool for class literal and instances" by @Glyphack on 2024-11-09 18:29_

---

_@Glyphack reviewed on 2024-11-10 17:43_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:2838 on 2024-11-10 17:43_

Should I refactor the bool method so we can call `return_ty_result` there and move this piece of code there?

---

_Renamed from "[red-knot] Infer bool for class literal and instances" to "[red-knot] Infer bool for instances" by @Glyphack on 2024-11-10 18:21_

---

_@Glyphack reviewed on 2024-11-10 18:30_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:2862 on 2024-11-10 18:30_

same as https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAySMApiAIYA2AUNQMaXkDOTUAggFzVQ9QAmJYFAD6wgEZgwlUQAomJSsACUAWgB8mFDC69dUECRgBXECigAGWgYBuJKsPgISMlGHxsZSpUA

---

_@Glyphack reviewed on 2024-11-10 18:32_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:2899 on 2024-11-10 18:32_

Pyright is okay with this:
https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAySMApiAIYA2AUNQMaXkDOTUAggFzVQ9QAmJYFAD6wyiRSiAFExKVgASgC0APijBKYcjC689UECRgBXECigAGAHQXahgG4kqw%2BAhJSUYfGykKFQA

But python itself is not:
```
    not A()
TypeError: 'float' object cannot be interpreted as an integer
```

So I decided to add this.

---

_Renamed from "[red-knot] Infer bool for instances" to "[red-knot] Infer unary not operation for instances" by @Glyphack on 2024-11-10 18:34_

---

_@InSyncWithFoo reviewed on 2024-11-11 12:34_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/unary/instance.md`:114 on 2024-11-11 12:34_

Consider adding these two weird testcases:

```python
class LenReturnsTrue:
	def __len__(self) -> Literal[True]: ...

# revealed: Literal[False]
reveal_type(not LenReturnsTrue())
```

```python
class LenReturnsFalse:
	def __len__(self) -> Literal[False]: ...

# revealed: Literal[True]
reveal_type(not LenReturnsFalse())
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unary/instance.md`:62 on 2024-11-11 18:24_

Because it is permitted for a subclass to add a `__bool__` method when the base class doesn't have one, and `Type::Instance` types include subclasses, we cannot safely rely on this behavior. (When we do find a `__bool__` method, we can rely on this because it would be an invalid override for a subclass to override `__bool__` with a return type that isn't a subtype of the base class' return type.)

So we need to infer `bool` here.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unary/instance.md`:77 on 2024-11-11 18:28_

Because a subclass could add a `__bool__` method, and the `__bool__` method takes priority over `__len__` for determining boolean value of an instance, I think we cannot conclude anything from the `__len__` method, and we need to just infer `bool` in both of these cases.

If a class has `__len__` but no `__bool__`, we cannot trust `__len__` because a subclass could add a `__bool__`, which would then take precedence. If a class has both `__bool__` and `__len__`, then `__len__` is always irrelevant, `__bool__` determines our answer.

I think this means we can (and must) totally ignore `__len__` when it comes to determining boolean value of a `Type::Instance`, which should simplify the implementation.

(I realize our TODO comment mentioned `__len__`, which just means we didn't think through this carefully enough when we wrote that comment :/ sorry that comment led to extra work.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unary/instance.md`:96 on 2024-11-11 18:30_

This looks wrong. `__bool__` takes priority over `__len__`, and `WithBothLenAndBool2` has a `__bool__` returning `Literal[True]`, so this must be `Literal[False]`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unary/instance.md`:100 on 2024-11-11 18:32_

Ideally I think we would raise a diagnostic _here_ for the wrong return type for `__bool__`, so the error is caught at the class definition, rather than just in the usage. (Doesn't need to be done in this PR, but let's at least add a TODO comment here.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unary/instance.md`:104 on 2024-11-11 18:33_

And I think here we don't need the diagnostic, we can just infer `bool` if the return type of `__bool__` isn't valid.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unary/instance.md`:113 on 2024-11-11 18:33_

This test can probably just be eliminated, since as I mentioned above I don't think `__len__` can be considered at all.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unary/instance.md`:114 on 2024-11-11 18:34_

These would be good test cases for another PR, but I don't think `__len__` can actually be considered at all as part of type inference for `not x`, as mentioned above

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1166 on 2024-11-11 18:34_

I don't see any need to make this change in this PR?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2838 on 2024-11-11 18:55_

I think with the adjustments the semantics that I outlined above in comments on the tests, this becomes much simpler and at least some of these problems disappear.

1) We don't need to emit diagnostics here, because invalid `__bool__` and `__len__` methods should cause diagnostics in the class definition, not here.

2) I think this case doesn't even require any special logic for `Type::Instance` here; we should just call `.bool()` on the type and negate it.

3) Within `Type::bool`, in the case for `Type::Instance`, I think we should be able to do `instance_ty.call_dunder("__bool__")` and check the return value to see if it is `Literal[True]` or `Literal[False]` -- for anything else, we return `Truthiness::Ambiguous`.

The only point I'm unsure of is the cycle you mentioned. It seems to me that once we simplify this code, the cycle should not be possible; `Type::call` only calls `Type::bool` if the thing being called is the `bool` builtin itself, and the `__bool__` method on a type should generally not be `builtins.bool`. It would be worth adding a test for the very weird edge case where someone does `__bool__ = bool` on a type; maybe we do need to add a special case to avoid panic here?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2862 on 2024-11-11 18:56_

Yes, I see the pyright precedent, but I think pyright is wrong (or at least sub-optimal) in issuing this diagnostic at the call site rather than at the definition of the class.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2899 on 2024-11-11 18:58_

I agree that we should have this diagnostic, but I think it should not happen in this PR for two different reasons:

1) I don't think this PR actually should involve `__len__` at all.
2) I think the diagnostic should be on the class definition, not at the usage site.

---

_@carljm reviewed on 2024-11-11 18:59_

Thanks for working on this! Left a few comments. The good news is I think the comments should make the implementation much simpler.

---

_@Glyphack reviewed on 2024-11-13 16:55_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:2838 on 2024-11-13 16:55_

With your suggestion the cycle is gone. I initially was calling `bool` on the return type of the `__bool__` and that was the reason of cycle but now I am checking the output for `Literal[False]` and `Literal[True]`.

The overflow happens for `__bool__ = bool` So I'm checking the result of `member`. Is it a huge problem if we call this twice? since call_dunder also calls this but I guessed it's better to check it in the bool function.

---

_Marked ready for review by @Glyphack on 2024-11-13 22:28_

---

_Review requested from @MichaReiser by @Glyphack on 2024-11-13 22:28_

---

_Review requested from @AlexWaygood by @Glyphack on 2024-11-13 22:28_

---

_Review requested from @sharkdp by @Glyphack on 2024-11-13 22:28_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unary/instance.md`:42 on 2024-11-13 22:34_

```suggestion

At runtime, the `__len__` method is a fallback for `__bool__`, but we can't make use of that.
If we have a class that defines `__len__` but not `__bool__`, it is possible that any subclass
could add a `__bool__` method that would invalidate whatever conclusion we drew from `__len__`.
So classes without a `__bool__` method, with or without `__len__`, must be inferred as unknown
truthiness.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unary/instance.md`:59 on 2024-11-13 22:35_

```suggestion

# We don't get into a cycle if someone sets their `__bool__` method to the `bool` builtin:
class BoolIsBool:
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unary/instance.md`:65 on 2024-11-13 22:36_

```suggestion
# At runtime, no `__bool__` and no `__len__` means truthy, but we can't rely on that, because
# a subclass could add a `__bool__` method.
class NoBoolMethod: ...
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unary/instance.md`:70 on 2024-11-13 22:37_

```suggestion
# And we can't rely on `__len__` for the same reason: a subclass could add `__bool__`.
class LenZero:
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1183 on 2024-11-13 22:40_

```suggestion
                    // We only check the `__bool__` method for truth testing, even though at
                    // runtime there is a fallback to `__len__`, since `__bool__` takes precedence
                    // and a subclass could add a `__bool__` method.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1225 on 2024-11-13 22:45_

I wonder if we could do this inside `call_dunder` instead, so as to avoid the duplicated `to_meta_type...member` calls, which `call_dunder` will end up doing again.

---

_@carljm approved on 2024-11-13 22:46_

Looks great, thank you!!

Only minor wording nits and one other comment; I'll just apply these and look into the `call_dunder` question briefly, and then land this.

---

_@carljm reviewed on 2024-11-13 23:03_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1225 on 2024-11-13 23:03_

After looking at it, I decided that's no better; this special case is a bit ugly no matter where it goes, and this is probably the best place for it.

I did rework the condition slightly to ensure that we really do have the `bool` built-in and not some random other class named `bool`.

---

_@carljm reviewed on 2024-11-13 23:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2838 on 2024-11-13 23:08_

Yes after looking at it, I think you're right that `Type::bool` is the best place to handle this.

---

_Comment by @Glyphack on 2024-11-13 23:15_

Thanks for helping me why my initial approach was wrong. I will try to make it less work for you next time.

---

_Comment by @carljm on 2024-11-13 23:27_

> Thanks for helping me why my initial approach was wrong. I will try to make it less work for you next time.

No problem, thank you for the contributions!!

---

_Merged by @carljm on 2024-11-13 23:31_

---

_Closed by @carljm on 2024-11-13 23:31_

---

_Comment by @carljm on 2024-11-13 23:38_

Also, to be clear, your initial approach was wrong mostly because we had a TODO comment in the source code specifically suggesting the wrong approach 😆

---

_Branch deleted on 2024-11-13 23:40_

---

_Label `red-knot` added by @carljm on 2024-11-14 05:44_

---
