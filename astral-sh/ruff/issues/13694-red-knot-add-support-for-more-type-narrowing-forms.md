---
number: 13694
title: "[red-knot] add support for more type narrowing forms"
type: issue
state: closed
author: carljm
labels:
  - help wanted
  - ty
assignees: []
created_at: 2024-10-09T19:30:27Z
updated_at: 2025-04-02T00:15:45Z
url: https://github.com/astral-sh/ruff/issues/13694
synced_at: 2026-01-10T01:22:54Z
---

# [red-knot] add support for more type narrowing forms

---

_Issue opened by @carljm on 2024-10-09 19:30_

- [x] x is not None
- [x] #13715
- [x] x is not T (where T is any singleton type, incl a class or function)
- [x] x is T
- [x] x != T (where y is of type T with T a singleton type)
- [x] #13893
- [x] not isinstance(x, T) (where T is a class or tuple of classes)
- [x] apply negated narrowing from previously excluded cases to `elif` and `else` blocks
- [x] E and F (where E and F are both supported narrowing expressions)
- [x] E or F (where E and F are both supported narrowing expressions; reduces to (T & C(E)) | (T & C(F)), where T is the prior type of a variable and C(E) / C(F) are the constraints on it obtained from E and F)
- [x] #14117
- [x] not issubclass(x, T) (where T is a class or tuple of classes)
- [x] #14431
- [x] type(x) is not C (where C is a class)
- [x] #14550
- [x] #14547 
- [ ] match-statement narrowing forms (individual TODOs in code, could break these out)
- [ ] TypeIs and TypeGuard special forms

This is not an exhaustive list of what we'll eventually want to support, it's just a starter list of some useful ones that aren't currently blocked by other type system features that aren't implemented yet.

(Supporting narrowing on `x.attr` or `x[sub]` expression forms is out of scope for this issue.)

Implementation for these lives in `crates/red_knot_python_semantic/src/types/narrow.rs`.

