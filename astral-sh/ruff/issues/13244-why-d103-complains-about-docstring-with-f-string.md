---
number: 13244
title: Why D103 complains about docstring with f-string?
type: issue
state: closed
author: alanwilter
labels:
  - question
assignees: []
created_at: 2024-09-04T14:16:12Z
updated_at: 2024-09-05T08:50:49Z
url: https://github.com/astral-sh/ruff/issues/13244
synced_at: 2026-01-10T01:22:53Z
---

# Why D103 complains about docstring with f-string?

---

_Issue opened by @alanwilter on 2024-09-04 14:16_


```python
boo = 'hello'
def foo()->None:
    f"""{boo} world!"""
    return
```

`Missing docstring in public function (Ruff` [`D103`](https://docs.astral.sh/ruff/rules/undocumented-public-function))

Or is there a "proper" way to do this otherwise?

---

_Comment by @MichaReiser on 2024-09-04 14:59_

F-strings aren't considered docstrings according to Python:


```shell
>>> def foo()->None:
...     f"""{boo} world!"""
...     return
... 
>>> foo.__doc__
>>> def bar() -> None:
...     """Hello world!"""
...     return
... 
>>> bar.__doc__
'Hello world!'
```

So the rule here is correct.

I don't know if there's an idiomatic way to set a dynamic function's docstring at runtime.

---

_Label `question` added by @MichaReiser on 2024-09-04 14:59_

---

_Comment by @alanwilter on 2024-09-04 15:03_

But what about templates for docstrings? In my case I make use of variables point to file name that may change. Going replacing in every docstring seems nonsense.
Anyway, I will silence D103 in code base for those cases.

---

_Comment by @MichaReiser on 2024-09-04 15:06_

As I said, I don't know how to do dynamic docstrings. Maybe take a look at [this StackOverflow thread](https://stackoverflow.com/questions/2693883/dynamic-function-docstring). 

You can keep using f-strings. Just keep in mind that those are not docstrings and won't show up as documentation. 



---

_Closed by @MichaReiser on 2024-09-04 15:06_

---

_Comment by @dhruvmanila on 2024-09-05 06:29_

> But what about templates for docstrings? In my case I make use of variables point to file name that may change. Going replacing in every docstring seems nonsense.

As far as I know, this can only be done by manually setting the `__doc__` attribute on the object that you're trying to add documentation for. So, in your example:
```
>>> def foo() -> None:
...     return
... 
>>> boo = 'hello'
>>> foo.__doc__ = f"{boo} world!"
>>> help(foo)
Help on function foo in module __main__:

foo() -> None
    hello world!
```

I know Pandas does it a lot and they even have a decorator for it: https://github.com/pandas-dev/pandas/blob/bc9b1c3c4b979978dcdef42b900aa633cfeee28e/pandas/util/_decorators.py#L342-L400. I think even NumPy does it.

---

_Comment by @alanwilter on 2024-09-05 08:50_

Cool suggestion @dhruvmanila, that sorted my issues.

---
