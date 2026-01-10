```yaml
number: 15715
title: "[red-knot] Handle public symbols declared as `Unknown` like undeclared symbols"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/union-for-declared-types
created_at: 2025-01-24T13:15:14Z
updated_at: 2025-01-25T12:25:02Z
url: https://github.com/astral-sh/ruff/pull/15715
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Handle public symbols declared as `Unknown` like undeclared symbols

---

_Pull request opened by @sharkdp on 2025-01-24 13:15_

## Summary

For *definitely declared* symbols, we currently rely on the declared type only, even if we also see bindings for that symbol. This means that we handle the two cases (1) a symbol is undeclared and (2) a symbol is declared with type `Any`/`Unknown` differently, which seems inconsistent.

One potential solution to this problem is the following: Instead of returning `T_declared` as the public type of a declared symbol, we return `T_declared | T_declared & T_inferred` instead. This represents a type that is *no smaller than and no larger than* `T_declared`. For fully static types, this construct simplifies to `T_declared`. But for non-fully-static types, this can lead to more precise types. For example, if the declared type is `Any` (or `Unknown`), we now use `Any | Any & T` which can be simplified to `Any | T`. This represents the fact that the public type for this symbol must be at least as large as the inferred type `T`. This allows us to issue diagnostics in cases like the following:

```py
from typing import Any

class C:
    x: Any = 1

reveal_type(C.x)  # Any | Literal[1]    (previously: Any)

y: str = C.x  # error: [invalid-assignment]
```
Red knot (main), mypy and pyright issue no diagnostic in the last line here.

In the example given above, it looks like the intersection is not necessary. However, consider the following scenario. We still want to see `int` as the public type here. This is possible because `int | int & Any = int`:
```py
from typing import Any

def any() -> Any: ...

class C:
    x: int = any()

reveal_type(C.x)  # int
```

The same behavior should also apply to possibly-declared symbols.

### Why `(T_declared | T_declared & T_inferred)`?

The nice property of this expression is that it works for other cases in the matrix below as well.

#### Undeclared (`T_declared = Unknown`)

If a symbol is definitely undeclared, we can set `T_declared = Unknown`. In this case, the expression `T_declared | T_declared & T_inferred = Unknown | Unknown & T_inferred` simplifies to `Unknown | T_inferred`. This is exactly what we have in the **last column** of the matrix below (where the entry in the last *row* further simplifies because `Unknown | Unknown = Unknown`).

#### Unbound (`T_inferred = Unknown`)

If a symbol is definitely unbound, we can set `T_inferred = Unknown`. In this case, the expression `T_declared | T_declared & T_inferred = T_declared | T_declared & Unknown` simplifies to `T_declared`. This is exactly what we have in the **last row** of the matrix (where the entry in the last *column* simplifies because `T_declared` is `Unknown` in this case as well)

### Type inference matrix

(changes affect the first column only)

#### Before

| **Public type**  | declared                   | possibly-undeclared | undeclared         |
| ---------------- | -------------------------- | ------------------- | -------------------|
| bound            | `T_decl` &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | `T_decl \| T_inf`   | `Unknown \| T_inf` |
| possibly-unbound | `T_decl` | `T_decl \| T_inf`   | `Unknown \| T_inf` |
| unbound          | `T_decl`                   | `T_decl`            | `Unknown`          |

#### After

| **Public type**  | declared                   | possibly-undeclared  | undeclared         |
| ---------------- | -------------------------- | ---------------------| ------------------ |
| bound            | `T_decl \| T_decl & T_inf` | `T_decl \| T_inf`    | `Unknown \| T_inf` |
| possibly-unbound | `T_decl \| T_decl & T_inf` | `T_decl \| T_inf`    | `Unknown \| T_inf` |
| unbound          | `T_decl`                   | `T_decl`             | `Unknown`          |

### What about possibly-undeclared?

It would be great if we could replace `T_declared -> T_declared | Unknown` for this case (and similar for possibly unbound symbols). This is doable, but leads to lots of unions/intersections with `Unknown`, something that we don't fully simplify at the moment, so I have not fully looked into this yet. As it stands, our handling of possibly-undeclared symbols is at least debatable. For example, should we issue a diagnostic in the following example (we currently do not).
```py
def flag() -> bool: return True
def get_int() -> int: ...

class C:
    if flag():
        x: str
    else:
        x = get_int()

C.x = 1  # this seems to violate the `str` declaration?
```

## Test Plan

Updated existing tests.

---

_Label `red-knot` added by @sharkdp on 2025-01-24 13:15_

---

_Review requested from @carljm by @sharkdp on 2025-01-24 13:15_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-24 13:15_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-24 13:15_

---

_@sharkdp reviewed on 2025-01-24 13:39_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/assignment/annotations.md`:57 on 2025-01-24 13:39_

These changes look a bit strange, but they will all go away when the TODO is resolved.

---

