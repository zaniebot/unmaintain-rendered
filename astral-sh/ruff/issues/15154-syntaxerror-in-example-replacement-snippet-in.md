---
number: 15154
title: "`SyntaxError` in example replacement snippet in `nonlocal-without-binding` (`PLE0117`)"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - documentation
assignees: []
created_at: 2024-12-26T22:06:21Z
updated_at: 2024-12-27T12:15:14Z
url: https://github.com/astral-sh/ruff/issues/15154
synced_at: 2026-01-10T01:22:56Z
---

# `SyntaxError` in example replacement snippet in `nonlocal-without-binding` (`PLE0117`)

---

_Issue opened by @AlexWaygood on 2024-12-26 22:06_

The docs for [`nonlocal-without-binding`](https://docs.astral.sh/ruff/rules/nonlocal-without-binding/) have this snippet under the heading `## Use instead`:

```py
class Foo:
    bar = 1

    def get_bar(self):
        nonlocal bar
        ...
```

I would not advise anybody to use this code, because it causes a `SyntaxError` 😆

```pycon
>>> class Foo:
...     bar = 1
... 
...     def get_bar(self):
...         nonlocal bar
...         ...
...         
  File "<python-input-13>", line 5
    nonlocal bar
    ^^^^^^^^^^^^
SyntaxError: no binding for nonlocal 'bar' found
```

That's because name lookup in method scopes can't see bindings in class scopes. I.e., an example with a function scope inside a function scope (rather than a function scope inside a class scope) works correctly:

```py
>>> def foo():
...     bar = 1
...     def get_bar():
...         nonlocal bar
...
>>>
```

---

_Label `bug` added by @AlexWaygood on 2024-12-26 22:06_

---

_Label `documentation` added by @AlexWaygood on 2024-12-26 22:06_

---

_Comment by @enochkan on 2024-12-26 23:46_

@AlexWaygood can I contribute to this? 

Assuming this is the only fix: https://github.com/astral-sh/ruff/commit/54170c33fdc79de18b9fa1e7c516d3b530a763b2 ?


---

_Referenced in [astral-sh/ruff#15157](../../astral-sh/ruff/pulls/15157.md) on 2024-12-26 23:53_

---

_Closed by @AlexWaygood on 2024-12-27 12:15_

---
