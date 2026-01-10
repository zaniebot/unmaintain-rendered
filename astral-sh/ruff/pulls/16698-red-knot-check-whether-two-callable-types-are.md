```yaml
number: 16698
title: "[red-knot] Check whether two callable types are equivalent"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/callable-is-equivalent
created_at: 2025-03-13T05:27:23Z
updated_at: 2025-05-07T15:21:03Z
url: https://github.com/astral-sh/ruff/pull/16698
synced_at: 2026-01-10T18:57:02Z
```

# [red-knot] Check whether two callable types are equivalent

---

_Pull request opened by @dhruvmanila on 2025-03-13 05:27_

## Summary

This PR checks whether two callable types are equivalent or not.

This is required because for an equivalence relationship, the default value does not necessarily need to be the same but if the parameter in one of the callable has a default value then the corresponding parameter in the other callable should also have a default value. This is the main reason a manual implementation is required.

And, as per https://typing.python.org/en/latest/spec/callables.html#id4, the default _type_ doesn't participate in a subtype relationship, only the optionality (required or not) participates. This means that the following two callable types are equivalent:

```py
def f1(a: int = 1) -> None: ...
def f2(a: int = 2) -> None: ...
```

Additionally, the name of positional-only, variadic and keyword-variadic are not required to be the same for an equivalence relation.

A potential solution to avoid the manual implementation would be to only store whether a parameter has a default value or not but the type is currently required to check for assignability.

## Test plan

Add tests for callable types in `is_equivalent_to.md`


---

_Label `red-knot` added by @dhruvmanila on 2025-03-13 05:27_

---

_Comment by @github-actions[bot] on 2025-03-13 05:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Renamed from "[red-knot] Add tests for callable equivalence" to "[red-knot] Check whether two callable types are equivalent" by @dhruvmanila on 2025-03-19 10:38_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:4620 on 2025-03-19 10:42_

This is required because we want to avoid checking the default type itself for equivalence.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:4585 on 2025-03-19 10:43_

This is the main reason a manual implementation is required.

---

_@dhruvmanila reviewed on 2025-03-19 10:43_

---

_Marked ready for review by @dhruvmanila on 2025-03-19 10:48_

---

_Review requested from @carljm by @dhruvmanila on 2025-03-19 10:48_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-03-19 10:48_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-03-19 10:48_

---

_Review requested from @dcreager by @dhruvmanila on 2025-03-19 10:48_

---

_@AlexWaygood reviewed on 2025-03-19 11:34_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:127 on 2025-03-19 11:34_

this makes me wonder whether we should be preserving the default _value_ of the parameter at all when inferring the signature of a function (as opposed to just whether it _has_ a default). For a `def` function or a `lambda`, we obviously need to check whether the inferred type of the default value is assignable to the parameter annotation (if there is one). But is there a reason we need to keep track of the default value as part of the signature after that?

It seems like it could simplify the implementation you have here quite a bit if our `Signature` did not keep track of the precise default value of parameters, and only kept track of whether each parameter has a default

---

_@dhruvmanila reviewed on 2025-03-19 11:40_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:127 on 2025-03-19 11:40_

Yeah, that's one of the potential solution that I've mentioned in the PR description. I'll need to check what the fallout of that change would be but I agree that it would be a simpler solution.

---

_@dcreager reviewed on 2025-03-19 13:52_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:127 on 2025-03-19 13:52_

We'll need to store that somewhere, since at a call site we need to know what value to use if no argument is provided for that parameter.

Or I guess — maybe we don't, since if no argument was matched to the parameter, there's no argument type to verify is assignable to that default value type?

I'm wondering if generics would be an issue. _[tests something...]_  Yes, [here we go](https://pyright-play.net/?strict=true&code=CYUwZgBGDaAqC6AKAHgLgrCBeCAWAlBALQB8EANgJYDOALnPKgFASsQBOItAruwHYRoyeEyacAbiACG5APq0AngAcQiMIgCMAdnz4xISTPnLV6gER12lPgHMzu-YbmKVaxA6A):

```python
def f[T](x: T = 4) -> list[T]:
    return [x]

reveal_type(f(17))  # revealed: list[int]
reveal_type(f("string"))  # revealed: list[str]
reveal_type(f())  # revealed: list[int]
```

We'll need to keep around the default `Literal[4]` / `int` type to be able to solve the constraint system for when we instantiate that generic function in the `f()` call.

---

_@AlexWaygood reviewed on 2025-03-19 14:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:127 on 2025-03-19 14:00_

> ```python
> def f[T](x: T = 4) -> list[T]:
>     return [x]
> 
> reveal_type(f(17))  # revealed: list[int]
> reveal_type(f("string"))  # revealed: list[str]
> reveal_type(f())  # revealed: list[int]
> ```

Pyright accepts that function but mypy rejects it. I'm not sure whether using a TypeVar for a parameter with a default value like that is explicitly allowed by the spec or not.

If I were writing a stub for this function in typeshed, I would use overloads in order to be explicit and unambiguous:

