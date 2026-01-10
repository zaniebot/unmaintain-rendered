```yaml
number: 210
title: Rethink public types of symbols
type: issue
state: closed
author: dcreager
labels: []
assignees: []
created_at: 2025-01-27T21:16:48Z
updated_at: 2025-06-26T10:24:42Z
url: https://github.com/astral-sh/ty/issues/210
synced_at: 2026-01-10T02:07:35Z
```

# Rethink public types of symbols

---

_Issue opened by @dcreager on 2025-01-27 21:16_


https://github.com/astral-sh/ruff/pull/15676 currently has the following mdtest with incorrect results:

```py
def top_level_return(cond1: bool, cond2: bool):
    x = 1

    def g():
        reveal_type(x)  # revealed: Unknown | Literal[1, 2, 3]

    if cond1:
        if cond2:
            x = 2
        else:
            x = 3
    return

def return_from_if(cond1: bool, cond2: bool):
    x = 1

    def g():
        reveal_type(x)  # revealed: Unknown | Literal[1]

    if cond1:
        if cond2:
            x = 2
        else:
            x = 3
        return

def return_from_nested_if(cond1: bool, cond2: bool):
    x = 1

    def g():
        reveal_type(x)  # revealed: Unknown | Literal[1, 3]

    if cond1:
        if cond2:
            x = 2
            return
        else:
            x = 3
```

We should determine how we want to handle the assignments in this scope.

One possibility (which mimics our current behavior for module-level scopes) would be to reveal `Literal[1, 2, 3]` for all three.  (This would require changes to our terminal statement support: we currently purposefully resolve the `x` use in the nested function relative to the end of `f`'s scope, but the `return` statements are preventing some of the bindings from being visible there.)

---

_Comment by @carljm on 2025-01-27 21:58_

Just to be clear, the "should reveal" assertions in the example above are based on what we would currently reveal for a similarly-defined symbol in a module level scope, if we don't consider the `return` statements. That doesn't mean they represent what we _should_ reveal. It's a bit tricky to say what we should reveal, because it all depends on how much analysis we attempt of where the nested scope can possibly be called from. In all three examples above, a sufficiently powerful analysis would be able to declare that `g` is never called, and doesn't escape from the local scope to be called elsewhere, so we may as well consider the body of `g` itself to be unreachable code.

Barring any attempt to analyze where the nested scope can be called from, I think the starting point has to be that all definitions are visible, regardless of control flow. (This is effectively modeling that `g` could be, perhaps transitively, called from anywhere, including e.g. after `x = 2` and before `return` in the third example, meaning even `Literal[2]` should be considered a possible value. Of course we can see that in fact `g` can't be called there, but that's starting to "analyze where the nested scope can be called from".)

Also, for symbols in function-like scopes, if we don't see a nested scope with a `nonlocal` statement for that symbol and assignment to it, we can remove the union with `Unknown`, because nothing outside the scope can reassign the symbol, even if it is not declared.

All of that is to say: we have a lot of design work to do on types for closed-over variables, and "the status quo before terminal support", as documented in the tests shown above, is not a very good guide to the desired eventual behavior.

---

_Comment by @dcreager on 2025-01-28 21:32_

> All of that is to say: we have a lot of design work to do on types for closed-over variables, and "the status quo before terminal support", as documented in the tests shown above, is not a very good guide to the desired eventual behavior.

I've updated the issue description to not presume what behavior we'll want to implement.

---

_Comment by @sharkdp on 2025-04-08 06:38_

Just adding a note here that the changes in https://github.com/astral-sh/ruff/pull/17169 can probably be reverted after we fixed this.

---

_Comment by @sharkdp on 2025-04-09 09:16_

I think this public-types issue does not just affect function-like scopes. One false-positive example that I see relatively often is the following:

```py
def flag() -> bool:
    return True


if flag():
    x = 1

    def f() -> int:
        return x  # Red Knot: Name `x` used when possibly not defined

    # Function only used inside this branch
    f()
```

It also requires a more sophisticated analysis of the location at which the inner function is defined and called.

---

_Renamed from "[red-knot] Rethink public types of symbols in function-like scopes" to "Rethink public types of symbols in function-like scopes" by @MichaReiser on 2025-05-07 15:27_

---

_Comment by @carljm on 2025-05-14 17:47_

Great example case in #389 (closed as duplicate)

---

_Renamed from "Rethink public types of symbols in function-like scopes" to "Rethink public types of symbols" by @carljm on 2025-05-28 23:43_

---

_Comment by @DetachHead on 2025-06-10 09:48_

a more simplified example:

```py
a = 1


def foo():
    reveal_type(a)  # Unknown | Literal[1]
```

---

_Comment by @sharkdp on 2025-06-10 10:55_

> a more simplified example:
> 
> a = 1
> 
> 
> def foo():
>     reveal_type(a)  # Unknown | Literal[1]

The `Unknown | Literal[1]` here is intended, see https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md for details.

---

_Assigned to @sharkdp by @carljm on 2025-06-17 16:13_

---

_Closed by @sharkdp on 2025-06-26 10:24_

---
