```yaml
number: 458
title: "[panic] dependency graph cycle when querying value_type_(Id(6400)), set cycle_fn/cycle_initial to fixpoint iterate"
type: issue
state: closed
author: aslpavel
labels:
  - fatal
assignees: []
created_at: 2025-05-20T09:44:05Z
updated_at: 2025-05-20T13:14:11Z
url: https://github.com/astral-sh/ty/issues/458
synced_at: 2026-01-10T02:34:10Z
```

# [panic] dependency graph cycle when querying value_type_(Id(6400)), set cycle_fn/cycle_initial to fixpoint iterate

---

_Issue opened by @aslpavel on 2025-05-20 09:44_

Panic as the result of running `uvx ty check ukanren.py`

### File content:
<details>

<summary>ukanen.py</summary>

```python
#!/usr/bin/env python
# pyright: strict
from __future__ import annotations

import itertools
import unittest
from functools import reduce
from typing import Any, cast, final, override
from collections.abc import Callable, Iterable, Iterator


@final
class Var:
    """Variable"""

    __slots__ = ["id"]

    def __init__(self, id: int) -> None:
        self.id = id

    @override
    def __eq__(self, other: Any) -> bool:
        return isinstance(other, Var) and self.id == other.id

    @override
    def __hash__(self) -> int:
        return hash(self.id)

    @override
    def __repr__(self) -> str:
        return f"Var({self.id})"


type Val = Var | tuple[Val, ...] | int | str | None
type State = tuple[SMap, int]
type Stream[State] = Iterable[State]
type Goal = Callable[[State], Stream[State]]

type _List[V] = tuple[V, _List[V]] | None


@final
class ImmMap[K, V]:
    __slots__ = ["root"]

    def __init__(self, root: dict[K, V]) -> None:
        self.root = root

    @classmethod
    def empty(cls) -> ImmMap[K, V]:
        return cls({})

    def insert(self, key: K, value: V) -> ImmMap[K, V]:
        return ImmMap(self.root | {key: value})

    def get[O](self, key: K, default: O = None) -> V | O:
        return self.root.get(key, default)

    def __iter__(self) -> Iterator[tuple[K, V]]:
        return iter(self.root.items())

    @override
    def __repr__(self) -> str:
        return f"ImmMap({self.root})"


GUARD = cast(Val, object())


@final
class SMap:
    """Substitution map"""

    __slots__ = ["root"]

    def __init__(self, root: ImmMap[Var, Val]):
        self.root = root

    def walk(self, var: Val) -> Val:
        """Resolve variable until unmapped of value is reached"""
        if not isinstance(var, Var):
            return var
        val = self.root.get(var, GUARD)
        if val is GUARD:
            return var
        else:
            return self.walk(val)

    def deep_walk(self, val: Val) -> Val:
        """Walk complex type"""
        val = self.walk(val)
        if isinstance(val, Var):
            return val
        elif isinstance(val, tuple):
            return tuple(map(self.deep_walk, val))
        return val

    def assoc(self, var: Var, val: Val) -> SMap:
        """Bind variable to a value"""
        return SMap(self.root.insert(var, val))

    @override
    def __repr__(self) -> str:
        return f"{self.root}"


type _Body[*P] = Callable[[*P], Goal]
# type constraint `*P : Var` on the above is not supported
type Body = (
    _Body[Var]
    | _Body[Var, Var]
    | _Body[Var, Var, Var]
    | _Body[Var, Var, Var, Var]
    | _Body[Var, Var, Var, Var, Var]
    | _Body[Var, Var, Var, Var, Var, Var]
)


def fresh(body: Body) -> Goal:
    """Allocate new fresh variable(s)"""

    def body_fresh(state: State) -> Stream[State]:
        smap, count = state
        return body(*(Var(index) for index in range(count, count + arity)))(
            (smap, count + arity)
        )

    arity = body.__code__.co_argcount
    assert arity > 0, "body of `fresh` should take at least one argument"
    return body_fresh


def unify(smap: SMap, left: Val, right: Val) -> SMap | None:
    """Unify `left` with `right` terms"""
    left = smap.walk(left)
    right = smap.walk(right)

    if left == right:
        return smap
    elif isinstance(left, Var):
        return smap.assoc(left, right)
    elif isinstance(right, Var):
        return smap.assoc(right, left)
    elif (
        isinstance(left, tuple) and isinstance(right, tuple) and len(left) == len(right)
    ):
        for lval, rval in zip(left, right):
            smap_next = unify(smap, lval, rval)
            if smap_next is None:
                return None
            smap = smap_next
        return smap
    return None


# -----------------------------------------------------------------------------#
# Stream
# -----------------------------------------------------------------------------#
szero: Stream[Any] = ()


def splus[I](a: Stream[I], b: Stream[I]) -> Stream[I]:
    return itertools.chain(a, b)


def sunit[I](item: I) -> Stream[I]:
    return (item,)


def sbind[A, B](stream: Stream[A], fn: Callable[[A], Stream[B]]) -> Stream[B]:
    for i_item in stream:
        yield from fn(i_item)


def siter[I](stream: Stream[I]) -> Iterator[I]:
    return iter(stream)


def sdelay[I](thunk: Callable[[], Stream[I]]) -> Stream[I]:
    return thunk()


# -----------------------------------------------------------------------------#
# Primitives
# -----------------------------------------------------------------------------#
def eq(left: Val, right: Val) -> Goal:
    def eq_goal(state: State) -> Stream[State]:
        smap, count = state
        smap = unify(smap, left, right)
        if smap is None:
            return szero
        return sunit((smap, count))

    return eq_goal


def disj(*goals: Goal) -> Goal:
    """Disjoint: any goals must be reached"""

    def disj(first: Goal, second: Goal) -> Goal:
        return lambda state: splus(first(state), second(state))

    return reduce(disj, goals)


def conj(*goals: Goal) -> Goal:
    """Conjoint: all goals must be reaached"""

    def conj(first: Goal, second: Goal) -> Goal:
        return lambda state: sbind(first(state), second)

    return reduce(conj, goals)


def delay(goal: Goal) -> Goal:
    return lambda state: sdelay(lambda: goal(state))


# -----------------------------------------------------------------------------#
# Run
# -----------------------------------------------------------------------------#
def run(body: Body) -> Iterator[Val]:
    def reify(state: State) -> Val:
        smap, _ = state
        return smap.deep_walk(Var(0))

    # from fn.immutable import Map as ImmMap
    empty_state: State = (SMap(ImmMap[Var, Val].empty()), 0)
    return map(reify, siter(fresh(body)(empty_state)))


# -----------------------------------------------------------------------------#
# Unittest
# -----------------------------------------------------------------------------#
class MicroKanrenUnittest(unittest.TestCase):
    @staticmethod
    def einstein_problem() -> Callable[[Var], Goal]:
        def left_of(i0: Val, i1: Val) -> Goal:
            """index `i1` is directly following `i2` in the range of [0, 4)"""
            goals: list[Goal] = []
            for index in range(0, 4):
                goals.append(conj(eq(i0, index), eq(i1, index + 1)))
            return disj(*goals)

        def next_to(i0: Val, i1: Val) -> Goal:
            """index `i0` is adjacent to `i1`"""
            return disj(left_of(i0, i1), left_of(i1, i0))

        def member(x: Val, xs: Val) -> Goal:
            """`x` is a member of `xs` tuple of 5 elements"""
            return delay(
                fresh(
                    lambda x0, x1, x2, x3, x4: conj(
                        eq(xs, (x0, x1, x2, x3, x4)),
                        disj(eq(x, x0), eq(x, x1), eq(x, x2), eq(x, x3), eq(x, x4)),
                    )
                )
            )

        def house_attrs(house: Val, **attrs: Val) -> Goal:
            """House with specified attributes"""

            def house_attrs(
                index: Val,
                nation: Val,
                color: Val,
                pet: Val,
                drink: Val,
                smoke: Val,
            ) -> Goal:
                goals = [eq(house, (index, nation, color, pet, drink, smoke))]
                kv = attrs.copy()
                for key, var in (
                    ("index", index),
                    ("nation", nation),
                    ("color", color),
                    ("pet", pet),
                    ("drink", drink),
                    ("smoke", smoke),
                ):
                    val = kv.pop(key, None)
                    if val is not None:
                        goals.append(eq(val, var))
                assert not kv, f"unhandled attrs: {kv}"
                return conj(*goals)

            return fresh(house_attrs)

        def house(street: Val, house: Val, **attrs: Val) -> Goal:
            """`house` on the `street` with specified attributes"""
            return conj(
                house_attrs(house, **attrs),
                member(house, street),
            )

        def house_exists(street: Val, **attrs: Val) -> Goal:
            """Any house on the `street` with specified attributes"""
            return fresh(lambda h: house(street, h, **attrs))

        def house_next_to(
            street: Val,
            attrs_0: dict[str, Val],
            attrs_1: dict[str, Val],
        ) -> Goal:
            """Two houses on the `street` next to each other with `attrs_0` and `attrs_1`"""
            return fresh(
                lambda h0, i0, h1, i1: conj(
                    next_to(i0, i1),
                    house(street, h0, index=i0, **attrs_0),
                    house(street, h1, index=i1, **attrs_1),
                )
            )

        def house_left_of(
            street: Val,
            attrs_0: dict[str, Val],
            attrs_1: dict[str, Val],
        ) -> Goal:
            """Two houses on the `street` with `attrs_0` followed by house with `attrs_1`"""
            return fresh(
                lambda h0, i0, h1, i1: conj(
                    left_of(i0, i1),
                    house(street, h0, index=i0, **attrs_0),
                    house(street, h1, index=i1, **attrs_1),
                )
            )

        return lambda q: fresh(
            lambda fish_owner, street: conj(
                eq(q, (fish_owner, street)),
                fresh(
                    lambda h0, h1, h2, h3, h4: conj(
                        eq(street, (h0, h1, h2, h3, h4)),
                        house_attrs(h0, index=0),
                        house_attrs(h1, index=1),
                        house_attrs(h2, index=2),
                        house_attrs(h3, index=3),
                        house_attrs(h4, index=4),
                    )
                ),
                house_exists(street, nation="brit", color="red"),
                house_exists(street, nation="swede", pet="dog"),
                house_exists(street, nation="dane", drink="tea"),
                house_left_of(street, dict(color="green"), dict(color="white")),
                house_exists(street, color="green", drink="coffee"),
                house_exists(street, smoke="pall mall", pet="bird"),
                house_exists(street, smoke="dunhill", color="yellow"),
                house_exists(street, index=2, drink="milk"),
                house_exists(street, index=0, nation="norweigan"),
                house_next_to(street, dict(smoke="blend"), dict(pet="cat")),
                house_next_to(street, dict(pet="horse"), dict(smoke="dunhill")),
                house_exists(street, drink="beer", smoke="bluemaster"),
                house_exists(street, nation="german", smoke="prince"),
                house_next_to(street, dict(nation="norweigan"), dict(color="blue")),
                house_next_to(street, dict(smoke="blend"), dict(drink="water")),
                house_exists(street, nation=fish_owner, pet="fish"),
            )
        )

    def test_einstein_problem(self) -> None:
        results = tuple(run(self.einstein_problem()))
        self.assertEqual(len(results), 1)
        result = results[0]
        assert isinstance(result, tuple)
        fish_owner, street = result
        self.assertEqual(fish_owner, "german")
        self.assertEqual(
            street,
            (
                (0, "norweigan", "yellow", "cat", "water", "dunhill"),
                (1, "dane", "blue", "horse", "tea", "blend"),
                (2, "brit", "red", "bird", "milk", "pall mall"),
                (3, "german", "green", "fish", "coffee", "prince"),
                (4, "swede", "white", "dog", "beer", "bluemaster"),
            ),
        )


if __name__ == "__main__":
    unittest.main()
```

