```yaml
number: 162
title: "walrus expressions in a comprehension scope \"leak\" into the comprehension's enclosing scope"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - runtime semantics
assignees: []
created_at: 2025-03-24T15:36:10Z
updated_at: 2025-05-11T07:30:31Z
url: https://github.com/astral-sh/ty/issues/162
synced_at: 2026-01-10T02:34:09Z
```

# walrus expressions in a comprehension scope "leak" into the comprehension's enclosing scope

---

_Issue opened by @AlexWaygood on 2025-03-24 15:36_

### Summary

List/set/dict comprehensions, and generator expressions, run code in an isolated scope, meaning that names defined in the comprehension body cannot be accessed from outside the comprehension:

```pycon
>>> [x for x in range(2)]
[0, 1]
>>> x
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    x
NameError: name 'x' is not defined
```

But... there is one exception to this: walrus expressions! Here, `y` is not accessible from outside the comprehension scope, but `x` _is_ because it was defined using a walrus (it "leaks" into the comprehension's enclosing scope):

```pycon
>>> [(x := y*2) for y in range(2)]
[0, 2]
>>> y
Traceback (most recent call last):
  File "<python-input-3>", line 1, in <module>
    y
NameError: name 'y' is not defined
>>> x
2
```

This inconsistency might _look_ like a bug in CPython, but it's intentional behaviour. PEP 572, which introduced walrus expressions to Python, [states](https://peps.python.org/pep-0572/#scope-of-the-target):

> An assignment expression does not introduce a new scope. In most cases the scope in which the target will be bound is self-explanatory: it is the current scope. If this scope contains a `nonlocal` or `global` declaration for the target, the assignment expression honors that. A lambda (being an explicit, if anonymous, function definition) counts as a scope for this purpose.
>
> There is one special case: an assignment expression occurring in a list, set or dict comprehension or in a generator expression (below collectively referred to as “comprehensions”) binds the target in the containing scope, honoring a `nonlocal` or `global` declaration for the target in that scope, if one exists. For the purpose of this rule the containing scope of a nested comprehension is the scope that contains the outermost comprehension. A lambda counts as a containing scope.
> 
> The motivation for this special case is twofold. First, it allows us to conveniently capture a “witness” for an `any()` expression, or a counterexample for `all()`, for example:
> 
> ```py
> if any((comment := line).startswith('#') for line in lines):
>     print("First comment:", comment)
> else:
>     print("There are no comments")
> 
> if all((nonblank := line).strip() == '' for line in lines):
>     print("All lines are blank")
> else:
>     print("First non-blank line:", nonblank)
> ```
> 
> Second, it allows a compact way of updating mutable state from a comprehension, for example:
> 
> ```py
> # Compute partial sums in a list comprehension
> total = 0
> partial_sums = [total := total + v for v in values]
> print("Total:", total)
> ```

Use of this feature is reasonably common for generator expressions in `any()` and `all()` (both mentioned as motivations in the PEP), so it's important for red-knot to model this accurately. We currently don't -- this test fails on `main` when it should pass:

```py
class Iterator:
    def __next__(self) -> int:
        return 42

class Iterable:
    def __iter__(self) -> Iterator:
        return Iterator()

[(a := b * 2) for b in Iterable()]
[c for d in Iterable() if (c := d - 10) > 0]
{(e := f * 2): (g := f * 3) for f in Iterable()}
list(((h := i * 2) for i in Iterable()))

reveal_type(a)  # revealed: int
reveal_type(c)  # revealed: int
reveal_type(e)  # revealed: int
reveal_type(g)  # revealed: int
reveal_type(h)  # revealed: int
```

https://playknot.ruff.rs/5be255af-1c62-44b1-b843-eaba65293980

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-03-24 15:36_

---

_Renamed from "red-knot should model that definitions created using walrus expressions inside comprehension scopes "leak" into the comprehension's enclosing scope" to "red-knot should model that definitions created using walrus expressions inside a comprehension scope "leak" into the comprehension's enclosing scope" by @AlexWaygood on 2025-03-24 15:38_

---

_Comment by @carljm on 2025-03-24 16:04_

Yeah, I think there is a TODO comment for this somewhere in SemanticIndexBuilder IIRC. Thanks for writing it up!

---

_Comment by @dhruvmanila on 2025-03-24 16:05_

https://github.com/astral-sh/ruff/blob/b9ac714d95f36f232711398ebf7a8e27962f76a7/crates/red_knot_python_semantic/src/semantic_index/builder.rs#L1671-L1676

---

_Renamed from "red-knot should model that definitions created using walrus expressions inside a comprehension scope "leak" into the comprehension's enclosing scope" to "[red-knot] walrus expressions in a comprehension scope "leak" into the comprehension's enclosing scope" by @carljm on 2025-03-26 12:13_

---

_Renamed from "[red-knot] walrus expressions in a comprehension scope "leak" into the comprehension's enclosing scope" to "walrus expressions in a comprehension scope "leak" into the comprehension's enclosing scope" by @MichaReiser on 2025-05-07 15:26_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-11 07:30_

---
