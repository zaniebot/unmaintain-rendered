```yaml
number: 16129
title: "`RUF038` Surprising fix for  `Literal[True, False]` in functions with overloads"
type: issue
state: open
author: Geo5
labels:
  - rule
assignees: []
created_at: 2025-02-12T23:17:31Z
updated_at: 2025-02-16T15:01:50Z
url: https://github.com/astral-sh/ruff/issues/16129
synced_at: 2026-01-12T15:54:55Z
```

# `RUF038` Surprising fix for  `Literal[True, False]` in functions with overloads

---

_@Geo5_

### Description

~~I stumbled upon some code of mine where it seems to me that `mypy` and `ruff` do not agree whether `Literal[True, False] == bool` and rule `RUF038` suggest a fix which introduces a mypy error.~~

Edit: I read the docs for the rule again and this general case seems to be mentioned in the "Fix safety" already. Maybe this example is different, but if you feel like it is already covered well enough, feel free to close the issue!

--------------
Consider this code containing two `@overload`s and illustrating perhaps two different problems:
```python
import random
from typing import Literal, overload


class Foo:
    @overload
    def foo(self, *, bar: Literal[True]) -> int: ...

    @overload
    def foo(self, *, bar: Literal[False]) -> float: ...

    def foo(self, *, bar: Literal[True, False]) -> int | float:
        return 0


class FooSub(Foo):
    @overload
    def foo(self, *, bar: Literal[True]) -> int: ...

    @overload
    def foo(self, *, bar: Literal[False]) -> float: ...

    def foo(self, *, bar: Literal[True, False]) -> int | float:
        return super().foo(bar=bar)


def compute_bool() -> bool:
    return random.random() > 10


Foo().foo(bar=compute_bool())  # Problem 1: For this line mypy raises an error
```
```bash
$ mypy --strict test.py
test.py:31: error: No overload variant of "foo" of "Foo" matches argument type "bool"  [call-overload]
test.py:31: note: Possible overload variants:
test.py:31: note:     def foo(self, *, bar: Literal[True]) -> int
test.py:31: note:     def foo(self, *, bar: Literal[False]) -> float
Found 1 error in 1 file (checked 1 source file)
```
And ruff suggest fixing the annotations like this:
```bash
$ ruff check --isolated --select RUF --preview test.py
test.py:12:27: RUF038 `Literal[True, False]` can be replaced with `bool`
   |
10 |     def foo(self, *, bar: Literal[False]) -> float: ...
11 |
12 |     def foo(self, *, bar: Literal[True, False]) -> int | float:
   |                           ^^^^^^^^^^^^^^^^^^^^ RUF038
13 |         return 0
   |
   = help: Replace with `bool`

test.py:23:27: RUF038 `Literal[True, False]` can be replaced with `bool`
   |
21 |     def foo(self, *, bar: Literal[False]) -> float: ...
22 |
23 |     def foo(self, *, bar: Literal[True, False]) -> int | float:
   |                           ^^^^^^^^^^^^^^^^^^^^ RUF038
24 |         return super().foo(bar=bar)
   |
   = help: Replace with `bool`

Found 2 errors.
No fixes available (2 hidden fixes can be enabled with the `--unsafe-fixes` option).
```
Resulting in:
```python
import random
from typing import Literal, overload


class Foo:
    @overload
    def foo(self, *, bar: Literal[True]) -> int: ...

    @overload
    def foo(self, *, bar: Literal[False]) -> float: ...

    def foo(self, *, bar: bool) -> int | float:
        return 0


class FooSub(Foo):
    @overload
    def foo(self, *, bar: Literal[True]) -> int: ...

    @overload
    def foo(self, *, bar: Literal[False]) -> float: ...

    def foo(self, *, bar: bool) -> int | float:
        return super().foo(bar=bar)  # Problem 2 introduced


def compute_bool() -> bool:
    return random.random() > 10


Foo().foo(bar=compute_bool()) # Problem 1 again, same as unfixed code
```
And again `mypy` on the fixed code:
```bash
$ mypy --strict test_fixed.py
test_fixed.py:24: error: Returning Any from function declared to return "int | float"  [no-any-return]
test_fixed.py:24: error: No overload variant of "foo" of "Foo" matches argument type "bool"  [call-overload]
test_fixed.py:24: note: Possible overload variants:
test_fixed.py:24: note:     def foo(self, *, bar: Literal[True]) -> int
test_fixed.py:24: note:     def foo(self, *, bar: Literal[False]) -> float
test_fixed.py:31: error: No overload variant of "foo" of "Foo" matches argument type "bool"  [call-overload]
test_fixed.py:31: note: Possible overload variants:
test_fixed.py:31: note:     def foo(self, *, bar: Literal[True]) -> int
test_fixed.py:31: note:     def foo(self, *, bar: Literal[False]) -> float
Found 3 errors in 1 file (checked 1 source file)
```
-------
```bash
$ python --version
Python 3.11.10
$ ruff --version
ruff 0.9.6
$ mypy --version
mypy 1.15.0 (compiled: yes)
```

