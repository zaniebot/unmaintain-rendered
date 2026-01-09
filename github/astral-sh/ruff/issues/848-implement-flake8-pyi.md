---
number: 848
title: Implement flake8-pyi
type: issue
state: closed
author: JonathanPlasse
labels:
  - plugin
assignees: []
created_at: 2022-11-21T10:18:46Z
updated_at: 2023-08-04T23:00:51Z
url: https://github.com/astral-sh/ruff/issues/848
synced_at: 2026-01-07T13:12:14-06:00
---

# Implement flake8-pyi

---

_Issue opened by @JonathanPlasse on 2022-11-21 10:18_

# [`flake8-pyi`](https://pypi.org/project/flake8-pyi/)

## Error codes

- [x] `Y001` Names of TypeVars, ParamSpecs and TypeVarTuples in stubs should usually start with _. This makes sure you don't accidentally expose names internal to the stub.
- [x] `Y002` If test must be a simple comparison against sys.platform or sys.version_info. Stub files support simple conditionals to indicate differences between Python versions or platforms, but type checkers only understand a limited subset of Python syntax, and this warning triggers on conditionals that type checkers will probably not understand.
- [x] `Y003` Unrecognized sys.version_info check. Similar, but triggers on some comparisons involving version checks.
- [x] `Y004` Version comparison must use only major and minor version. Type checkers like mypy don't know about patch versions of Python (e.g. 3.4.3 versus 3.4.4), only major and minor versions (3.3 versus 3.4). Therefore, version checks in stubs should only use the major and minor versions. If new functionality was introduced in a patch version, pretend that it was there all along.
- [x] `Y005` Version comparison must be against a length-n tuple.
- [x] `Y006` Use only < and >= for version comparisons. Comparisons involving > and <= may produce unintuitive results when tools do use the full sys.version_info tuple.
- [x] `Y007` Unrecognized sys.platform check. Platform checks should be simple string comparisons.
- [x] `Y008` Unrecognized platform. To prevent you from typos, we warn if you use a platform name outside a small set of known platforms (e.g. "linux" and "win32").
- [x] `Y009` Empty body should contain ..., not pass. This is just a stylistic choice, but it's the one typeshed made.
- [x] `Y010` Function body must contain only .... Stub files should not contain code, so function bodies should be empty.
- [x] `Y011` All default values for typed function arguments must be .... Type checkers ignore the default value, so the default value is not useful information in a stub file.
- [x] `Y012` Class body must not contain pass.
- [x] `Y013` Non-empty class body must not contain ....
- [x] `Y014` All default values for arguments must be .... A stronger version of Y011 that includes arguments without type annotations.
- [x] `Y015` Attribute must not have a default value other than ....
- [x] `Y016` Unions shouldn't contain duplicates, e.g. str | str is not allowed.
- [x] `Y017` Stubs should not contain assignments with multiple targets or non-name targets.
- [x] `Y018` A private TypeVar should be used at least once in the file in which it is defined.
- [x] `Y019` Certain kinds of methods should use _typeshed.Self instead of defining custom TypeVars for their return annotation. This check currently applies for instance methods that return self, class methods that return an instance of cls, and `__new__` methods.
- [x] `Y020` Quoted annotations should never be used in stubs.
- [x] `Y021` Docstrings should not be included in stubs.
- [x] `Y022` Imports linting: use typing-module aliases to stdlib objects as little as possible (e.g. builtins.list over typing.List, collections.Counter over typing.Counter, etc.). (`UP035`)
- [x] `Y023` Where there is no detriment to backwards compatibility, import objects such as ClassVar and NoReturn from typing rather than typing_extensions.
- [x] `Y024` Use typing.NamedTuple instead of collections.namedtuple, as it allows for more precise type inference.
- [x] `Y025` Always alias collections.abc.Set when importing it, so as to avoid confusion with builtins.set.
- [x] `Y026` Type aliases should be explicitly demarcated with typing.TypeAlias.
- [x] `Y027` Same as Y022. Unlike Y022, however, the imports disallowed with this error code are required if you wish to write Python 2-compatible stubs. Switch this error code off in your config file if you support Python 2.
- [x] `Y028` Always use class-based syntax for typing.NamedTuple, instead of assignment-based syntax. (`UP014`)
- [x] `Y029` It is almost always redundant to define `__str__` or `__repr__` in a stub file, as the signatures are almost always identical to object.`__str__` and object.`__repr__`.
- [x] `Y030` Union expressions should never have more than one Literal member, as Literal[1] | Literal[2] is semantically identical to Literal[1, 2].
- [x] `Y031` TypedDicts should use class-based syntax instead of assignment-based syntax wherever possible. (In situations where this is not possible, such as if a field is a Python keyword or an invalid identifier, this error will not be raised.) (`UP013`)
- [x] `Y032` The second argument of an `__eq__` or `__ne__` method should usually be annotated with object rather than Any.
- [x] `Y033` Do not use type comments (e.g. x = ... # type: int) in stubs, even if the stub supports Python 2. Always use annotations instead (e.g. x: int).
- [x] `Y034` Y034 detects common errors where certain methods are annotated as having a fixed return type, despite returning self at runtime. Such methods should be annotated with _typeshed.Self. This check looks for:

  1.  Any in-place BinOp dunder methods (`__iadd__`, `__ior__`, etc.) that do not return Self.
  2.  `__new__`, `__enter__` and `__aenter__` methods that return the class's name unparameterised.
  3.  `__iter__` methods that return Iterator, even if the class inherits directly from Iterator.
  4.  `__aiter__` methods that return AsyncIterator, even if the class inherits directly from AsyncIterator.

This check excludes methods decorated with @overload or @abstractmethod.
- [x] `Y035` `__all__` and `__match_args__` in a stub file should always have values, as these special variables in a .pyi file have identical semantics to `__all__` and `__match_args__` in a .py file. E.g. write `__all__` = ["foo", "bar"] instead of `__all__`: list[str].
- [x] `Y036` Y036 detects common errors in `__exit__` and `__aexit__` methods. For example, the first argument in an `__exit__` method should either be annotated with object or type[BaseException] | None.
- [x] `Y037` Use PEP 604 syntax instead of typing.Union and typing.Optional. E.g. use str | int instead of Union[str, int], and use str | None instead of Optional[str]. (`UP007`)
- [x] `Y038` Use from collections.abc import Set as AbstractSet instead of from typing import AbstractSet. Like Y027, this error code should be switched off in your config file if your stubs support Python 2. (`UP035`)
- [x] `Y039` Use str instead of typing.Text. This error code is incompatible with stubs supporting Python 2. (Implemented as `UP019`.)
- [x] `Y040` Never explicitly inherit from object, as all classes implicitly inherit from object in Python 3. This error code is incompatible with stubs supporting Python 2. (Implemented as `UP004`.)
- [x] `Y041` Y041 detects redundant numeric unions. For example, PEP 484 specifies that type checkers should treat int as an implicit subtype of float, so int is redundant in the union int | float. In the same way, int is redundant in the union int | complex, and float is redundant in the union float | complex.
- [x] `Y042` Type alias names should use CamelCase rather than snake_case
- [x] `Y043` Do not use names ending in "T" for private type aliases. (The "T" suffix implies that an object is a TypeVar.)
- [x] `Y044` from `__future__` import annotations has no effect in stub files, since type checkers automatically treat stubs as having those semantics.
- [x] `Y045` `__iter__` methods should never return Iterable[T], as they should always return some kind of iterator.
- [x] `Y046` A private Protocol should be used at least once in the file in which it is defined.
- [x] `Y047` A private TypeAlias should be used at least once in the file in which it is defined.
- [x] `Y048` Function bodies should contain exactly one statement. (Note that if a function body includes a docstring, the docstring counts as a "statement".)
- [x] `Y049` A private TypedDict should be used at least once in the file in which it is defined.
- [x] `Y050` Prefer typing_extensions.Never over typing.NoReturn for argument annotations. This is a purely stylistic choice in the name of readability.
- [x] `Y051` Y051 detect redundant unions between Literal types and builtin supertypes. For example, Literal[5] is redundant in the union int | Literal[5], and Literal[True] is redundant in the union Literal[True] | bool.
- [x] `Y052` Y052 disallows assignments to constant values where the assignment does not have a type annotation. For example, `x = 0` in the global namespace is ambiguous in a stub, as there are four different types that could be inferred for the variable `x`: `int`, `Final[int]`, `Literal[0]`, or `Final[Literal[0]]`. Enum members are excluded from this check, as are various special assignments such as `__all__` and `__match_args__`.
- [x] `Y053` Only string and bytes literals <=50 characters long are permitted.
- [x] `Y054` Only numeric literals with a string representation <=10 characters long are permitted.
- [x] `Y055` Unions of the form `type[X] \| type[Y]` can be simplified to `type[X \| Y]`. Similarly, `Union[type[X], type[Y]]` can be simplified to `type[Union[X, Y]]`.
- [x] `Y056` Do not call methods such as `.append()`, `.extend()` or `.remove()` on `__all__`. Different type checkers have varying levels of support for calling these methods on `__all__`. Use `+=` instead, which is known to be supported by all major type checkers.

---

_Comment by @SigureMo on 2022-11-21 12:14_

Flake8-pyi currently only works with `.pyi` files (https://github.com/PyCQA/flake8-pyi/issues/118), it would be great if Ruff could support `.py` files.

Flake8-pyi author list some rules in https://github.com/qutebrowser/qutebrowser/issues/7098#issuecomment-1109959035 that can be easily apply to `.py` files 

In addition, there are some rules that overlap with pyupgrade, such as Y037 and U007.

---

_Label `rule` added by @charliermarsh on 2022-11-21 14:49_

---

_Label `rule` removed by @charliermarsh on 2022-12-31 18:18_

---

_Label `plugin` added by @charliermarsh on 2022-12-31 18:18_

---

_Referenced in [astral-sh/ruff#2682](../../astral-sh/ruff/pulls/2682.md) on 2023-02-09 03:53_

---

_Comment by @charliermarsh on 2023-02-10 00:03_

These are being implemented under the `PYI` prefix (we tend to avoid single-letter prefixes like `Y`, since they lead to conflicts).

---

_Referenced in [astral-sh/ruff#2717](../../astral-sh/ruff/issues/2717.md) on 2023-02-10 16:03_

---

_Referenced in [astral-sh/ruff#2805](../../astral-sh/ruff/pulls/2805.md) on 2023-02-12 10:18_

---

_Referenced in [astral-sh/ruff#3230](../../astral-sh/ruff/pulls/3230.md) on 2023-02-26 00:17_

---

_Comment by @sbdchd on 2023-02-26 03:06_

Since opening this issue Y014 and Y011 have changed their behavior to allow some defaults, do you think we should mirror the new behavior?

https://github.com/PyCQA/flake8-pyi/pull/326
https://github.com/PyCQA/flake8-pyi/issues/316

---

_Comment by @charliermarsh on 2023-02-26 03:23_

@sbdchd - Yeah I'd vote to mirror their behavior.

---

_Referenced in [astral-sh/ruff#3238](../../astral-sh/ruff/pulls/3238.md) on 2023-02-26 16:58_

---

_Referenced in [astral-sh/ruff#3291](../../astral-sh/ruff/pulls/3291.md) on 2023-03-01 11:18_

---

_Comment by @fschulze on 2023-03-06 07:01_

I second @SigureMo, it would be great if some rules like ``Y006`` would apply to ``*.py`` files as well.

---

_Referenced in [astral-sh/ruff#3922](../../astral-sh/ruff/pulls/3922.md) on 2023-04-09 04:01_

---

_Referenced in [astral-sh/ruff#4211](../../astral-sh/ruff/pulls/4211.md) on 2023-05-03 15:43_

---

_Referenced in [astral-sh/ruff#4214](../../astral-sh/ruff/pulls/4214.md) on 2023-05-03 20:55_

---

_Referenced in [astral-sh/ruff#4246](../../astral-sh/ruff/issues/4246.md) on 2023-05-15 02:15_

---

_Referenced in [astral-sh/ruff#4517](../../astral-sh/ruff/pulls/4517.md) on 2023-05-19 04:24_

---

_Referenced in [astral-sh/ruff#4695](../../astral-sh/ruff/pulls/4695.md) on 2023-05-28 17:14_

---

_Referenced in [astral-sh/ruff#4700](../../astral-sh/ruff/pulls/4700.md) on 2023-05-29 04:39_

---

_Referenced in [astral-sh/ruff#4756](../../astral-sh/ruff/pulls/4756.md) on 2023-05-31 14:08_

---

_Referenced in [astral-sh/ruff#4764](../../astral-sh/ruff/pulls/4764.md) on 2023-05-31 19:07_

---

_Referenced in [astral-sh/ruff#4770](../../astral-sh/ruff/pulls/4770.md) on 2023-05-31 22:37_

---

_Referenced in [astral-sh/ruff#4775](../../astral-sh/ruff/pulls/4775.md) on 2023-06-01 03:52_

---

_Referenced in [astral-sh/ruff#4791](../../astral-sh/ruff/pulls/4791.md) on 2023-06-01 17:25_

---

_Referenced in [astral-sh/ruff#4820](../../astral-sh/ruff/pulls/4820.md) on 2023-06-02 23:47_

---

_Referenced in [astral-sh/ruff#4851](../../astral-sh/ruff/pulls/4851.md) on 2023-06-05 01:50_

---

_Referenced in [astral-sh/ruff#4884](../../astral-sh/ruff/pulls/4884.md) on 2023-06-06 00:19_

---

_Referenced in [astral-sh/ruff#5021](../../astral-sh/ruff/pulls/5021.md) on 2023-06-12 09:20_

---

_Referenced in [astral-sh/ruff#5453](../../astral-sh/ruff/pulls/5453.md) on 2023-06-30 12:56_

---

_Referenced in [astral-sh/ruff#5457](../../astral-sh/ruff/pulls/5457.md) on 2023-07-01 19:55_

---

_Comment by @zanieb on 2023-07-06 17:35_

I'm implementing PYI030

---

_Referenced in [astral-sh/ruff#5570](../../astral-sh/ruff/pulls/5570.md) on 2023-07-06 19:01_

---

_Comment by @density on 2023-07-09 17:56_

I am implementing `PYI036`

---

_Referenced in [astral-sh/ruff#5668](../../astral-sh/ruff/pulls/5668.md) on 2023-07-11 01:57_

---

_Comment by @density on 2023-07-13 01:57_

I'm implementing PYI041

---

_Referenced in [astral-sh/ruff#5722](../../astral-sh/ruff/pulls/5722.md) on 2023-07-13 15:05_

---

_Comment by @LaBatata101 on 2023-07-14 19:57_

I'm implementing `PYI026`

---

_Referenced in [astral-sh/ruff#5773](../../astral-sh/ruff/issues/5773.md) on 2023-07-15 09:19_

---

_Referenced in [astral-sh/ruff#5844](../../astral-sh/ruff/pulls/5844.md) on 2023-07-17 22:52_

---

_Comment by @qdegraaf on 2023-07-19 19:00_

I'm implementing PYI017

---

_Referenced in [astral-sh/ruff#5895](../../astral-sh/ruff/pulls/5895.md) on 2023-07-19 19:25_

---

_Comment by @LaBatata101 on 2023-07-21 20:08_

I'm implementing `PYI056`

---

_Referenced in [astral-sh/ruff#5959](../../astral-sh/ruff/pulls/5959.md) on 2023-07-21 22:42_

---

_Comment by @LaBatata101 on 2023-07-22 16:54_

I'm implementing PYI018 and PYI046, PYI047, PYI049 (which are similar to PYI018)

---

_Referenced in [astral-sh/ruff#6018](../../astral-sh/ruff/pulls/6018.md) on 2023-07-24 23:00_

---

_Referenced in [astral-sh/ruff#6098](../../astral-sh/ruff/pulls/6098.md) on 2023-07-26 15:13_

---

_Referenced in [astral-sh/ruff#6134](../../astral-sh/ruff/pulls/6134.md) on 2023-07-27 21:02_

---

_Referenced in [astral-sh/ruff#6136](../../astral-sh/ruff/pulls/6136.md) on 2023-07-27 22:01_

---

_Comment by @LaBatata101 on 2023-07-27 23:23_

I'm implementing `PYI051`

---

_Comment by @qdegraaf on 2023-07-31 12:22_

I'm implementing `PYI019`

---

_Referenced in [astral-sh/ruff#6204](../../astral-sh/ruff/pulls/6204.md) on 2023-07-31 17:18_

---

_Referenced in [astral-sh/ruff#6215](../../astral-sh/ruff/pulls/6215.md) on 2023-08-01 13:22_

---

_Comment by @LaBatata101 on 2023-08-02 03:23_

I'm implementing `PYI055`

---

_Referenced in [astral-sh/ruff#6316](../../astral-sh/ruff/pulls/6316.md) on 2023-08-03 20:07_

---

_Comment by @LaBatata101 on 2023-08-03 21:36_

I'm implementing `PYI023`

---

_Referenced in [astral-sh/ruff#6354](../../astral-sh/ruff/pulls/6354.md) on 2023-08-04 22:25_

---

_Comment by @charliermarsh on 2023-08-04 23:00_

Done! Thanks everyone!

---

_Closed by @charliermarsh on 2023-08-04 23:00_

---
