---
number: 14598
title: "[red-knot] Infer precise types for `len()` calls"
type: issue
state: closed
author: InSyncWithFoo
labels:
  - ty
assignees: []
created_at: 2024-11-26T04:51:59Z
updated_at: 2024-12-04T19:16:55Z
url: https://github.com/astral-sh/ruff/issues/14598
synced_at: 2026-01-10T01:22:55Z
---

# [red-knot] Infer precise types for `len()` calls

---

_Issue opened by @InSyncWithFoo on 2024-11-26 04:51_

Sometimes, it is useful to know the precise length of a type:

```python
reveal_type(len((0, 1, 2)))  # revealed: Literal[3]
```

This might help with narrowing:

```python
a: tuple[Literal[1], int] | tuple[Literal[2], str, str]

if len(a) == 2:
	reveal_type(a[0])  # revealed: Literal[1]
```

Current behaviour of [Mypy](https://mypy-play.net/?mypy=latest&python=3.13&gist=012c5dba20655618e8b42b119c151143) and [Pyright](https://pyright-play.net/?pyrightVersion=1.1.389&pythonVersion=3.14&strict=true&enableExperimentalFeatures=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAySMApiAIYA2AsAFB10AmJwUwAFOQFywCuClEgG0ipCpSEBGALoAaTChjSoAHz4DhoslSEAmOVADOMEPOMhpASigBaAHxQAcmBQkudKJ8ytBKTtYBeAKhdd1ovCKgQEgA3EioAfXgEEk4hAAYrTwBiKAhEOAiAhy1xKWkPSKrqmtyEOBAkNAALfGLCYm0JGUrPXqhKSShg339%2B6LjE5NTB62rc-Pqih1QYfpqNqrqGptbhlcUGcK8kVnZKXSguEZI-cktA4ND1rwn4yiTEGd05nLyC5YKNbHTagiLbRotNolTplfQvTxvKZfNKZOYLAFedqlHQ9EFgsEQ3bQjpiXHyeG0IA):

```python
def f(a: tuple[Literal[1], int] | tuple[Literal[2], str, str]) -> None:
    if len(a) == 2:
        reveal_type(a[0])  # mypy    => Literal[1]
                           # pyright => Literal[1]
    
    l1 = len(a)
    reveal_type(l1)        # mypy    => int
                           # pyright => int

    if (l2 := len(a)) == 2:
        reveal_type(l2)    # mypy    => int
                           # pyright => Literal[2]
        reveal_type(a[0])  # mypy    => Literal[1]
                           # pyright => Literal[1, 2]
```

---

_Referenced in [astral-sh/ruff#14599](../../astral-sh/ruff/pulls/14599.md) on 2024-11-26 04:55_

---

_Label `red-knot` added by @MichaReiser on 2024-11-26 06:50_

---

_Closed by @carljm on 2024-12-04 19:16_

---

_Closed by @carljm on 2024-12-04 19:16_

---
