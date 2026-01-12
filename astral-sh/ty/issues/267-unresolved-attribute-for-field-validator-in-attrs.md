```yaml
number: 267
title: "`unresolved-attribute` for `@<field>.validator` in `attrs` class"
type: issue
state: open
author: my1e5
labels:
  - dataclasses
  - attribute access
  - library
assignees: []
created_at: 2025-05-08T11:02:31Z
updated_at: 2025-11-14T14:56:52Z
url: https://github.com/astral-sh/ty/issues/267
synced_at: 2026-01-12T15:54:22Z
```

# `unresolved-attribute` for `@<field>.validator` in `attrs` class

---

_@my1e5_

### Summary

Reporting this here to understand if this is something that needs sorting on the `attrs` library side.

`attrs` allows you to define a validator for an attribute using a decorator (`@<field>.validator`)

```py
# /// script
# requires-python = ">=3.13"
# dependencies = [
#     "attrs",
# ]
# ///

from typing import Any

from attrs import define, field


@define(kw_only=True)
class Foo:
    name: str = field(default="")
    age: int = field(default=0)

    @age.validator
    def check_age(self, _: Any, value: int) -> None:
        if value < 0:
            raise ValueError("Age must be a positive integer.")


if __name__ == "__main__":
    foo = Foo(age=3)
    foo.age = -2  # This will raise a ValueError
```
### ty 
```
$ uvx --with attrs==25.3.0 ty check foo.py 
Installed 2 packages in 51ms
error: lint:unresolved-attribute: Type `Literal[0]` has no attribute `validator`
  --> foo.py:18:6
   |
16 |     age: int = field(default=0)
17 |
18 |     @age.validator
   |      ^^^^^^^^^^^^^
19 |     def check_age(self, _: Any, value: int) -> None:
20 |         if value < 0:
   |
info: `lint:unresolved-attribute` is enabled by default

Found 1 diagnostic
```
### mypy
```
$ uvx --with attrs mypy --strict foo.py
Success: no issues found in 1 source file
```

### Version

ty 0.0.0-alpha.7 (905a3e1e5 2025-05-07)

---

_Comment by @sharkdp on 2025-05-08 11:05_

Thank you for reporting this.

It probably falls under the more general point *"Support for [dataclass fields](https://docs.python.org/3/library/dataclasses.html#dataclasses.field)"* here: https://github.com/astral-sh/ty/issues/111.

---

_Label `dataclasses` added by @AlexWaygood on 2025-05-10 17:59_

---

_Label `attribute access` added by @AlexWaygood on 2025-05-11 08:06_

---

_Comment by @abhijeetbodas2001 on 2025-06-10 13:19_

I don't think better support for `fields` will remove this diagnostic. `mypy` does not give a diagnostic for this, because it does not do any type checking for `validator` at all, if used as a decorator:

https://github.com/python/mypy/blob/fe91422e56e38c0ec67fccdd5f589dbeb03f3b52/mypy/plugins/attrs.py#L591-L600

`pyright` does not do this special-casing, and does give a diagnostic for the above example:


```bash
(code) [~/code/mypy] $ bat /tmp/foo.py
───────┬────────────────────────────────────────────────────────────────────────────────────────
       │ File: /tmp/foo.py
───────┼────────────────────────────────────────────────────────────────────────────────────────
   1   │ from attrs import define, field
   2   │
   3   │
   4   │ @define(kw_only=True)
   5   │ class Foo:
   6   │     age: int = field(default=0)
   7   │
   8   │     @age.validator
   9   │     def check_age(self, _, value: int) -> None:
  10   │         ...
───────┴────────────────────────────────────────────────────────────────────────────────────────
(code) [~/code/mypy] $ mypy /tmp/foo.py
Success: no issues found in 1 source file
(code) [~/code/mypy] $ pyright /tmp/foo.py
/tmp/foo.py
  /tmp/foo.py:8:10 - error: Cannot access attribute "validator" for class "int"
    Attribute "validator" is unknown (reportAttributeAccessIssue)
1 error, 0 warnings, 0 informations
```

---

_Comment by @carljm on 2025-06-10 15:32_

It looks to me like both pyright's diagnostic and ours are based on assuming that `age` in the class scope is an `int` (as annotated) rather than a `field` object. So it does look to me like proper support for `field` objects should fix this.

---

_Comment by @abhijeetbodas2001 on 2025-06-10 15:49_

Even `mypy` infers `age` as an `int` in the class scope. And I believe that is not wrong, since the matching `field` overload has been annotated as returning the type of `default`?

```py
# This form catches an explicit default argument.
@overload
def field(
    *,
    default: _T,
    validator: _ValidatorArgType[_T] | None = ...,
    # more args
) -> _T: ...
```



---

_Comment by @AlexWaygood on 2025-06-10 15:52_

