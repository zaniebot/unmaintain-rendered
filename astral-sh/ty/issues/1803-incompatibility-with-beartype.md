---
number: 1803
title: Incompatibility with beartype
type: issue
state: closed
author: adamtheturtle
labels: []
assignees: []
created_at: 2025-12-08T09:08:30Z
updated_at: 2026-01-02T07:33:46Z
url: https://github.com/astral-sh/ty/issues/1803
synced_at: 2026-01-10T01:51:14Z
---

# Incompatibility with beartype

---

_Issue opened by @adamtheturtle on 2025-12-08 09:08_

### Summary

```python
from beartype import beartype


@beartype
def my_func() -> str:
    return "Hello, world!"


my_func()
```

Command:

```
uv run --with="beartype==0.22.8" --with="ty==0.0.1a32" ty check example.py
```

This errors with:

```
error[missing-argument]: No argument provided for required parameter 1
 --> example.py:9:1
  |
9 | my_func()
  | ^^^^^^^^^
  |
info: Union variant `(Unknown, /) -> Unknown` is incompatible with this call site
info: Attempted to call union type `(def my_func() -> str) | Unknown | ((Unknown, /) -> Unknown)`
info: rule `missing-argument` is enabled by default
```

`mypy` and `pyright` both pass.

### Version

ty 0.0.1-alpha.32 (84a188116 2025-12-05)

---

_Comment by @carljm on 2025-12-08 19:35_

