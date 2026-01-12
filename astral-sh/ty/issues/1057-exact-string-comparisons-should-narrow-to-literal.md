```yaml
number: 1057
title: "Exact string comparisons should narrow to `Literal`"
type: issue
state: closed
author: oliverlambson
labels:
  - question
assignees: []
created_at: 2025-08-20T04:06:56Z
updated_at: 2025-09-09T14:33:05Z
url: https://github.com/astral-sh/ty/issues/1057
synced_at: 2026-01-12T15:54:24Z
```

# Exact string comparisons should narrow to `Literal`

---

_@oliverlambson_

### Summary

When a string variable is compared against string literals in ways that constrain its possible values, the type checker should narrow the variable's type to the appropriate `Literal` type within the conditional block.

This should work for:

- Equality comparisons: `s == "value"`
- Membership tests: `s in ["value1", "value2"]`
- Other comparisons that resolve to specific literal values

### Minimal reproduction

String variables retain their `str` type even after comparisons that logically constrain their possible values, causing type errors when passing them to functions expecting literal types.

```python
from typing import Literal


def process_string(s: str) -> None:
    if s == "something":
        use_something(s)


def use_something(_s: Literal["something"]) -> None: ...
```

```
$ uvx ty check narrow_string.py
error[invalid-argument-type]: Argument to function `use_something` is incorrect
 --> narrow_string.py:6:23
  |
4 | def process_string(s: str) -> None:
5 |     if s == "something":
6 |         use_something(s)
  |                       ^ Expected `Literal["something"]`, found `str`
```

### Other type checkers

Within the true branch above, using `reveal_type(s)`:

- basedpyright resolves to `Literal["something"]`
- mypy resolves to `builtins.str`

### Version

ty 0.0.1-alpha.19 (e9cb838b3 2025-08-19)

---

_Label `question` added by @sharkdp on 2025-08-20 06:21_

---

_Comment by @sharkdp on 2025-08-20 06:27_

Thank you for reporting this.

The proposed narrowing is not sound. `str` can be subclassed, and subclasses could theoretically overload `__eq__`:
```py
class Problematic(str):
    def __eq__(self, other):
        return True

process_string(Problematic("oh"))
```
For this reason, we currently don't narrow the type of `s` in that conditional.

It doesn't help for your example, but note that narrowing *is* sound in the negative `!=` case:
```py
def process_string(s: str) -> None:
    if s != "something":
        reveal_type(s)  # str & ~Literal["something"]
```

The positive case would also work if `process_string` takes a `LiteralString`, but I don't know if that's an option for you:
```py
def process_string(s: LiteralString) -> None:
    if s == "something":
        reveal_type(s)  # Literal["something"]
```

---

_Comment by @oliverlambson on 2025-08-20 15:43_

Ah I hadn't thought about the operator overloading, thanks!

---

_Closed by @oliverlambson on 2025-08-20 15:43_

---

_Comment by @evtn on 2025-09-08 05:51_

**upd: nevermind, seems like `str.__eq__` bails out if the class of rhs is not exactly str, so `s.__eq__` could be called here anyway**

Would it make sense to narrow in an inversed case? (at the time of writing it is not narrowed)

```py
def process_string(s: str) -> None:
    if "something" == s:
        use_something(s)
```

I assume that would require some changes for `LiteralString.__eq__` signature, but that does look sound

---

_Comment by @carljm on 2025-09-08 19:15_

Other type checkers (e.g. pyright) also don't support narrowing in these reversed "Yoda conditions". In principle I would like to support it, but it's not high priority. Feel free to open a separate issue for it!

---

_Comment by @evtn on 2025-09-09 14:28_

Well, it's weird, but:

```
>>> class P(str):
...     def __eq__(self, other):
...         return True
...         
>>> "123".__eq__(P("321"))
False
>>> "123" == P("321")
True
>>> class P2(str):
...     def __eq__(self, other):
...         return NotImplemented
...         
>>> "123" == P2("435")
False
>>> "123".__eq__(P2("435"))
False
```

I thought the `__eq__` method returned NotImplemented, but it's `False`, and then `==` ignores this and calls `P.__eq__` for some reason? CPython is doing weird stuff here

---

_Comment by @evtn on 2025-09-09 14:33_

Oh I see, that's literally in Python data model spec:

> If the operands are of different types, and the right operand’s type is a direct or indirect subclass of the left operand’s type, the reflected method of the right operand has priority, otherwise the left operand’s method has priority. Virtual subclassing is not considered.

---