These cases we may want eventually, but we are deferring for now (please _don't_ add support for these right now):

- [ ] x == None (not sound for most x, don't want to do it unless driven by real use cases)
- [ ] x == T (where T is any literal type) -- not sound for most x; need real use cases (maybe TypedDicts are a use case?)
- [ ] x != T (where T is any literal type) -- use cases not clear yet; could result in complex-but-useless intersections


---

_Label `red-knot` added by @carljm on 2024-10-09 19:30_

---

_Label `help wanted` added by @carljm on 2024-10-09 19:32_

---

_Comment by @carljm on 2024-10-09 21:35_

https://github.com/microsoft/pyright/blob/main/docs/type-concepts-advanced.md#type-guards is pyright's documentation on supported narrowing forms.

---

_Comment by @dhruvmanila on 2024-10-10 05:06_

Thanks for listing them down. There are also pattern constraints which are applied and yet to be implemented: https://github.com/astral-sh/ruff/blob/5b4afd30caebd6e2c5c27bbf5debc1663cbb28a2/crates/red_knot_python_semantic/src/types/narrow.rs#L96-L125

---

_Comment by @Lexxxzy on 2024-10-11 09:54_

Is one of the tasks a good first issue? I really liked the idea behind red-knot and am looking forward to contribute

---

_Comment by @carljm on 2024-10-11 16:53_

Adding support for `x != None` should be very good for a first issue, since it should have exactly the same effect as `x is not None`, which is already supported.

---

_Referenced in [astral-sh/ruff#13749](../../astral-sh/ruff/pulls/13749.md) on 2024-10-14 12:01_

---

_Referenced in [astral-sh/ruff#13803](../../astral-sh/ruff/pulls/13803.md) on 2024-10-17 22:30_

---

_Referenced in [astral-sh/ruff#13893](../../astral-sh/ruff/issues/13893.md) on 2024-10-23 13:58_

---

_Referenced in [astral-sh/ruff#13918](../../astral-sh/ruff/pulls/13918.md) on 2024-10-24 22:26_

---

_Comment by @carljm on 2024-10-25 17:48_

Some predicates narrow based on characteristics of an object that could be transient for mutable objects.

For example, `if x:` means that `x` must be truthy. This means that if `x` is typed as `None`, or `Literal[False]` or `Literal[0]`, we can narrow its type to `Never`, since those types represent known immutable objects that are never truthy and never can be. If `x` is typed as a union including any of those types, we can safely eliminate them from the union. And it will be important that we can do this. But we cannot generally assume that this narrowing means `x` will remain truthy. If `x` is a list, it could be truthy one moment, and the next moment (after a `.clear()` call), it no longer is.

It is not correct to represent the narrowing effect of `if x == 1:` as an intersection with `Literal[1]`, because any type in Python can write an `__eq__` method that compares equal to the integer `1`. So `x == 1` does not imply that `x` must actually be of type `Literal[1]`. Even if `x` were previously typed as `int`, it could be an instance of a subclass of `int` that compares equal to `1`; that's not `Literal[1]`. And similar to truthiness, we can't try to preserve our knowledge that `x == 1` for arbitrary `x` type, because `x` could be mutable, and its equality to `1` could change any time after the narrowing, without us being aware of it. But there are some types that we know represent objects that can never become equal to `1`: for instance `None`, or any other literal type that isn't `Literal[1]`. It will be important that we can narrow away these types.

To sum this up with examples, this is the behavior I think we will want:

```py
x: int | None
if x:
    # this could also be `int & ~Literal[0]`?
    reveal_type(x)  # revealed: int
else:
    # we can't narrow `int` down to `Literal[0]` because of subclasses
    reveal_type(x)  # revealed: int | None
```

```py
x: Literal[0, 1, 2]
if x:
    reveal_type(x)  # revealed: Literal[1, 2]
else:
    reveal_type(x)  # revealed: Literal[0]
```

```py
x: int
if x == 1:
    # not safe to narrow to `Literal[1]`, could be a subclass of `int` with custom `__eq__`
    reveal_type(x)  # revealed: int
else:
    # could be `int & ~Literal[1]`
    reveal_type(x)  # revealed: int
```

```py
x: Literal[1, 2, 3]
if x == 1:
    reveal_type(x)  # revealed: Literal[1]
else:
    reveal_type(x)  # revealed: Literal[2, 3]
```

One way to approach this, which is tempting in its generality (and previously tempted me), is to define new `Type` variants such as `Truthy` and `EqualTo(T)`, and then return these types as the constraints on x from `x` or `x == 1`. Then we can handle these types in intersection simplification, where e.g. `Truthy` would be disjoint with `None` and `Literal[False]` and some other types, and `EqualTo[Literal[1]]` would be disjoint from all other literals.

The downsides of this approach are two-fold. One is that we then have to handle these types in all type operations, increasing the size of the matrix for binary operations especially, even though it's not clear that these types have expressive utility outside of narrowing.

The bigger problem is that these types represent possibly-transient characteristics, as discussed above. A particular `list[int]` may be truthy one moment, and then after a `.clear()` call, it is no longer truthy. This means that really the _only_ safe use of these types is to immediately eliminate union elements that we know must always be disjoint with them; otherwise we should simply discard them. For example, after `x == 1` where `x` is `int`, not only can we not narrow `x` to `Literal[1]`, we shouldn't even keep its type as `int & EqualTo(Literal[1])`, because it may be that some subsequent method call on `x` makes it no longer `EqualTo(Literal[1])` at all. In the same way, `l.clear()` would invalidate an intersection like `list[int] & Truthy`.

I think this is sufficient reason to discard having types like `EqualTo` or `Truthy`.

I think the key observation here is that, since there are a limited set of types where we know all inhabitants are immutable with respect to equality and truthiness (all literal and singleton types), the narrowing we can apply in these cases is only a _negative_ intersection to eliminate some of these literal types, never an intersection with a positive type. If the narrowing is represented as an intersection with some constraint type (as we currently represent it), the constraint type must be a negation type (an intersection with only negative elements in it.)

This is easy for `if x:`: it can be safely represented as an intersection with `~Literal[0] & ~Literal[False] & ~Literal[""] & ~None`. But this approach doesn't work for narrowing `if not x:` or `if x == 1:`, because the set of all known-never-to-be-falsy types is too large to represent directly (it includes all literal types that are not `0/False/""/None`), and similarly the set of all known-not-to-equal-1 types is very large (all literal types that are not `Literal[1]`).

We could inspect the current type of `x` in `if x == 1:` while inferring narrowing constraints, and just build a negative intersection containing only the _relevant_ types from the too-large-to-represent set of negative constraints. That is, if `x` is `Literal[1, 2, 3]` and we have `if x == Literal[1]:`, in theory we'd want to intersect `x` with the too-large-to-represent type "all literal types that are not `Literal[1]`", but in practice we could intersect it simply with `~Literal[2] & ~Literal[3]` and get the same result. This muddies the boundaries between inferring a constraint and applying it. But this may be a totally fine pragmatic approach.

A variant of this could be to have constraints no longer be represented simply as a `Type` to intersect with, but as a new enum that might be just a `Type` to intersect with, or might be something like `EliminateNotEqualTo[Literal[1]]` or `EliminateNonTruthy`, and then we'd apply those constraints later, in type inference, when we already have the type of `x` in hand. The actual application would look similar to what we'd do in the previous paragraph, we just wouldn't do it eagerly when inferring the constraint itself, so as to keep "inference of constraints from a test expression" and "application of constraints to a type" more cleanly separated. `Type` could even gain dedicated methods like `exclude_never_equal_to(Type)` and `exclude_always_truthy` to handle intersecting a type with these too-large-to-represent negations.

One practical reason to prefer the latter (placing this logic in `Type` instead of directly inside constraint inference) is that there are other places we may want to narrow a type in this way, where we aren't using definitions and constraints. One such case is https://github.com/astral-sh/ruff/issues/13632

EDIT for posterity: in the end in #14687 we did decide to add `Type::AlwaysTruthy` and `Type::AlwaysFalsy`, but with a narrower definition. These types are only inhabited by objects which are always guaranteed to return `LiteralTrue]` or `Literal[False]` from `__bool__`, never by objects which may return different values at different times. That addresses the unsoundness issues discussed above. We can only ever intersect with these types as a negated type; so e.g. `if x` intersects with `~AlwaysFalsy`, allowing us to eliminate known-always-falsy types (e.g. `Literal[False]` or `Literal[0]` or an instance of a class with `def __bool__(self) -> Literal[False]`) from the type of `x`.

---

_Referenced in [astral-sh/ruff#13632](../../astral-sh/ruff/issues/13632.md) on 2024-10-25 17:59_

---

_Referenced in [astral-sh/ruff#13665](../../astral-sh/ruff/pulls/13665.md) on 2024-10-25 18:01_

---

_Comment by @TomerBin on 2024-10-28 23:40_

"Not isinstance" and "elif else" can be checked off ✅

---

_Referenced in [astral-sh/ruff#14037](../../astral-sh/ruff/pulls/14037.md) on 2024-11-01 11:14_

---

_Comment by @cake-monotone on 2024-11-04 12:40_

I think we also need to handle `TypeIs[T]` and `TypeGuard[T]` for narrowing. (blocked by the implementation of generics for now)

Although the implementation will likely be much later, I thought it would be useful to list it here. Once completed, we should be able to effectively test it using various functions from the inspect module (e.g., `isclass`, `isfunction`).

---

_Comment by @carljm on 2024-11-04 21:30_

Yeah, I was thinking of `TypeIs` and `TypeGuard` as a separate feature, but it doesn't hurt to list it here.

I don't think it is blocked on generics. These are just special forms, which use bracket syntax, but they don't require user-defined generic classes or functions.

---

_Referenced in [astral-sh/ruff#14117](../../astral-sh/ruff/issues/14117.md) on 2024-11-05 20:19_

---

_Referenced in [astral-sh/ruff#14431](../../astral-sh/ruff/issues/14431.md) on 2024-11-18 12:26_

---

_Referenced in [astral-sh/ruff#14432](../../astral-sh/ruff/pulls/14432.md) on 2024-11-18 12:27_

---

_Referenced in [astral-sh/ruff#14550](../../astral-sh/ruff/issues/14550.md) on 2024-11-22 23:45_

---

_Comment by @InSyncWithFoo on 2024-11-28 00:05_

Another possible narrowing pattern:

```python
a: LiteralString

if a == "foo":
	reveal_type(a)  # revealed: Literal["foo"]
```


---

_Referenced in [astral-sh/ruff#14648](../../astral-sh/ruff/issues/14648.md) on 2024-11-28 00:10_

---

_Referenced in [astral-sh/ruff#14668](../../astral-sh/ruff/pulls/14668.md) on 2024-11-29 06:01_

---

_Referenced in [astral-sh/ruff#14687](../../astral-sh/ruff/pulls/14687.md) on 2024-11-30 07:21_

---

_Referenced in [astral-sh/ruff#16314](../../astral-sh/ruff/pulls/16314.md) on 2025-02-22 12:17_

---

_Referenced in [astral-sh/ruff#16974](../../astral-sh/ruff/pulls/16974.md) on 2025-03-26 04:35_

---

_Referenced in [astral-sh/ruff#17030](../../astral-sh/ruff/pulls/17030.md) on 2025-03-28 06:07_

---

_Referenced in [astral-sh/ruff#17059](../../astral-sh/ruff/pulls/17059.md) on 2025-03-29 23:54_

---

_Comment by @MatthewMckee4 on 2025-04-02 00:04_

I'm looking at `evaluate_expr_compare` now, and in the `ast::Expr::Name` arm the `CmpOp` types left are `Lt`, `LtE`, `Gt`, `GtE`. Are these arms that we intend to cover? It seems like we probably cannot very easily implement any narrowing for these. @carljm 

https://github.com/astral-sh/ruff/blob/eb3e1763096bd85b37f1609b6baf1e10694143cf/crates/red_knot_python_semantic/src/types/narrow.rs#L416-L418

Edit: From https://github.com/microsoft/pyright/blob/main/docs/type-concepts-advanced.md#type-guards pyright supports `len(x) < L` where x is a tuple. I suppose i can add this?

---

_Comment by @carljm on 2025-04-02 00:09_

Sure, adding the length narrowing on tuples would be fine, though I think relatively low priority. I agree that otherwise there's probably no narrowing we can do on less-than or greater-than compares.

One kind of narrowing not mentioned here (could create a new issue for it) that would have significant impact on false positives would be narrowing on assert statements. This doesn't involve narrowing on any new kinds of test expressions, it just means adding a narrowing constraint in `SemanticIndexBuilder` when we hit an `assert` statement.

---

_Comment by @MatthewMckee4 on 2025-04-02 00:12_

Cool, sounds good, I'm up for that

---

_Comment by @carljm on 2025-04-02 00:15_

I'm going to go ahead and close this issue, we've done most of the things listed here, and I think there's limited value at this point in having a blanket tracking issue for narrowing. We can create new issues as we decide to prioritize new kinds of narrowing.

---

_Closed by @carljm on 2025-04-02 00:15_

---

_Referenced in [astral-sh/ty#117](../../astral-sh/ty/issues/117.md) on 2025-04-21 22:08_

---
