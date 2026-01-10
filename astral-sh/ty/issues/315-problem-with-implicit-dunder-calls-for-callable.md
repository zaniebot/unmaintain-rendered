```yaml
number: 315
title: "Problem with implicit dunder calls for `Callable` members"
type: issue
state: closed
author: Julian-J-S
labels:
  - question
  - calls
  - attribute access
assignees: []
created_at: 2025-05-11T09:57:43Z
updated_at: 2025-05-14T18:07:52Z
url: https://github.com/astral-sh/ty/issues/315
synced_at: 2026-01-10T02:34:09Z
```

# Problem with implicit dunder calls for `Callable` members

---

_Issue opened by @Julian-J-S on 2025-05-11 09:57_

Its hard for me to narrow down the problem but I will try to give as much context as needed 🤓 

My first guess would be that this strange "Variable-function-definition" (instead of normal method) might cause the problem.
However, Pylance seems to have no problem with that.

Background: I am using `pyspark` (through `databricks-connect` library) and found there to be some problematic type problems.

## Code

```python
from pyspark.sql import functions as F

add = F.col("a") + F.col("b")
sub = F.col("a") - F.col("b")
```

## Pylance ✅ 

<img width="754" alt="Image" src="https://github.com/user-attachments/assets/4694da92-57e6-4360-b5ca-a4e43ac65400" />

### Going deeper
When I "Go to Definition" I can see some weird `(variable) def` instead of the normal `(method) def` inside the `Column` class.

<img width="653" alt="Image" src="https://github.com/user-attachments/assets/105e3caa-5337-4007-9a8b-0fa7a3a7e764" />

## ty

<img width="930" alt="Image" src="https://github.com/user-attachments/assets/ebdacbd2-5a8a-4349-9f58-071164ca2583" />

---

_Renamed from "Arithmetic functions not found; class `(variable) def` not found/identified" to "Problem with implicit dunder calls for `Callable` members" by @sharkdp on 2025-05-13 09:44_

---

_Comment by @sharkdp on 2025-05-13 09:45_

Thank you for reporting this. This is a bug in ty.

