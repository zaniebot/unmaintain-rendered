```yaml
number: 2321
title: "SyntaxError in Python 3.11 when unpacking sequences inside of []"
type: issue
state: closed
author: ned2
labels:
  - bug
assignees: []
created_at: 2023-01-29T09:45:27Z
updated_at: 2023-02-23T13:44:12Z
url: https://github.com/astral-sh/ruff/issues/2321
synced_at: 2026-01-12T15:54:42Z
```

# SyntaxError in Python 3.11 when unpacking sequences inside of []

---

_@ned2_

Ruff version: 0.0.237
Python version: 3.11.1

Command run:

    ruff check --isolated code.py

Contents of `code.py`:

```
my_dict = {}
my_dict[*"ab"] = 1
```

This is valid Python 3.11 code, with the string `'ab'` being unpacked into a tuple, resulting the in they key with tuple `('a','b')` being assigned a value of `1`. The above invocation of ruff however throws this error:

    E999 SyntaxError: invalid syntax. Got unexpected token '*'

The snippet itself doesn't work in Python 3.9 or 3.10 (throwing SyntaxErrors), so this seems like a syntax enhancement in 3.11 that should be supported. That said, I can't find where this is documented as a change in 3.11. All I can see is [this](https://docs.python.org/3/whatsnew/3.11.html#other-language-changes ) reference to _Starred unpacking expressions can now be used in for statements._ , which references this [CPython issue](https://github.com/python/cpython/issues/90881), but that looks to be a different context for using `*`, and was only documenting behaviour that changed in 3.9. 

---

_Comment by @ned2 on 2023-01-29 09:46_

Oh just occurred to me, would this be better submitted to the RustPython project?

---

_Comment by @charliermarsh on 2023-01-29 16:31_

Oh interesting! Yes, I think this would be at-home on RustPython. Do you mind filing there?

---

_Label `bug` added by @charliermarsh on 2023-01-29 16:31_

---

_Comment by @ned2 on 2023-01-31 09:00_

Done: https://github.com/RustPython/RustPython/issues/4479

As I mention in that issue, I wonder if it's related to the new `except*` syntax that was introduced in 3.11 for `ExceptionGroup`s. The change I picked up on above doesn't seem to be documented, whereas `except*` is - maybe it's a side-effect of the change.

Also, maybe this issue should then should really be about the fact that ruff doesn't support the `except*` syntax of 3.11 required to use `ExceptionGroup`s, which is going to be more impactful to users.   

---

_Comment by @ned2 on 2023-01-31 10:31_

Ok so it turns out this is an expected change in 3.11, but it wasn't called out in the changelog. See the section [Change 1: Star Expressions in Indexes](https://peps.python.org/pep-0646/#change-1-star-expressions-in-indexes) in PEP 646, which was accepted for 3.11.

---

_Comment by @youknowone on 2023-02-21 17:49_

The parser parts are resolved
https://github.com/RustPython/RustPython/pull/4531
https://github.com/RustPython/RustPython/pull/4532

---

_Closed by @charliermarsh on 2023-02-21 18:42_

---

_Comment by @LeeeeT on 2023-02-23 07:53_

@charliermarsh, I'm still getting an error when trying to annotate `*args` as a type variable tuple:

```python
def f(*args: *tuple[int]) -> None: ...
```
```
error: Failed to parse file.py: invalid syntax. Got unexpected token '*' at line 1 column 14
file.py:1:15: E999 SyntaxError: invalid syntax. Got unexpected token '*'
```

Although, this is a valid syntax according to [PEP 646](https://peps.python.org/pep-0646/#args-as-a-type-variable-tuple), and it passes static type checking using [Pyright](https://github.com/microsoft/pyright).

**ruff 0.0.252**

---

_Comment by @LeeeeT on 2023-02-23 08:00_

Moreover, Ruff seems not to support [exception groups](https://peps.python.org/pep-0654).

```python
print(ExceptionGroup)
```
```
file.py:1:7: F821 Undefined name `ExceptionGroup`
```

Should I create a separate issue for this?

---

_Comment by @charliermarsh on 2023-02-23 12:27_

Thanks @LeeeeT, can you file a separate issue for the `*tuple` thing?

---

_Comment by @charliermarsh on 2023-02-23 13:39_

@LeeeeT - Nevermind, I filed as https://github.com/charliermarsh/ruff/issues/3170. `ExceptionGroup` was fixed via #3167.

---

_Comment by @LeeeeT on 2023-02-23 13:44_

Thank you for fixing it so quickly! ❤️

---