---

_Comment by @Geo5 on 2025-02-12 23:34_

Oh i am so sorry! I didn't read [the doc for rule `RUF038`](https://docs.astral.sh/ruff/rules/redundant-bool-literal/) close enough and did not look at the issues linked there! I think the issues for mypy and pyright might cover this?

---

_Renamed from "`RUF038` `Literal[True, False]` might not always be equal to `bool`" to "`RUF038` Incorrect fix for  `Literal[True, False]` in functions with overloads" by @Geo5 on 2025-02-12 23:44_

---

_Renamed from "`RUF038` Incorrect fix for  `Literal[True, False]` in functions with overloads" to "`RUF038` Surprising fix for  `Literal[True, False]` in functions with overloads" by @Geo5 on 2025-02-13 00:00_

---

_Comment by @MichaReiser on 2025-02-13 08:35_

Yes, this part is covered by the section you call out. However, it does make me wonder if we should skip providing a fix alltogether if a method is marked as `@overload`. @AlexWaygood wdyt?

---

_Label `rule` added by @MichaReiser on 2025-02-13 08:35_

---

_Comment by @AlexWaygood on 2025-02-13 16:39_

It's not totally clear to me whether mypy is correct in rejecting the overloads after they've been autofixed by Ruff here. Overloads aren't currently clearly specified in the typing spec, but there is a [draft PR](https://github.com/python/typing/pull/1839) to provide a detailed specification for them, which I think is nearly over the line now. The rendered version of the draft spec is [here](https://typing--1839.org.readthedocs.build/en/1839/spec/overload.html), and it describes how type checkers should perform "argument type expansion" for overloads, which for `bool` means that it should implicitly convert `bool` into union `Literal[True] | Literal[False]`. Assuming that the draft overload chapter is accepted to the spec without major revisions, I think that might put mypy's behaviour here in violation of the new spec. @carljm can correct me if I'm wrong!

Whether mypy is right or wrong, though, is perhaps irrelevant here. We perhaps shouldn't be making changes that would cause your CI to fail, and here we clearly are, since mypy now emits more errors after Ruff's autofix than it did initially. So I'd be okay with ignoring any `@overload`-decorated function definitions (or any function definitions that shadow `@overload`-decorated function definitions) for this rule. I think that makes sense.

---

_Comment by @carljm on 2025-02-13 17:20_

According to the draft spec for overload handling, mypy is in the wrong here by failing to expand `bool` to `Literal[True, False]` when checking a call with an argument typed as `bool` to a function with overloads taking `Literal[True]` and `Literal[False]`.

Both the error in the initial code and the new error in the fixed code are examples of the exact same mypy limitation, the problem just occurs in a new location (inside the implementation of `FooSub.bar`) when the annotation on `FooSub.bar` is changed to `bool`.

If mypy were fixed to perform expansion of `bool`, neither version of the code would have any errors.

In general (even not considering overloads specifically) anywhere a type checker fails to treat `bool` and `Literal[True, False]` the same, it's a type checker bug IMO -- those types are equivalent. (Side note: our fix safety docs for this rule are simply wrong when they claim in the second bullet point that `bool` and `Literal[True, False]` are not strictly equivalent because `bool` is a subtype of `int` -- `Literal[True, False]` is also a subtype of `int`.)

The fix safety docs do already mention this exact problem in the fix safety section, and even link to relevant mypy bugs.

I think we could decline to autofix this in this particular overload case, as a concession to mypy, but I'm not sure how many real-world scenarios would be helped by that. An overload like this is quite likely already causing errors under mypy, because it's almost certain you'd be calling it somewhere with a value typed as `bool` (as is indeed the case in version 1 of the code here, before the autofix.) So personally my feeling is that calling it out in "fix safety" is sufficient concession to the mypy bug.

---

_Comment by @AlexWaygood on 2025-02-13 17:22_

> (Side note: our fix safety docs for this rule are simply wrong when they claim in the second bullet point that `bool` and `Literal[True, False]` are not strictly equivalent because `bool` is a subtype of `int` -- `Literal[True, False]` is also a subtype of `int`.)

Yeah I also noticed that -- I agree, I'm not sure what those docs are trying to say there

---

_Comment by @Geo5 on 2025-02-16 15:01_

> I think we could decline to autofix this in this particular overload case, as a concession to mypy, but I'm not sure how many real-world scenarios would be helped by that. An overload like this is quite likely already causing errors under mypy, because it's almost certain you'd be calling it somewhere with a value typed as `bool` (as is indeed the case in version 1 of the code here, before the autofix.) So personally my feeling is that calling it out in "fix safety" is sufficient concession to the mypy bug.

I agree with everything you said, thank you for the writeup!
Just for some additional context, i didn't hit the `mypy` bug without the fix, as i only ever called the overloaded function as `foo(..., True)` or `foo(..., False)`, but i agree that this is probably rare.

---
