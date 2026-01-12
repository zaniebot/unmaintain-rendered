```yaml
number: 12287
title: TRY004 applies when unrelated values are type-checked
type: issue
state: open
author: dscorbett
labels:
  - rule
assignees: []
created_at: 2024-07-11T02:16:21Z
updated_at: 2025-06-09T12:35:47Z
url: https://github.com/astral-sh/ruff/issues/12287
synced_at: 2026-01-12T15:54:51Z
```

# TRY004 applies when unrelated values are type-checked

---

_@dscorbett_

TRY004 assumes that if any exception is raised, subject to a type-checking `if` condition, the exception should be a `TypeError`. That is only true when an argument has the wrong type. If the exception happens for a different reason (e.g. invalid state) then `TypeError` isn’t necessarily appropriate (even if the invalid state is detected by checking the type of an instance variable).

[The Python documentation for `TypeError`](https://docs.python.org/3/library/exceptions.html#TypeError) says:
> Passing arguments of the wrong type (e.g. passing a [`list`](https://docs.python.org/3/library/stdtypes.html#list) when an [`int`](https://docs.python.org/3/library/functions.html#int) is expected) should result in a [`TypeError`](https://docs.python.org/3/library/exceptions.html#TypeError), but passing arguments with the wrong value (e.g. a number outside expected boundaries) should result in a [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError).

Example code:
```python
class FreezableList:
    def __init__(self):
        self.delegate = []

    def freeze(self):
        self.delegate = tuple(self.delegate)

    def append(self, value):
        if isinstance(self._delegate, list):
            self.delegate.append(value)
        else:
            raise ValueError("Appending to a frozen list")
```

Current behavior:
```console
$ ruff --version
ruff 0.5.1
$ ruff check --isolated --select TRY004 --output-format concise try004_example.py
try004_example.py:12:13: TRY004 Prefer `TypeError` exception for invalid type
Found 1 error.
```

A heuristic could be that, if the value in the `isinstance` call is neither an argument to the method or function nor is derived from an argument, TRY004 should not report anything.

---

_Comment by @dhruvmanila on 2024-07-11 03:13_

Thanks for opening this issue! Technically, I think the heuristic is correct to raise a violation in this case. I'm not sure whether raising a `ValueError` makes more sense here because the "value" is not the problem here. Personally, I would raise a `TypeError` here because the main issue is in the type itself. CPython also raises a `TypeError` in case you try to concatenate a frozenset:

```
In [1]: x = frozenset(['a', 'b', 'c'])

In [2]: x + frozenset(['d'])
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
Cell In[2], line 1
----> 1 x + frozenset(['d'])

TypeError: unsupported operand type(s) for +: 'frozenset' and 'frozenset'
```

> A heuristic could be that, if the value in the `isinstance` call is neither an argument to the method or function nor is derived from an argument, TRY004 should not report anything.

I'm not sure if that would solve your use-case because `self.delegate` would be considered as derived from the `self` argument. Or, am I misunderstanding what you're trying to say?

---

_Comment by @dscorbett on 2024-07-11 15:09_

> Personally, I would raise a `TypeError` here because the main issue is in the type itself.

I raise `ValueError` because the fact that I’m using `isinstance` is an internal implementation detail. For example, here is an alternative implementation of that class:
```python
class FreezableList:
    def __init__(self):
        self._delegate = []
        self._frozen = False

    @property
    def delegate(self):
        return tuple(self._delegate) if self._frozen else self._delegate

    def freeze(self):
        self._frozen = True

    def append(self, value):
        if not self._frozen:
            self.delegate.append(value)
        else:
            raise ValueError("Appending to a frozen list")
```

Both versions of the class implement essentially the same API, so they should presumably raise the same errors under the same circumstances, regardless of whether `isinstance` is used internally. The error is fundamentally about calling a method when the object is in the wrong state, not about types, so `ValueError` is more appropriate than `TypeError`.

> I'm not sure whether raising a `ValueError` makes more sense here because the "value" is not the problem here.

It isn’t great but `ValueError` seems to be the closest Python has to what I mean; in Java, I would use `IllegalStateException`.

> I'm not sure if that would solve your use-case because `self.delegate` would be considered as derived from the `self` argument. Or, am I misunderstanding what you're trying to say?

That’s true; I meant arguments besides `self`. `self` is an exception because it is set apart syntactically when calling the method, so it doesn’t feel like an argument in the same way. My intuition is that if the type-checking problem is related to a non-`self` argument, it is probably the caller’s fault for passing in a value of the wrong type, which warrants a `TypeError`, whereas if the problem is related to `self` (or one of its attributes), it has a different cause and need not warrant being surfaced to the caller as a `TypeError`.

---

_Comment by @dhruvmanila on 2024-07-15 09:36_

> That’s true; I meant arguments besides `self`. `self` is an exception because it is set apart syntactically when calling the method, so it doesn’t feel like an argument in the same way. My intuition is that if the type-checking problem is related to a non-`self` argument, it is probably the caller’s fault for passing in a value of the wrong type, which warrants a `TypeError`, whereas if the problem is related to `self` (or one of its attributes), it has a different cause and need not warrant being surfaced to the caller as a `TypeError`.

I see. I think it makes sense, I'm trying to think through if there's any case where this might yield false negatives but can't find any. I'd love to hear others opinion.

---

_Label `rule` added by @dhruvmanila on 2024-07-15 09:36_

---

_Comment by @wolkiewiczk on 2025-06-09 12:35_

> A heuristic could be that, if the value in the isinstance call is neither an argument to the method or function nor is derived from an argument, TRY004 should not report anything.

I don't think that TRY004 should report if isinstance check is performed on an object derived from an argument. See this example:

```python
class MyTree(IntervalTree):

    def __init__(self, intervals=None):
        intervals = intervals or []
        for iv in intervals:
            if not isinstance(iv.data, MyDataclass):
                raise ValueError(
                    f"All intervals in {type(self).__name__} must have the 'data' attribute set to "
                    f"the instance of {MyDataclass.__name__} subclass."
                )
        super().__init__(*args, **kwargs)
```

In this case TRY004 is reported, even though the type of my argument is correct. Even the types of objects inside the iterable are correct, they are just required to have a certain class set as an attribute so I think that the `ValueError` is the correct choice here.

---
