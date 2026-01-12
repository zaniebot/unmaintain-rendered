```yaml
number: 13879
title: "[red-knot] false positive \"possibly not defined\" for symbol used in generator expression inside `for` loop"
type: issue
state: closed
author: alex
labels:
  - bug
  - ty
assignees: []
created_at: 2024-10-22T13:16:16Z
updated_at: 2025-01-09T17:51:28Z
url: https://github.com/astral-sh/ruff/issues/13879
synced_at: 2026-01-12T15:54:53Z
```

# [red-knot] false positive "possibly not defined" for symbol used in generator expression inside `for` loop

---

_@alex_

(I'm aware that red-knot is not yet stable).

This was tested with `7dbd8f0f8e725d05cc1d15e6ef1c6cc2ed2b2090` with no Ruff settings.

Code:
```python
import datetime


def get_weeks() -> list[datetime.date]:
    raise NotImplementedError


def get_week_groups() -> list[list[datetime.date]]:
    pass


def main():
    weeks = get_weeks()
    all_week_groups = get_week_groups()
    for week in weeks:
        for group in all_week_groups:
            if any(d == week for d in group):
                print("Blah")
```

red-knot output:

```
/t/ruff ❯❯❯ ./target/release/red_knot --current-directory=z/
ERROR /private/tmp/ruff/z/t.py:17:25: Name `week` used when possibly not defined
```

But obviously `week` is always defined there.

---

_Label `bug` added by @AlexWaygood on 2024-10-22 13:48_

---

_Label `red-knot` added by @AlexWaygood on 2024-10-22 13:48_

---

_Added to milestone `Red Knot 2024` by @carljm on 2024-11-07 15:21_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-11-09 13:00_

---

_Comment by @AlexWaygood on 2024-11-13 16:18_

Okay, here's a much more minimal repro. It turns out the bug is not in fact to do with nested `for` loops -- it's to do with a nested scope inside the `for` loop -- because the generator expression inside the `any()` call is, of course, a nested scope! We emit a false-positive "x is possibly undefined" error on the final line here:

```py
class IntIterator:
    def __next__(self) -> int:
        return 42

class IntIterable:
    def __iter__(self) -> IntIterator:
        return IntIterator()

for x in IntIterable():
    reveal_type(x)  # revealed: int

    # revealed: int
    # revealed: int
    any(reveal_type(x) == reveal_type(y) for y in IntIterable())
```

The reason why we think that `x` here is undefined when viewed from the perspective of code inside the nested scope is that our current model uses the "public type" of symbols from outer scopes when considered by code in inner scopes. For a symbol with a declared type (a symbol with an explicit type annotation), we use the type declaration for the public type. But `x` here is not declared, so we use the final inferred type from the end of the outer scope for the public type. That makes sense for nested functions, since nested functions aren't necessarily called immediately -- in fact, they're usually not! But here it causes a false positive, since this generator expression _is_ executed immediately, and only ever inside an iteration of the loop, in which the symbol `x` is guaranteed to be bound.

This limitation in our model (that some nested scopes in fact run immediately, and should be modeled as such) has already been noted in https://github.com/astral-sh/ruff/issues/13933 with respect to class and comprehension scopes. But generator expressions are trickier! When passed to an `any()` or `all()` expression, generator scopes execute immediately. We could possibly use a heuristic here: "any function that accepts an arbitrary `Iterable` and is passed a generator expression is more likely than not to execute that generator expression immediately". But it's not guaranteed to be the case that _any_ arbitrary generator expression passed to a function call will be immediately executed; and using heuristics like the one I just proposed above is at least a bit ugly.

I'm curious how mypy and pyright deal with this... I'll investigate that now.

---

_Renamed from "[red-knot] scope bug with nested for loops" to "[red-knot] false positive "possibly not defined" for symbol used in generator expression inside `for` loop" by @AlexWaygood on 2024-11-13 18:05_

---

_Comment by @AlexWaygood on 2024-11-13 19:20_

> I'm curious how mypy and pyright deal with this... I'll investigate that now.

The conclusion of my investigation is that both mypy and pyright appear to make no distinction between generator expressions and comprehensions when it comes to modelling how the scopes work. And for all of them, they appear to model that the scope runs immediately.

- mypy treats all comprehensions and generator expressions as ScopeType.Generator in its internal model, and it models all ScopeType.Generators as executing immediately: https://github.com/python/mypy/blob/2ebc690279c7e20ed8bec006787030c5ba57c40e/mypy/partially_defined.py#L223-L225
- I believe pyright uses the same AST node for list comprehensions and generator expressions, and uses this `isGenerator` flag to determine whether the node refers to a generator expression or list comprehension: https://github.com/microsoft/pyright/blob/db2c9b0522664351b6d3078f82a72e798ca168f6/packages/pyright-internal/src/parser/parseNodes.ts#L1417-L1424. The only place where it looks to me like pyright reads this flag appears to be https://github.com/microsoft/pyright/blob/db2c9b0522664351b6d3078f82a72e798ca168f6/packages/pyright-internal/src/analyzer/typeEvaluator.ts#L14360-L14364

---

_Removed from milestone `Red Knot 2024` by @carljm on 2025-01-09 17:43_

---

_Added to milestone `Red Knot Q1 2025` by @carljm on 2025-01-09 17:43_

---

_Comment by @carljm on 2025-01-09 17:51_

I'm going to close this and consider it merged into https://github.com/astral-sh/ruff/issues/13933

---

_Closed by @carljm on 2025-01-09 17:51_

---