[`beartype/__init__.py` imports the `beartype` decorator from `beartype._decor.decormain`](https://github.com/beartype/beartype/blob/main/beartype/__init__.py#L158-L159)

`decormain.py` initially defines it using [these overloads](https://github.com/beartype/beartype/blob/main/beartype/_decor/decormain.py#L51-L55) meant for static type checkers

But then later in the same module it [conditionally](https://github.com/beartype/beartype/blob/main/beartype/_decor/decormain.py#L75) overwrites it with [this no-op definition](https://github.com/beartype/beartype/blob/main/beartype/_decor/decormain.py#L80-L86), or else with [this import](https://github.com/beartype/beartype/blob/main/beartype/_decor/decormain.py#L140) [from `beartype._decor._decorcache`](https://github.com/beartype/beartype/blob/main/beartype/_decor/decorcache.py#L48-L55).

No type-checker can statically decide whether `is_python_optimized()` is true or false, so all type checkers consider both the `if` and `else` paths to be possible.

All four type checkers treat the `if` path the same. The `def beartype` there is treated as the implementation of the overloads above, and thus effectively ignored for static typing purposes.

Mypy and pyright do not support conditional re-definition of a name. Thus, in the `else` path, mypy and pyright error on the attempt to "redefine" `beartype` via import, and fallback to ignoring it. (The type error is silenced on that import line with `# type: ignore`.)

Ty (and pyrefly) support conditional re-definition of names, so we do not error on that import, and treat the type of `beartype` as a union of the overloads above, and the imported version of the function from `decorcache.py` (which is not designed to be visible to static type checkers). It's that imported version of `beartype` that then causes the problem when it is used.

I don't see this as a bug in ty (or pyrefly). Nothing in the typing spec prohibits type checkers from supporting conditional re-definition, and the conclusions that ty reaches here are plainly accurate: at runtime, `beartype` can be the imported version of the function from `decorcache.py`. Beartype is relying on how mypy and pyright happen to handle a type error, which doesn't seem likely to be a reliable behavior even for pyright and mypy, let alone for other type checkers.

If beartype wants to reliably hide that second definition of the `beartype` decorator from static type checkers, it should use `if not TYPE_CHECKING:`, or define the static-type-checker version in a stub file that entirely hides the real implementation, or add the overloads to `decorcache.py` also, so the imported version has the same overloads.

---

_Closed by @carljm on 2025-12-08 19:35_

---

_Comment by @adamtheturtle on 2025-12-08 19:55_

Thank you @carljm for looking into this. Hopefully this is something that can be resolved within `beartype`.

---

_Comment by @leycec on 2025-12-10 07:53_

**Ah-ha!** The wonderful @beartype collaborator @Glinte has [since resolved this on the @beartype side](https://github.com/beartype/beartype/pull/600). We praise his name. 🥳 

That said...

> No type-checker can statically decide whether `is_python_optimized()` is true or false...

**Ah-ha!** The opposite is true, actually. It's just a matter of will, grit, determination, and the desire to suffer. Technically, **absolutely all pure static type-checkers can statically decide whether `is_python_optimized()` is `True` or `False`.** Why? Because this is the relevant implementation of the `is_python_optimized()` function (omitting ignorable docstrings and comments):

```python
def is_python_optimized() -> bool:
    if (
        TYPE_CHECKING or
        not __debug__
    ):
        return True

    ...
```

**This is a `ty` bug.** That's just a hard fact. `ty` *absolutely* should know that `is_python_optimized()` unconditionally evaluates to `True`. In fact, **mypy and pyright both do just that.** They correctly deduce via static analysis alone that `is_python_optimized()` unconditionally evaluates to `True`. It can't be *that* hard. You just need to propagate `if TYPE_CHECKING ...:` checks across function boundaries, which is something you want to do sooner or later anyway. Ideally, sooner.

This issue was unwisely closed prematurely and should be reopened. At a bare minimum, `ty` needs to hit feature parity with mypy and pyright by exhibiting similar intelligence. Obviously, `ty` isn't Turing-complete. It's a parser. But there's a considerable amount of intelligence you can stuff into the narrow confines of parsing.

Mypy and pyright both exhibit that intelligence. So should `ty`. I shrug. I shrug so hard, fam. `</shrug>`

---

_Comment by @AlexWaygood on 2025-12-10 08:08_

I'm very glad to hear that this issue has been resolved for beartype users!

> `ty` _absolutely_ should know that `is_python_optimized()` unconditionally evaluates to `True`. In fact, **mypy and pyright both do just that.** They correctly deduce via static analysis alone that `is_python_optimized()` unconditionally evaluates to `True`.

_Do_ they?
- mypy: https://mypy-play.net/?mypy=latest&python=3.12&gist=6b77d9a0fd1cd31aeef337080608b304
- Pyright: https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAqAmgAoCiA%2BgMIASZVA0gJIByA4gFAcAmApsJgDOFBHBgALMCgpgEMLEgBevbgAoAlFAC0APigAjMGAA2ALg5RLmAaotX7xctTqNWbKLjv3LKMPgoUfPoArmgBXlDq5t5WILwwwSAohCDBvFwcSAJIwqISUjJyCspqUREAHlAAvFAALBy8xoK80VaVNQBEtR1cWVC%2B%2BDkiYpLSsvIQSioarZZw1XUNTS0R853dERxxAG68AIbGFPAIvKrl6lu8uwdHiKdwF0A

---

_Comment by @sharkdp on 2025-12-10 08:09_

(Edit: oops, Alex was faster)

> `ty` _absolutely_ should know that `is_python_optimized()` unconditionally evaluates to `True`. In fact, **mypy and pyright both do just that.**

I don't think that is true. If we consider a program like the following, then all of pyright, mypy, pyrefly and ty understand that the type of `a` can only be `None`.
```py
from typing import TYPE_CHECKING, reveal_type


def is_python_optimized() -> bool:
    if TYPE_CHECKING or not __debug__:
        return True
    else:
        return False


a: int | None
a = 1

if True:
    a = None

reveal_type(a)  # all type checkers reveal `None` here
```

However, if we replace the `if True` with `if is_python_optimized()`, then all of these type checkers reveal a type of either `Literal[1] | None` or `int | None`.

---

_Comment by @Glinte on 2025-12-10 08:15_

@leycec Not gonna lie, I think you have a decent bit of knowledge gap on static type checkers, so you may want to be a bit more careful than usual when talking about them.

---

_Comment by @sharkdp on 2025-12-10 08:16_

To be fair, it's perfectly reasonable to think that a type checker should infer a return type of `Literal[True]` for this function. But in that case, the `-> bool` return type annotation would have to be removed (otherwise this is an explicit request for the type checker to widen the type). pyright can do this:
```py
from typing import TYPE_CHECKING

def is_python_optimized():
    if (
        TYPE_CHECKING or
        not __debug__
    ):
        return True

    return False

reveal_type(is_python_optimized())  # pyright infers `Literal[True]` here
```

And ty should also be able to do this once https://github.com/astral-sh/ruff/pull/17371 lands.

---

_Comment by @adamtheturtle on 2026-01-02 07:33_

Beartype has been updated and now works with ty.

---