```py
@overload
def f(x: int = 4) -> list[int]: ...
@overload
def f[T](x: T) -> list[T]: ...
```

although I think using a type parameter with a default would also be unambiguous (but mypy still rejects it -- not sure if that's a mypy bug or not):

```py
def f[T = int](x: T = 4) -> list[T]: ...
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4579 on 2025-03-19 17:17_

I think this should not be required for positional-only parameters? Their name cannot affect whether a call succeeds or fails to bind.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4564 on 2025-03-19 17:19_

Ultimately we will also need `is_gradual_equivalent_to`, in which `None` vs `None` should be considered equivalent. Similar to what we previously discussed for `is_subtype_of` vs `is_assignable_to`, it may end up being simplest to rename this method to `is_gradual_equivalent_to` and ensure it handles all types correctly, and in the `is_equivalent_to_` case it will only be called with fully-static callable types.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:127 on 2025-03-19 17:28_

As far as I know (and can see), we don't use the default value from the Signature object either for verifying its assignability to the annotated type, or for deciding the type of the parameter internally in the function. All of that is derived directly from inference on the AST. So I think just storing a boolean for defaults could work.

I do see a significant advantage to trying to maintain the invariant that equivalent callable types should be identical. Not only for implementation complexity of `is_equivalent_to`, but also for predictable behavior. If two callable types are equivalent, we would simplify one of them from a union in favor of the other, and it could be weird if we would arbitrarily carry metadata that doesn't affect equivalence from one of them and not from the other.

The other change I think we would need to make in order to implement "equivalent signatures are identical" is to never store a name for a positional-only parameter. (It also looks like this PR might currently be missing the case that two callable types are equivalent if their only difference is the name of a positional-only parameter.)

Another approach could be to maintain this invariant for `GeneralCallable` types, but not necessarily for `Signature` itself. That is, `Signature` could retain the ability for positional-only parameters to have names, and retain the type of default values, but we could ensure that whenever we create a `GeneralCallable` type, we erase the default types (replace with `Unknown`) and erase the names of positional-only parameters. This approach would require a bit more care, but might be useful if we think that precise default types and names of positional-only parameters are useful to have for e.g. signatures of `FunctionLiteral` types. (I am not sure they are useful even there, though. So I would be inclined to first evaluate the "never store this information" option and see what the fallout is like. It will probably impact whether we use parameter names or positions in binding errors against positional-only parameters, but I am not sure that using names is preferable there.)

Regarding @dcreager 's generics example, I think it is interesting but in some sense orthogonal to this PR. If we decide to follow pyright and include the default type as a constraint on a type variable, then we not only need to preserve that default type in the signature, we also need to consider it in equivalence of callable types. Two generic functions differing only in the default type of a parameter would not actually be equivalent. So this could be supported (if we decide we need to support it) by introducing an enum with `DefaultType::Present` and `DefaultType::Precise(Type<'db>)` variants, or (if we decide to have `Signature` continue to represent default types, but erase them to `Unknown` in callable types) by just not erasing those particular default types that do matter.

---

_@carljm reviewed on 2025-03-19 17:28_

I would be inclined to explore whether we can keep an invariant that equivalent callable types are normalized to actually be the same `Signature` object.

---

_@dhruvmanila reviewed on 2025-03-20 04:55_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:4564 on 2025-03-20 04:55_

We already have `is_gradual_equivalent_to` but I understand what you mean. I'm going to first explore whether we can maintain the equivalence relation at the type level and then see if this needs to be simplified.

---

_@dhruvmanila reviewed on 2025-03-20 05:18_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:127 on 2025-03-20 05:18_

Yeah, I was thinking of exploring an alternate implementation of maintaining the invariant at the type level, will do so now.

> The other change I think we would need to make in order to implement "equivalent signatures are identical" is to never store a name for a positional-only parameter. (It also looks like this PR might currently be missing the case that two callable types are equivalent if their only difference is the name of a positional-only parameter.)

Yeah, I missed that. This does have a minor fallout where the display would only show the type of the parameter as there's no name but I don't think that's a major issue. Mypy does not show the name for the positional-only parameter while Pyright does:
```
Pyright: Type of "right" is "(x: int, y: float, /, *, a: int) -> None"
mypy: Revealed type is "def (builtins.int, builtins.float, *, a: builtins.int)"
```

Additionally, this might also have a fallout in the LSP where the hover implementation and the signature help would only show the positional-only parameter type and not the name. Following is a screenshot triggering the signature help for the first parameter:

<img width="652" alt="Screenshot 2025-03-20 at 10 46 02 AM" src="https://github.com/user-attachments/assets/c424535e-7e36-4b85-9c86-11851550bd68" />

It might be fine for signature help but I think the hover implementation should still show the signature with the parameter names even for positional-only parameters.

---

_@dhruvmanila reviewed on 2025-03-20 05:46_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:4579 on 2025-03-20 05:46_

I think it's the same for variadic and keyword-variadic parameters as well because their name is not relevant in a subtype relation.

Both of the assignment is valid which means they're equivalent:

```py
class OtherCallable(Protocol):  # Supertype
    def __call__(self, *args1: int) -> Any: ...


class SelfCallable(Protocol):  # Subtype
    def __call__(self, *args2: int) -> None: ...


def _(self_callable: SelfCallable, other_callable: OtherCallable):
    a: OtherCallable = self_callable
    b: SelfCallable = other_callable
```

---

_@dhruvmanila reviewed on 2025-03-20 06:43_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:4564 on 2025-03-20 06:43_

Hmm, taking a look at the current implementation of `is_gradual_equivalent_to`, I think it's incorrect because it only looks at the annotated type for the parameters and not any other properties like name, default, and kind. This means that the following two callable types are gradual equivalent:

```py
def f1(a): ...
def f2(b): ...
```

I think I'll fix this as a follow-up which will then also include the simplification because it needs to special case the `...` gradual form.

---

_Comment by @github-actions[bot] on 2025-03-20 06:57_

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

_@dhruvmanila reviewed on 2025-03-20 07:06_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:127 on 2025-03-20 07:06_

This will also have a fallout in the error messages where we won't be able to include the name of the positional-only, variadic and keyword-variadic parameters, it will only include the index.

For example, in a function that accepts positional-only parameter:
```diff
- Object of type `float` cannot be assigned to parameter 1 (`a`) of function `pos_only`; expected type `int`
+ Object of type `float` cannot be assigned to parameter 1 of function `pos_only`; expected type `int`
```

For variadic and keyword-variadic, we could choose to be explicit and mention the parameter kind instead of the name:
```diff
- Object of type `float` cannot be assigned to parameter `*args` of function `var`; expected type `int`
+ Object of type `float` cannot be assigned to variadic parameter of function `var`; expected type `int` [lint:invalid-argument-type]
```

And, for keyword variadic parameter:

```diff
+ Object of type `float` cannot be assigned to keyword variadic parameter of function `kvar`; expected type `int` [lint:invalid-argument-type]
```

---

_Comment by @dhruvmanila on 2025-03-20 12:19_

**Update from the above discussion:**

1. I explored the option to maintain the equivalent relation at the type level but it has certain fallout for downstream usages which I've mentioned [here](https://github.com/astral-sh/ruff/pull/16698#discussion_r2004828071) and [here](https://github.com/astral-sh/ruff/pull/16698#discussion_r2004947916).
2. For now, I've updated the manual implementation account for the cases where name shouldn't be checked for positional-only, variadic and keyword-variadic parameter
3. It turns out `is_gradual_equivalent_to` is incorrect in that sense, refer to [this comment](https://github.com/astral-sh/ruff/pull/16698#discussion_r2004920075) and will fix it in a follow-up which will also simplify both `is_equivalent_to` and `is_gradual_equivalent_to`

---

_@carljm reviewed on 2025-03-20 23:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:127 on 2025-03-20 23:20_

I think all of these observations are not arguments against making equivalent `GeneralCallable` types identical, but they are arguments in favor of this variant that I mentioned:

> Another approach could be to maintain this invariant for `GeneralCallable` types, but not necessarily for `Signature` itself. That is, `Signature` could retain the ability for positional-only parameters to have names, and retain the type of default values, but we could ensure that whenever we create a `GeneralCallable` type, we erase the default types (replace with `Unknown`) and erase the names of positional-only parameters.

With LSP hover and with most function calls, you will have a specific known `FunctionLiteral` or `BoundMethod`, which are singletons and therefore have no equivalent, and can maintain full details. I think in the much less common case of calling a general callable type (this would mostly happen with callbacks or similar scenarios), it is fine if we don't have names of positional or variadic arguments. Most general callable types in practice will come from `Callable` annotations, which can't even represent named positional arguments or variadic arguments.

So I still think it would be preferable to implement this "erasing" of irrelevant details when we go from a function-literal to a general callable type, so that equivalent callable types are identical.

But I'm also OK with deferring that if you would rather land this approach for now.



---

_@carljm approved on 2025-03-20 23:28_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:127 on 2025-03-21 03:08_

> So I still think it would be preferable to implement this "erasing" of irrelevant details when we go from a function-literal to a general callable type, so that equivalent callable types are identical.
> 
> But I'm also OK with deferring that if you would rather land this approach for now.

Yeah, I think that makes sense. This does mean that (a) we'll need to make `name: Name` into `name: Option<Name>` to erase it or replace it with `Name("")` and (b) use something else for the default type as `Type::Unknown` is not a static type, maybe just replace it with `None`.

I'd prefer to go forward with this approach now, I'll open an issue with some of my thoughts regarding this.

---

_@dhruvmanila reviewed on 2025-03-21 03:08_

---

_Merged by @dhruvmanila on 2025-03-21 03:19_

---

_Closed by @dhruvmanila on 2025-03-21 03:19_

---

_Branch deleted on 2025-03-21 03:19_

---

_@dhruvmanila reviewed on 2025-03-21 08:48_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:127 on 2025-03-21 08:48_

#16881 

---
