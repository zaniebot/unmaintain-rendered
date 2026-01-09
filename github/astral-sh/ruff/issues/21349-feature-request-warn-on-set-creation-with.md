---
number: 21349
title: "Feature request: warn on set creation with unhashable types"
type: issue
state: open
author: gtkacz
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-11-09T22:08:57Z
updated_at: 2025-11-10T17:00:10Z
url: https://github.com/astral-sh/ruff/issues/21349
synced_at: 2026-01-07T13:12:16-06:00
---

# Feature request: warn on set creation with unhashable types

---

_Issue opened by @gtkacz on 2025-11-09 22:08_

### Summary

If an user is trying to create a `set` or similar on unhashable types, it will error with something like `TypeError: unhashable type: 'list'`. This can easily be warned by `ruff` if it knows the type of the variable by checking for the existence of the `__hash__` method. 

Which would warn the user in something like:

```py
some_set = set([{'foo': 'bar'}])
```

Worth pointing out that iterables with root-level hashable types can be passed, e.g.:

```py
set({'foo': 'bar'})
>>> {'foo'}

set([0, 1, 2])
>>> {0, 1, 2}
```

Example checker implementation in Python:

```py
def is_unhashable(var: Any) -> bool:
	if not var.__class__.__hash__ and (var.__class__.__iter__ and not var[0].__class__.__hash__):
		return True

	return False

is_unhashable([0, 1, 2])
>>> False

is_unhashable([{'foo': 'bar'}])
>>> True

is_unhashable('foo')
>>> False
```

---

_Comment by @ntBre on 2025-11-10 14:22_

My first thought was that this might be a better fit for a type checker, but it doesn't look like [ty](https://play.ty.dev/1f0224b0-f593-40d3-a1d4-a59cf0b6eada), [pyright](https://pyright-play.net/?pythonVersion=3.8&strict=true&code=M4ewtgpg%2BsEC4AIC8DZwBQG0DeByAZiCLgFwK4BGAhgE64C%2BAugJRA), or [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=8ce24737929eb07d09bd82545ada1a42) flag this example either.

---

_Label `rule` added by @ntBre on 2025-11-10 14:22_

---

_Label `needs-decision` added by @ntBre on 2025-11-10 14:22_

---

_Comment by @MichaReiser on 2025-11-10 15:24_

Yeah, that was my thought too but `set` takes an `Iterable[T]` without any constraints on `T`. There's an `Hashable` Protocol but it isn't widely used. @AlexWaygood probably knows more. 

We could hard code common cases but a proper implementation requires multifile analysis, which Ruff doesn't support yet.

---

_Comment by @dylwil3 on 2025-11-10 16:53_

A relevant `typeshed` issue: https://github.com/python/typeshed/issues/9571

---

_Comment by @AlexWaygood on 2025-11-10 16:57_

Type checkers are mostly useless for accurately inferring hashability, unfortunately. There is a `Hashable` protocol, but a major problem is that `object` is inferred as a subtype of `Hashable` (because `hash(object())` works at runtime). You can logically infer from this that all Python objects are hashable, since all Python types are subtypes of `object`... but wait, that's obviously not true.

Hashability in Python just doesn't follow the normal rules that Python type checkers are meant to assume when they type-check Python code; it's a fundamental violation of the Liskov Substitution Principle baked deep into the language. I actually think Ruff may be able to do better than ty here just by hardcoding a few common cases of things that are known not to be hashable; it's very hard for type checkers to get this right.

---

_Comment by @AlexWaygood on 2025-11-10 17:00_

For related discussion, see:
- https://github.com/astral-sh/ty/issues/1132
- https://github.com/astral-sh/ruff/pull/20284
- https://github.com/PyCQA/flake8-pyi/issues/283#issuecomment-1248095891
- https://github.com/astral-sh/ruff/blob/1d188476b608c94cea5cb2a97ae248d70a0e2154/crates/ty_vendored/vendor/typeshed/stdlib/typing.pyi#L867-L873

---