_@sharkdp reviewed on 2025-01-24 13:40_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/annotations/unsupported_special_forms.md`:21 on 2025-01-24 13:40_

Similar to the other comment: This change look a bit strange, but it will go away when the TODO is resolved.

---

_@AlexWaygood reviewed on 2025-01-24 14:40_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/assignment/annotations.md`:57 on 2025-01-24 14:40_

Yes. For `f`, and `g` once we understand the annotation we will infer the type as being

```py
tuple[str, *tuple[int, ...], bytes] | tuple[Literal["42"], Literal[b"42"]]
```

Which, since the second element in the union is a subtype of the first element, simplifies to

```py
tuple[str, *tuple[int, ...], bytes]
```

which is just what they have as their annotation already

---

_@sharkdp reviewed on 2025-01-24 14:42_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/assignment/annotations.md`:57 on 2025-01-24 14:42_

And that reasoning is true for every fully-static declared+inferred type. If we did not issue a diagnostic on the definition `x: T_declared = <expr of type T_inferred>`, it means that `T_inferred` is assignable to `T_declared`. And if `T_declared` and `T_inferred` are fully-static, that means `T_inferred <: T_declared`, i.e. `T_declared | T_inferred = T_declared`

---

_@AlexWaygood reviewed on 2025-01-24 14:55_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/assignment/annotations.md`:57 on 2025-01-24 14:55_

Do we have any tests where the assigned value is inferred as having a non-fully-static type but the declared type _is_ fully static? If not, could we add some?

E.g. for this:

```py
from typing_extensions import Any, reveal_type

def f(x: Any):
    y: int = x

    def g():
        reveal_type(y)
```

We reveal `int` on `main`. What do we reveal with this PR?

---

_@sharkdp reviewed on 2025-01-24 14:58_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/assignment/annotations.md`:57 on 2025-01-24 14:58_

> What do we reveal with this PR?

`int | Any`

---

_@AlexWaygood reviewed on 2025-01-24 15:04_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/assignment/annotations.md`:57 on 2025-01-24 15:04_

I think that's a reasonable outcome. If they want `y` to have a narrower type than `int | Any`, they should check the type of `x` before assigning it to `y`, e.g. with an `assert`

```py
from typing_extensions import Any, reveal_type

def f(x: Any):
    assert isinstance(x, int)
    reveal_type(x)  # Once we understand narrowing using `assert`, this should be
                    # `int & Any`, I think?
    y: int = x

    def g():
        reveal_type(y)  # with this PR, I think this would be `int | int & Any`,
                        # which should simplify to `int`
```

but yeah -- we should document this by adding a test, since this PR changes behaviour for things like this!

---

_@carljm reviewed on 2025-01-24 15:11_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/annotations.md`:57 on 2025-01-24 15:11_

I'm not sure that's a reasonable outcome. The assignment `y: int = x` is only valid because `x` has a not fully static type that can materialize to something that's a subtype of `int`. I don't think after that assignment we should be anymore accounting for a possibility that `y` has a type wider than `int`. If anything it seems like the type of `y` (and maybe also `x`!) here should be `int & Any`, but I don't think that's quite right either, it corresponds to effectively ignoring annotated types on valid assignments, which is what we do in local inference but is not right for public types. 

Overall I'm not sold on this PR yet, would like to think it over a bit more. 

---

_@sharkdp reviewed on 2025-01-24 15:12_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/assignment/annotations.md`:57 on 2025-01-24 15:12_

No, that's a good point that I hadn't thought about. And I'm not sure I like the behavior. In the `x: Any = 1` case, we make the resulting public type *more precise*. But in the case `x: int = returns_any()` here, we make the public type of `x` *less* precise. I'm not sure if that's desirable. Maybe we want `(T_declared | T_inferred) & T_declared`? :smile: 

---

_@dcreager reviewed on 2025-01-24 15:19_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/assignment/annotations.md`:57 on 2025-01-24 15:19_

If anything it seems like this example should introduce a narrowing constraint on _`x`_: `x: Any & int`

---

_@sharkdp reviewed on 2025-01-24 15:24_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/assignment/annotations.md`:57 on 2025-01-24 15:24_

Remember that this is about *public* uses of `x`. Someone could have modified `x` in the meantime. This is why we also want a lower bound, I think. So `(int | Any) & int = int | Any & int = int` for this case here.

---

_Converted to draft by @sharkdp on 2025-01-24 18:44_

---

_Renamed from "[red-knot] Use `T_declared | T_inferred` for declared public symbols" to "[red-knot] Handle public symbols declared as `Unknown` like undeclared symbols" by @sharkdp on 2025-01-24 19:24_

---

_@sharkdp reviewed on 2025-01-24 21:16_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/assignment/annotations.md`:57 on 2025-01-24 21:16_

> we should document this by adding a test, since this PR changes behaviour for things like this!

Done.

---

_@sharkdp reviewed on 2025-01-24 21:16_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness/public.md`:10 on 2025-01-24 21:16_

TODO for me.

---

_Closed by @sharkdp on 2025-01-25 12:25_

---
