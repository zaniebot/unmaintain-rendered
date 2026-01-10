```yaml
number: 900
title: Unexpected semantic change for stringified annotations
type: issue
state: closed
author: Geo5
labels:
  - question
assignees: []
created_at: 2025-07-26T22:05:47Z
updated_at: 2025-08-04T23:27:14Z
url: https://github.com/astral-sh/ty/issues/900
synced_at: 2026-01-10T02:06:24Z
```

# Unexpected semantic change for stringified annotations

---

_Issue opened by @Geo5 on 2025-07-26 22:05_

### Summary

Consider the following example:
```python
def str(x: str) -> None: ...
def bytes(x: "bytes") -> None: ...

class t: ...
class t2: ...

class Foo:
    complex: complex = 1
    float: "float" = 1.0
    type: type = set

    def int(self, x: int, z: list) -> None: ...
    def slice(self, x: "slice") -> None: ...
    def t(self, x: t) -> None: ...
    def t2(self, x: "t2") -> None: ...
```
[playground](https://play.ty.dev/cfebf8b2-bbc0-4d06-930e-585689a88418)
This has the following errors:
```
Variable of type `def bytes(x: Unknown) -> None` is not allowed in a type expression (invalid-type-form) [Ln 2, Col 15]
Variable of type `float` is not allowed in a type expression (invalid-type-form) [Ln 9, Col 13]
Variable of type `def slice(self, x: Unknown) -> None` is not allowed in a type expression (invalid-type-form) [Ln 13, Col 25]
Variable of type `def t2(self, x: Unknown) -> None` is not allowed in a type expression (invalid-type-form) [Ln 15, Col 22]
```
So it seems like depending on if the annotations are in strings or not they get resolved differently which surprised me for a few reason:
- Mypy is consistent with these (`str` and `bytes` are errors, no errors in `Foo`)
- The same behavior applies when implicitly converting annotations to strings with `from __future__ import annotations` (which could ofc be done by ruff for example)

I think this is somewhat related to #899 , as the code without string annotations does not crash, exemplifying the different behavior as well.

I think there is a test for this in [ruff/crates/ty_python_semantic/resources/mdtest/cycle.md](https://github.com/astral-sh/ruff/blob/c0768dfd96a9432f71149dcb1c64d8fb298ef1a8/crates/ty_python_semantic/resources/mdtest/cycle.md)


Note also that in the LSP, the semantic token highlighter thing does not "agree" with the type resolving, i.e. `x: int` above is not colored as a type like in `z: list`, but as if it was referencing the int method.

### Version

_No response_

---

_Comment by @sharkdp on 2025-07-28 07:20_

Thank you for reporting this.

> So it seems like depending on if the annotations are in strings or not they get resolved differently

This seems to be working as intended. Stringified annotations are always deferred, whereas normal annotations are evaluated eagerly (unless you [are on Python 3.14](https://docs.python.org/3.14/whatsnew/3.14.html#pep-649-and-749-deferred-evaluation-of-annotations) or have `from __future__ import annotations`).

> Mypy is consistent with these (`str` and `bytes` are errors, no errors in `Foo`)

[Pyright also shows an error](https://pyright-play.net/?pythonVersion=3.13&strict=true&enableExperimentalFeatures=true&code=CYUwZgBAzgLgTgCgB4C5rwJQQLQD4IByA9gHYhoB0VAUKJAEYCeMIUyaAREy1B1noVLkIVCtWoBjADYBDKFAgxKNaXIUwATMrGTZ8iADEiRFNQjmIEogFsADlJCpLN%2B44gBeCAEYzFsFKIZJQgOf0CYDg9vCgAGX3MYRlthROSoqBAYcQsIOggASxIYBAypMAAaCCdCmEqALzQpfNh%2BfGIybXjc8GgmiRASkDLKpw4oPpA%2BHDahTpy84tKKqrQYVsEOkRp5ns1B4ZWQzSmBduFRIA) on the `bytes` definition, but seems to handle the annotations in the class differently(?)

---

_Label `question` added by @sharkdp on 2025-07-28 07:20_

---

_Comment by @Geo5 on 2025-07-28 08:40_

I wonder if it should depend on the python version?
Runtime `get_type_hints` on python 3.8 seems to match mypys and pyrights handling:
```python
def str(x: str) -> None: ...
def bytes(x: "bytes") -> None: ...

class t: ...
class t2: ...

class Foo:
    complex: complex = 1
    float: "float" = 1.0
    type: type = set

    def int(self, x: int, z: list) -> None: ...
    def slice(self, x: "slice") -> None: ...
    def t(self, x: t) -> None: ...
    def t2(self, x: "t2") -> None: ...
    
from typing import get_type_hints as g
print(g(str))
print(g(bytes))
print(g(Foo.int))
print(g(Foo.slice))
print(g(Foo.t))
print(g(Foo.t2))
import sys
print(sys.version)
```
```
{'x': <class 'str'>, 'return': <class 'NoneType'>}
{'x': <function bytes at 0x7f6c51b31040>, 'return': <class 'NoneType'>}
{'x': <class 'int'>, 'z': <class 'list'>, 'return': <class 'NoneType'>}
{'x': <class 'slice'>, 'return': <class 'NoneType'>}
{'x': <class '__main__.t'>, 'return': <class 'NoneType'>}
{'x': <class '__main__.t2'>, 'return': <class 'NoneType'>}
3.8.5 (default, Jul 20 2020, 23:11:29) 
[GCC 9.3.0]
```

But maybe i am only making up excuses for my surprise 😄

(I don`t have a newer python version in front of me rn, since 3.8 is eol anyways, but i can image it behaves the same until 3.14)

---

_Comment by @sharkdp on 2025-07-28 09:20_

I would think that something like
```py
float: "float" = 1.0
```
is just fundamentally broken and should be an error. The evaluation of the annotation should be deferred, so it will cyclically depend on the definition of the `float` symbol itself.

On the other hand, if you do the following, this is not treated as an error in ty, because we do allow symbols to be redeclared. So here, you're defining a new float called `float`. This is still problematic, of course, if you later try to use `float` as a type (e.g. `another_float: float = 1.0` would also lead to: "Variable of type `float` is not allowed in a type expression")
```py
float: float = 1.0
```

If you think that something is inconsistent, could you please focus on one example? There are so many outputs, it's hard to tell which one you're talking about.

---

_Comment by @Geo5 on 2025-07-29 20:53_

Yes sorry for making it so convoluted. I was surprised by:
```python
from __future__ import annotations
from typing import get_type_hints, reveal_type
class C:
    def int(self, x: int) -> None:
        reveal_type(x)
print(get_type_hints(C.int)["x"])
```
```sh
> uv run --python 3.14 test.py
<class 'int'>
> uv run --python 3.11 test.py 
<class 'int'>
> uvx ty check test.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[invalid-type-form]: Variable of type `def int(self, x: Unknown) -> None` is not allowed in a type expression
 --> test.py:4:22
  |
2 | from typing import get_type_hints, reveal_type
3 | class C:
4 |     def int(self, x: int) -> None:
  |                      ^^^
5 |         reveal_type(x)
6 | print(get_type_hints(C.int)["x"])
  |
info: rule `invalid-type-form` is enabled by default

info[revealed-type]: Revealed type
 --> test.py:5:21
  |
3 | class C:
4 |     def int(self, x: int) -> None:
5 |         reveal_type(x)
  |                     ^ `Unknown`
6 | print(get_type_hints(C.int)["x"])
  |

Found 2 diagnostics
```
I agree that `float: float = ...` at module level seems always broken. Inside a class scope like in the example i am less sure.

Note that on python 3.14 without the `__future__` import:
```sh
> uv run --python 3.14 test.py
<function C.int at 0x7ffae6c58e00>
```

---

_Comment by @sharkdp on 2025-07-30 18:34_

> Note that on python 3.14 without the `__future__` import:

So.. it looks like not even CPython is consistent on how this should be interpreted? :-) Or otherwise I would expect 3.14 to without the `__future__` import to behave the same as any version with the `__future__` import? Or are the semantics being changed in 3.14?

I don't really know what to do about this. I'm not aware of any resources where it is explicitly specified how names in "deferred evaluation" of annotations should be resolved.

---

_Comment by @jelle-openai on 2025-07-30 18:37_

The scoping semantics should be the same with and without quotes. `get_type_hints()` behavior is not a good guide for what the "right" behavior is; it's buggy and has some weird hacks in it.

---

_Comment by @Geo5 on 2025-07-30 19:19_

> The scoping semantics should be the same with and without quotes. `get_type_hints()` behavior is not a good guide for what the "right" behavior is; it's buggy and has some weird hacks in it.

Would you consider more testing with `annotationlib.get_annotations()` worth it, or is it similarly hacky?

I noticed the discrepancy by using mypy alongside ty in the first place, not by using `typing.get_type_hints( )`.

Maybe its not worth it to think about this edge case too much either way, so if you feel like there is nothing wrong with the behavior then feel free to close the issue! 

---

_Comment by @jelle-openai on 2025-07-30 20:37_

I would expect `get_annotations()` to work better in general, but it doesn't resolve nested stringified annotations, and it may also have bugs. In general, resolving stringified annotations at runtime fully reliable is extremely hard and sometimes impossible.

But I feel strongly that the right behavior is that the name resolution should be the same regardless of whether names are quoted or not.

---

_Comment by @carljm on 2025-08-04 17:05_

Thanks for the report and discussion! Looking at the examples here, it seems to me that our current handling of deferred name resolution for stringified annotations is reasonable and consistent, and also matches the runtime behavior of all annotations in 3.14+ (presuming they aren't accessed "early"), which I think is (in the long term) the most important compatibility.

The principles which I believe we apply (I think I'd consider a counter-example to any of these a bug, but I may be missing some subtlety):

1. String-quoted annotations resolve names as "deferred", meaning later definitions in the same scope (which also includes the "current" definition in cases like `float: "float" = ...`) are considered. This must be true in order for string-quoted annotations to be usable for forward references, which is part of their intended purpose. Thus it is fully expected in general that quoting an annotation can change its interpretation.
2. Name references in string-quoted annotations behave the same as in all annotations when `from __future__ import annotations` is used.
3. Deferred resolution (that is, string-quoting or using `from __future__ import annotations`) doesn't change Python's scoping rules. (This is contrary to the odd behavior of `get_type_hints` in some cases.)

We don't yet support 3.14, but when we do, I'd expect to also add the invariant that our name resolution in annotations in 3.14 should match what we do pre-3.14 when `from __future__ import annotations` is used.

I don't consider `typing.get_type_hints` or any other pre-3.14 implementation of "resolve stringified annotations back to runtime values" to be a useful guide to how stringified annotations should resolve in edge cases. (That means the only runtime behavior I consider to be a useful guide to deferred name resolution is the behavior of 3.14 without `from __future__ import annotations`.)

In general, my advice to anyone running into inconsistent behavior in these edge cases is to avoid shadowing names in the confusing ways that trigger these inconsistencies.

The one thing I see in this thread that sounds like a bug in ty is if our semantic highlighting is inconsistent with our type inference. Might be best to file a new issue specifically about that?

---

_Closed by @carljm on 2025-08-04 17:05_

---

_Comment by @Geo5 on 2025-08-04 23:27_

Thank you for taking the time to do the detailed write-up! I think i do now agree that the current behavior is consistent and your mention of "it should match 3.14+ runtime without stringified..." cleared it up for me. 

I think i also know what irritated me in the first place: mypy just feels a bit more useful to me in this edge case, because it seems to special case functions vs classes:
```python
from __future__ import annotations
from typing import reveal_type

class C:
    def f(self, x: int) -> None: # Should be a forward declaration to C.int
        reveal_type(x)
    def g(self, x: float) -> None: # Should be a forward declaration to C.float
        reveal_type(x)
    class int: ...
    def float(self) -> None: ...
```
```console
$ uvx mypy test.py
test.py:6: note: Revealed type is "test.C.int"
test.py:8: note: Revealed type is "builtins.float"
Success: no issues found in 1 source file
```
i.e. it seems to be deliberately inconsistent and i guess it tries to guess the users meaning. 

---
