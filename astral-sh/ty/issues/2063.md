---
number: 2063
title: "`__package__` in the global namespace should have type `str`, not `str | None`"
type: issue
state: open
author: spaceone
labels: []
assignees: []
created_at: 2025-12-18T11:50:53Z
updated_at: 2026-01-07T23:31:00Z
url: https://github.com/astral-sh/ty/issues/2063
synced_at: 2026-01-10T01:51:14Z
---

# `__package__` in the global namespace should have type `str`, not `str | None`

---

_Issue opened by @spaceone on 2025-12-18 11:50_

### Question

I am unsure about the following, but mypy doesn't detect it:

```sh
$ ty check t.py 
t.py:4:23: error[invalid-argument-type] Argument to function `version` is incorrect: Expected `str`, found `str | None`
$ cat t.py
```
```python
from importlib.metadata import version


__version__ = version(__package__)
```

### Version

0.0.3

---

_Label `question` added by @spaceone on 2025-12-18 11:50_

---

_Comment by @spaceone on 2025-12-18 12:10_

fixing it will cause a `mypy` issue:

`__version__ = version(cast('str', __package__)) # E: Redundant cast to "str"  [redundant-cast]`

---

_Comment by @sinon on 2025-12-18 12:38_

> fixing it will cause a `mypy` issue:
> 
> `__version__ = version(cast('str', __package__)) # E: Redundant cast to "str" [redundant-cast]`

A solution to keep mypy etc happy would be to narrow out the `| None` for ty instead of cast

```py
from importlib.metadata import version


__version__ = version(__package__ or "unset")

# or

if __package__:
    __version__ = version(__package__)
else:
    __version__ = version("unset")
```
https://play.ty.dev/edb7161a-2b3d-4373-87ed-c4f60edf5bfc

I think this is similar to: https://github.com/astral-sh/ty/issues/860 where based on typeshed alone `__package__` can be `None` https://github.com/search?q=repo%3Apython%2Ftypeshed%20__package__&type=code but typecheckers can probably infer that at runtime it will only be `str`

---

_Comment by @AlexWaygood on 2025-12-18 12:42_

Thanks! This issue is similar to https://github.com/astral-sh/ty/issues/860.

We get our type here from typeshed's `types.ModuleType` stub: https://github.com/python/typeshed/blob/8d96801533918957fb194e101cb321bfe1f836f8/stdlib/types.pyi#L362-L368. The reason why typeshed has the type `str | None` for this attribute is this line in the [Python data model](https://docs.python.org/3/reference/datamodel.html#module.__package__):

> defaults to `None` for modules created dynamically using the [types.ModuleType](https://docs.python.org/3/library/types.html#types.ModuleType) constructor; use [importlib.util.module_from_spec()](https://docs.python.org/3/library/importlib.html#importlib.util.module_from_spec) instead to ensure the attribute is set to a [str](https://docs.python.org/3/library/stdtypes.html#str).

But when we encounter `__package__` in the global namespace like this, it's extremely likely that the module is going to be implicitly created by the Python interpreter rather than created using an explicit `types.ModuleType` instantiation. So I think we could safely override typeshed's type here and infer `str` for `__package__` in the global namespace.

---

_Label `question` removed by @AlexWaygood on 2025-12-18 13:47_

---

_Renamed from "false positive for `__package__: str | None` ?" to "`__package__` in the global namespace should have type `str`, not `str | None`" by @AlexWaygood on 2025-12-18 13:47_

---

_Added to milestone `Stable` by @carljm on 2025-12-19 00:56_

---

_Comment by @sinon on 2026-01-07 19:37_

I started looking at implementing the support for this similar to how `__file__` is handled currently but I don't think the naive approach works as obviously in this case.

Given a testcase like:
```
ty-2063/
├── main.py
├── mod
│   ├── __init__.py
│   ├── a.py
│   └── b.py
└── top.py
```
with various `print(__package__)`, `reveal_type` and `main.py` being an entrypoint along with it importing all other modules.

### Runtime

```
uv run ty-2063/main.py
__main__.py None
Runtime type is 'NoneType'
mod.__init__ mod
Runtime type is 'str'
mod.a mod
Runtime type is 'str'
mod.b mod
Runtime type is 'str'
top  # NOTE: for clarity this is empty str
Runtime type is 'str'
```

### mypy

```sh
uv tool run mypy ty-2063/
ty-2063/mod/b.py:5: note: Revealed type is "builtins.str"
ty-2063/mod/a.py:5: note: Revealed type is "builtins.str"
ty-2063/top.py:5: note: Revealed type is "builtins.str"
ty-2063/mod/__init__.py:5: note: Revealed type is "builtins.str"
ty-2063/main.py:4: note: Revealed type is "builtins.str"
Success: no issues found in 5 source files
```
This is wrong for `main.py`, and if it used `__package__` with similar method to `version` like in issue description it would be a Runtime error.

### pyright

```sh
uv tool run pyright ty-2063/
ty-2063/main.py
  ty-2063/main.py:4:13 - information: Type of "__package__" is "str | None"
ty-2063/mod/__init__.py
  ty-2063/mod/__init__.py:5:13 - information: Type of "p" is "str | None"
ty-2063/mod/a.py
  ty-2063/mod/a.py:5:13 - information: Type of "p" is "str | None"
ty-2063/mod/b.py
  ty-2063/mod/b.py:5:13 - information: Type of "p" is "str | None"
ty-2063/top.py
  ty-2063/top.py:5:13 - information: Type of "p" is "str | None"
0 errors, 0 warnings, 5 informations
```

### pyrefly

```sh
uv tool run pyrefly check ty-2063/ --output-format min-text
 INFO ty-2063/main.py:4:12-25: revealed type: str | None [reveal-type]
 INFO ty-2063/mod/__init__.py:5:12-15: revealed type: str | None [reveal-type]
 INFO ty-2063/mod/a.py:5:12-15: revealed type: str | None [reveal-type]
 INFO ty-2063/mod/b.py:5:12-15: revealed type: str | None [reveal-type]
 INFO ty-2063/top.py:5:12-15: revealed type: str | None [reveal-type]
```

So there is a question of what does `ty` do:
- follow `mypy` which might have less user perceived false positives but some true positives (quiet a pragmatic choice as generally the true positive case would generally fail loudly and quickly 🤔 )
- leave it as is and follow `pyright` and `pyrefly`. More correct but maybe less useful in many usecases of `__package__`
- see if we can leverage the data gathered by module resolvers to infer an entrypoint and thus when it's `None` (this seems hairy and a potentially requires a brittle heuristic in my example I could have easily choose to run `top.py` instead and it would have been `None` instead)



Relevant PEP on `__package__` behaviour: https://peps.python.org/pep-0366/




---

_Comment by @carljm on 2026-01-07 23:21_

I would be inclined to stay on the side of correctness here. I don't think it is feasible to detect statically if a package can be the entry point (or can be imported in an odd way). `assert __package__ is not None` is an easy workaround for anyone who is OK with failing fast in a case where `__package__` is not set.

---

_Removed from milestone `Stable` by @carljm on 2026-01-07 23:23_

---

_Comment by @carljm on 2026-01-07 23:24_

Leaving this open for now in case there's more discussion, but taking it out of Stable milestone, as I'm not sure we should do anything here at all, and if we decide to, I don't think it's critical that we do so before stable release.

---

_Comment by @carljm on 2026-01-07 23:31_

Thanks @sinon for the deeper investigation!

---