I don't know if it's relevant here or not, but note that mypy actually has a plugin for `attrs` that is bundled as part of mypy itself (you don't need to `(uv) pip install` it separately like you do for most mypy plugins): https://github.com/python/mypy/blob/master/mypy/plugins/attrs.py

---

_Comment by @abhijeetbodas2001 on 2025-06-10 15:55_

Yup! The plugin is what is I believe doing the magic of ignoring the `age.validator` decorator to avoid the error (linked from my first comment).

---

_Comment by @erictraut on 2025-06-10 15:57_

If you change your code to the following, then no special magic is required to type check it, and it will work with all conformant type checkers. 

```python
    def check_age(self, _: Any, value: int) -> None:
        ...
    age: int = field(default=0, validator=check_age)
```

Even with mypy, this approach is probably preferable because the signature of `check_age` is type checked.

---

_Comment by @my1e5 on 2025-06-24 13:07_

This looks like it's solved with `ty 0.0.1-alpha.11 (1ae703836 2025-06-17)`? Or is this a false positive?
## `0.0.1a10`
```
$ uvx --with attrs==25.3.0 --with ty==0.0.1a10 ty check dev/foo.py 
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unresolved-attribute]: Type `Literal[0]` has no attribute `validator`
  --> dev\foo.py:11:6
   |
 9 |     age: int = field(default=0)
10 |
11 |     @age.validator
   |      ^^^^^^^^^^^^^
12 |     def check_age(self, _: Any, value: int) -> None:
13 |         if value < 0:
   |
info: rule `unresolved-attribute` is enabled by default

Found 1 diagnostic
```
## `0.0.1a11`
```
$ uvx --with attrs==25.3.0 --with ty==0.0.1a11 ty check dev/foo.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
All checks passed!
```

---

_Comment by @abhijeetbodas2001 on 2025-06-25 16:36_

We used to infer the type of `field(default=0)` as `Literal[0]` earlier, but sometime recently, we started inferring it as `Unknown`. This silenced the error. I bisected this to https://github.com/astral-sh/ruff/pull/18607.

cc @dhruvmanila

---

_Comment by @dhruvmanila on 2025-06-26 03:20_

Thanks for bisecting the issue! I think that's due to https://github.com/astral-sh/ty/issues/669. The third overload for `attrs.field` has the type of `default` as `_T` which is the return type but we're checking whether the non-generic argument type `Literal[0]` is assignable to `_T` when performing the overload call evaluation.

Edit: I've provided a possible solution to this issue over at #669 

---

_Comment by @abhijeetbodas2001 on 2025-08-20 04:27_

The error is back :)

```shell
(code) [~/code/ruff] $ uvx --with ty==0.0.1a19 ty check /tmp/foo.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                               All checks passed!
(code) [~/code/ruff] $ ./target/debug/ty check /tmp/foo.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                               error[unresolved-attribute]: Type `Literal[0]` has no attribute `validator`
  --> /tmp/foo.py:11:6
   |
 9 |     age: int = field(default=0)
10 |
11 |     @age.validator
   |      ^^^^^^^^^^^^^
12 |     def check_age(self, _: Any, value: int) -> None:
13 |         if value < 0:
   |
info: rule `unresolved-attribute` is enabled by default

Found 1 diagnostic
```

---

_Comment by @sharkdp on 2025-08-20 06:16_

> The error is back :)

You are fast! It changed with the recently merged https://github.com/astral-sh/ruff/pull/19964 by @dhruvmanila. Previously, `age` was inferred as `Unknown`, and so there was no error.

Now we infer `Literal[0]` for `age`, which is correct, according to [this overload of `field`](https://github.com/python-attrs/attrs/blob/19175d91df292e9d0722ecb6575043f1d124a123/src/attrs/__init__.pyi#L112-L133). And consequently, we get *"Type `Literal[0]` has no attribute `validator`"*.

For the standard library / typeshed `field` function, we [special-case the return type of the `field` function](https://github.com/astral-sh/ruff/blob/f019cfd15f8947b95ab0f200551d14558705f6e7/crates/ty_python_semantic/src/types/call/bind.rs#L951-L962) to return a `dataclasses.Field` instance, instead of the type of the default argument. To resolve the problem here, we will need to do the same for dataclass transformers and [their `field_specifiers`](https://github.com/python-attrs/attrs/blob/19175d91df292e9d0722ecb6575043f1d124a123/src/attrs/__init__.pyi#L158).

Thank you for reporting this (again)

---

_Comment by @my1e5 on 2025-09-03 16:36_

With the release of ty 0.0.1-alpha.20 (f41f00af1 2025-09-03), I've noticed that if `field()` doesn't contain the `default` parameter then there is no error raised. But if `default` is present (as it is in the original minimal example) then we get the error.

```py
from typing import Any

from attrs import define, field


@define
class Foo:
    bar: int = field(default=0)
    baz: int = field()

    @bar.validator
    def check_bar(self, _: Any, value: int) -> None:
        if value < 0:
            raise ValueError("Bar must be a positive integer.")

    @baz.validator
    def check_baz(self, _: Any, value: int) -> None:
        if value < 0:
            raise ValueError("Baz must be a positive integer.")
```
```
$ uvx --with attrs==25.3.0 --with ty==0.0.1a20 ty check dev/foo.py 
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files
error[unresolved-attribute]: Type `Literal[0]` has no attribute `validator`
  --> dev\foo.py:11:6
   |
 9 |     baz: int = field()
10 |
11 |     @bar.validator
   |      ^^^^^^^^^^^^^
12 |     def check_bar(self, _: Any, value: int) -> None:
13 |         if value < 0:
   |
info: rule `unresolved-attribute` is enabled by default

Found 1 diagnostic
```
Note how only `bar.validator` is an issue. `baz.validator` is fine.

---

_Comment by @sharkdp on 2025-09-04 11:52_

> With the release of ty 0.0.1-alpha.20 ([f41f00a](https://github.com/astral-sh/ty/commit/f41f00af15e7c21384e7abf05a9da3364a15d3f7) 2025-09-03), I've noticed that if `field()` doesn't contain the `default` parameter then there is no error raised. But if `default` is present (as it is in the original minimal example) then we get the error.

I haven't checked this in detail, but I would imagine it might be related to #1068.

---

_Label `library` added by @carljm on 2025-11-14 14:56_

---

_Added to milestone `Stable` by @carljm on 2025-11-14 14:56_

---