</details>

### Output:
```
error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/7edce6e/src/function/fetch.rs:129:25 when checking `/home/pavelaslanov/.config/configs/libs/py/ukanren.py`: `dependency graph cycle when querying value_type_(Id(6400)), set cycle_fn/cycle_initial to fixpoint iterate.
Query stack:
[
    check_types(Id(c00)),
    infer_scope_types(Id(1000)),
    infer_definition_types(Id(1436)),
    infer_expression_types(Id(1805)),
    value_type_(Id(6400)),
    infer_scope_types(Id(1006)),
    symbol_by_id(Id(301a)),
    infer_definition_types(Id(6e52)),
    all_narrowing_constraints_for_expression(Id(192c)),
    member_lookup_with_policy_(Id(2c15)),
    imported_modules(Id(243f)),
    infer_deferred_types(Id(1662)),
    infer_definition_types(Id(1612)),
    symbol_by_id(Id(3008)),
    infer_definition_types(Id(1711)),
]`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Args: ["ty", "check", "ukanren.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_scope_types(Id(1006))
             at crates/ty_python_semantic/src/types/infer.rs:121
   1: value_type_(Id(6400))
             at crates/ty_python_semantic/src/types.rs:7477
   2: infer_expression_types(Id(1805))
             at crates/ty_python_semantic/src/types/infer.rs:222
   3: infer_definition_types(Id(1436))
             at crates/ty_python_semantic/src/types/infer.rs:148
   4: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:121
   5: check_types(Id(c00))
             at crates/ty_python_semantic/src/types.rs:84
```

---

_Renamed from "[panic] While running `check` sub-command" to "[panic] dependency graph cycle when querying value_type_(Id(6400)), set cycle_fn/cycle_initial to fixpoint iterate" by @aslpavel on 2025-05-20 09:49_

---

_Label `fatal` added by @AlexWaygood on 2025-05-20 11:37_

---

_Comment by @sharkdp on 2025-05-20 13:12_

Thank you for reporting this. It happens due to the self-referential `Val` type alias. ~~I will add a MRE of this to #256~~ (actually, we have very similar examples there that lead to the same cycle entry point).

---

_Closed by @sharkdp on 2025-05-20 13:12_

---
