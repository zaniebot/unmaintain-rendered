---
number: 9070
title: Autofix remove types from docstring 
type: issue
state: open
author: KennethEnevoldsen
labels:
  - rule
assignees: []
created_at: 2023-12-09T16:04:39Z
updated_at: 2025-09-06T10:25:03Z
url: https://github.com/astral-sh/ruff/issues/9070
synced_at: 2026-01-07T13:12:15-06:00
---

# Autofix remove types from docstring 

---

_Issue opened by @KennethEnevoldsen on 2023-12-09 16:04_

Currently types can be added in two places in a function as a type hint and in the documentation.

```python
def myfunc(value: int):
   """
   Args:
      value (int): any value you want
   """
   pass
```

This can lead to potential conflict between the type hint and the docstring leading to multiple sources of truth. #5541 seeks to solve this by fixing the docstring, however this could still lead to the reverse issue; that the docstring is updated but not the type hint. It also avoid duplicating information. The reason to prioritise the type hint is meaningful as it can be checked by type hinters etc.

There might still be reasons to have types in the docstring such as documentation, but there already exist extension which remedy this need (e.g. https://pypi.org/project/sphinx-autodoc-typehints/). I probably wouldn't set it as a default though.

Let me know if there is something I have missed. I very much enjoy the package.

Edit: In a similar vein, it might also be worth discouraging "Defaults to {value}" in the docstring as it similar can be outdated and is already specified function signature.

---

_Comment by @sbrugman on 2023-12-09 23:36_

For the same reasoning we also dropped type information from our docstrings. The redundancy is error prone and adds little information.

Another similar rule would be type docstrings for classes, where the type hint can be provided by [instance variable annotations](https://peps.python.org/pep-0526/#class-and-instance-variable-annotations) ([example](https://github.com/pycodehash/pycodehash/pull/60/files))

---

_Comment by @Mr-Pepe on 2023-12-10 02:24_

Great idea! At work, we switched to `sphinx-autodoc-typehints` but now we have lots of code that is inconsistent, with old docstrings containing the type and new code omitting it. 

---

_Comment by @T-256 on 2023-12-10 07:26_

Also [PEP 727](https://peps.python.org/pep-0727/) maybe could change the way of how to document parameters. then, after it standardized, we need to remove junks from docstrings.

---

_Comment by @Mr-Pepe on 2023-12-10 13:19_

In order to implement an autofix, a new rule is needed first, right? Something like `Parameter type already documented via type hint`? This would probably fit into the `D4XX` category.


> Also [PEP 727](https://peps.python.org/pep-0727/) maybe could change the way of how to document parameters. then, after it standardized, we need to remove junks from docstrings.

I think that is orthogonal to this issue and should be treated separately, once PEP 727 has been accepted. The rule could be something like `Parameter already documented in function signature`.

---

_Label `rule` added by @zanieb on 2023-12-10 16:04_

---

_Comment by @zanieb on 2023-12-10 16:05_

This seems reasonable to me! Although we do not yet have a dedicated docstring parser and may want to wait until we write one.

---

_Comment by @sbrugman on 2023-12-10 20:55_

See also https://github.com/astral-sh/ruff/issues/458

---

_Referenced in [astral-sh/ruff#458](../../astral-sh/ruff/issues/458.md) on 2024-04-16 13:36_

---

_Comment by @cbornet on 2025-09-06 09:37_

Is that `DOC111`: https://github.com/astral-sh/ruff/issues/12434 ?

---

_Comment by @KennethEnevoldsen on 2025-09-06 09:59_

Yeah that looks pretty much equivalent

---

_Comment by @cbornet on 2025-09-06 10:25_

In pydoclint : https://jsh9.github.io/pydoclint/config_options.html#4---arg-type-hints-in-docstring-and---arg-type-hints-in-signature

---