The issue here is that `__add__` is [casted to a `Callable` type](https://github.com/apache/spark/blob/7c29c664cdc9321205a98a14858aaf8daaa19db2/python/pyspark/sql/column.py#L221-L224) and we don't properly model the treatment of that `Callable` type as a bound method. MRE:
```py
from __future__ import annotations

from typing import reveal_type, Callable, cast


def add_impl(l: Column, r: Column) -> Column:
    raise NotImplementedError


class Column:
    __add__ = cast(Callable[[Column, Column], Column], add_impl)


def _(c: Column):
    c + c  # ty: Operator `+` is unsupported between objects of type `Column` and `Column`
```
https://play.ty.dev/e1d594b6-1b28-4cf2-a78e-a3ad6806449b

---

_Label `bug` added by @sharkdp on 2025-05-13 09:46_

---

_Label `calls` added by @sharkdp on 2025-05-13 09:46_

---

_Label `attribute access` added by @sharkdp on 2025-05-13 09:46_

---

_Assigned to @sharkdp by @sharkdp on 2025-05-13 21:19_

---

_Comment by @sharkdp on 2025-05-14 13:03_

We've been thinking a bit more about this, and it looks to us that the problem is actually not with ty, but rather with that `Callable` annotation. The TLDR is: a generic `Callable` type does not necessarily need to be a descriptor with a `__get__` method (similar to a normal function). That means that we're technically correct to not create a bound method here.

---

To explain this in a bit more detail, let's create a callable that prints out its arguments:
```py
class ArgPrinter:
    def __call__(self, *args):
        print(f"{args=}")

arg_printer = ArgPrinter()
```

When we call `arg_printer(1, "a")`, it prints `args=(1, 'a')`. Now let's use this callable as a `__add__` method on `Column`. We add a `name` attribute to distinguish object identities:
```py
from dataclasses import dataclass

@dataclass
class Column:
    name: str

    __add__ = arg_printer

Column("left") + Column("right")
```

The last line here implicitly calls `__add__`. At runtime, this prints `args=(Column(name='right'),)`. Only the `Column("right")` object is passed as an argument(!). In general, the protocol for implicit dunder calls (using `__add__` as an example here) is as follows:

* Look up `__add__` in the MRO of `type(left)`
* Check if `type(dunder_callable)` has a `__get__` method. If so, set `dunder_callable = type(dunder_callable).__get__(dunder_callable, left, type(left))`. If not, keep `dunder_callable` from above`
* Call `dunder_callable(right)`.

For our `arg_printer` callable, there is no `__get__` method on `type(arg_printer) = ArgPrinter`, so that second step is skipped, and `arg_printer(right)` is called.

So the correct way to type `__add__` here would be `Callable[[Column], None]`. And indeed, if we set
```py
    __add__: Callable[[Column], None] = arg_printer
```
no error is emitted by ty (same for mypy and pyright).


Now what if the callable *is* a descriptor (e.g. a function)? Let's add a `__get__` method to `ArgPrinter` that emulates the binding of the `self` argument:
```
from functools import partial

class ArgPrinter:
    # __call__ as before

    def __get__(self, instance, owner):
        return partial(self, instance)
```
What this does is to return a new callable object. If that returned callable is called (in the implicit `__add__` call), it will call `self` (i.e. `ArgPrinter`'s `__call__` method), but pass `instance` as the first argument, before passing other arguments.

If we now repeat the `Column("left") + Column("right")` operation from above, we see that both "left" and "right" arguments are passed. `left` via the `instance` argument of the descriptor, and `right` as the actual argument to the `__add__` callable (which was returned from `ArgPrinter.__get__`).

How would we type `__add__` now? There is no way to express the descriptor behavior in a `Callable` annotation. The best we could do would probably be some `Protocol` like
```py
class AddColumns(Protocol):
    # or `-> Column`, for the real example
    def __call__(self, left: Column, right: Column) -> None: ...
    def __get__(self, instance: Column, owner: type) -> Callable[[Column], None]: ...
```
(to completely model the descriptor behavior, we would also add an overload for the case where `instance` is `None`, e.g. if `Column.__add__(left, right)` is called explicitly. In that case, the implementation should also just return `self` if `instance is None`. Which means that this overload should return `Callable[[Column, Column], None]`).

---

Where does that leave us? `pyspark`'s annotation for `__add__` on `Column` is technically wrong. If there is nothing else we know about the type of `__add__`, we must assume that it's not a descriptor, and therefore simply call it with one argument ("right"). Since this is incompatible with the `Callable[[Column, Column], Column]` signature, the implicit call fails. This is where the `unsupported-operator` diagnostic comes from (we should improve that diagnostic and also show the reason why it's not supported...).

Interestingly, `pyspark` seems to have changed [the way they defined `__add__` on `Column`](https://github.com/apache/spark/blob/092223365928d03014db6a65cfe9c28000e3c4d1/python/pyspark/sql/column.py#L84-L88) in more recent versions. So maybe this particular problem will eventually just go away?

That said, we realize that some library authors and users might rely on an implicit function-like behavior of `Callable` types, where they act as descriptors that bind the instance argument to the first ("`self`") parameter. If you or someone else sees this pattern being used somewhere else, we'd be glad to know.

---

_Comment by @sharkdp on 2025-05-14 13:07_

Interestingly, if the `cast` is changed to a normal declaration, then [pyright also issues a `reportOperatorIssue` diagnostic](https://pyright-play.net/?code=GYJw9gtgBA%2BjwFcAuCQFM5QJYQA5hCSgEMA7UsJYpLMUgZwChHRIokBPXLUgc2zwEi6AG5piAGxidcaADRQAwpInEARhPlQAxsXpIFABXBIw2sBOaMAJmmAlr1mDlwSAFBIBcSiwgikFEG9FX38ASigAWgA%2BHwk-Uk9GKBSoEGIsejQoADlKAEk8TQg0UiQ0awBREHAQK21Veno4hKTUqABiWBhiR0wAXh09JDdlCVUNNABtKZD4-wU5hIBdRdDSVYcnFwkw5NS4XqcYYJV1TRmlhZb-TauNqEGj5yKrW3sYN21g9bC21O0UAA1DpGEA).

---

_Label `bug` removed by @sharkdp on 2025-05-14 13:08_

---

_Label `question` added by @sharkdp on 2025-05-14 13:08_

---

_Comment by @Julian-J-S on 2025-05-14 15:17_

@sharkdp 
wow 🤯  what an impressive deep dive! thank you! 👍 

I guess its super early with `ty` and this is a general question/tradeoff between "being correct" and "best support/usability".
I think if this is a super niche outlier (though many people use Databricks), then I think being correct and getting others to be correct is a good choice 😄

I just checked and there was a code change more than a year ago that fixes this problem.
However, this was only in the 4.* branch which is still in development and will take months (or longer) to actually be released and then the adoption of Databricks and company runtimes will probably take years 😆 

Nonetheless, super exited for `ty` 🥳 

---

_Comment by @carljm on 2025-05-14 18:07_

Seeing that a) pyright also (sometimes) errors on this, b) the current behavior seems correct, and c) pyspark seems to have already recognized that and updated the annotation in more recent versions, I don't think there is a strong case for ty to change type checking behavior here at this point, so for now I am going to close this as "not planned." I did file https://github.com/astral-sh/ty/issues/391 to capture the fact that we really want more diagnostic context for `unsupported-operator`.

Thanks again for the report, and thanks @sharkdp for the thorough exploration here!

---

_Closed by @carljm on 2025-05-14 18:07_

---
