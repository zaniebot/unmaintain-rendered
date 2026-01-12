```yaml
number: 15809
title: "`C417` has multiple problems"
type: issue
state: open
author: InSyncWithFoo
labels:
  - bug
assignees: []
created_at: 2025-01-29T14:16:15Z
updated_at: 2025-11-06T14:16:38Z
url: https://github.com/astral-sh/ruff/issues/15809
synced_at: 2026-01-12T15:54:55Z
```

# `C417` has multiple problems

---

_@InSyncWithFoo_


While working on #15802, I noticed a few problems with [`C417`](https://docs.astral.sh/ruff/rules/unnecessary-map/) ([playground](https://play.ruff.rs/f6b44cea-27b5-44c5-bdab-f8c9678fe99f)).

When `list`/`set`/`dict` from the outer call is an overshadowed binding, the inner `map()` call is not reported at all:

```python
	map(lambda x: x, [])
#   ^^^^^^^^^^^^^^^^^^^^ Reported

	list(map(lambda x: x, []))
#   ^^^^^^^^^^^^^^^^^^^^^^^^^^ Outer `list()` call reported, nested `map()` call not reported

	def _():
		list = ...  # Overshadowed

		map(lambda x: x, [])
#       ^^^^^^^^^^^^^^^^^^^^ Reported

		list(map(lambda x: x, []))  # Currently no error
#            ^^^^^^^^^^^^^^^^^^^^ This should be reported on its own merit
```

If the iterable is a starred expression, the fix outputs a comprehension with different semantics:

```python
a = [(1, 2), (3, 4)]

# Before
map(lambda x: [*x, 10], *a)  # TypeError: <lambda>() takes 1 positional argument but 2 were given

# After
([*x, 10] for x in a)        # [1, 2, 10], [3, 4, 10]
```

`map()` calls over named expressions are not rewritten, but `list(map())`/`set(map())`/`dict(map())` are:

```python
# Before
map(lambda x: x + 10, (a := []))
list(map(lambda x: x + 10, (a := [])))
set(map(lambda x: x + 10, (a := [])))
dict(map(lambda x: (x, 10), (a := [])))

# After
map(lambda x: x + 10, (a := []))
[x + 10 for x in (a := [])]  # Undetected syntax error
{x + 10 for x in (a := [])}  # Undetected syntax error
{x: 10 for x in (a := [])}   # Undetected syntax error
```

The rule currently also doesn't report when there are multiple iterables:

```python
map(lambda x, y: x + y, a, b)  # Currently no error

# Can be simplified to:
(x + y for x, y in zip(a, b))
```


---

_Label `bug` added by @charliermarsh on 2025-01-29 15:59_

---

_Comment by @enkiusz on 2025-11-06 14:16_

Double parenthesis are also sometimes introduced, for example:

```shell
$ ruff cat test.py
d = {
    'ep': [
        { 'c': 21 },
        { 'c': 22 }
    ]
}

sum(map(lambda x: x.c, d['ep']))

$ ruff ruff check --fix --unsafe-fixes --show-fixes test.py
Fixed 1 error:
- test.py:
    1 × C417 (unnecessary-map)

Found 1 error (1 fixed, 0 remaining).                                                                               
➜  ruff cat test.py
d = {
    'ep': [
        { 'c': 21 },
        { 'c': 22 }
    ]
}

sum((x.c for x in d['ep']))

$ ruff 
```

---
