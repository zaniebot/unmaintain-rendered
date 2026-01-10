---
number: 8445
title: Investigate how documentation tooling treats indents in docstrings
type: issue
state: closed
author: konstin
labels:
  - docstring
  - formatter
assignees: []
created_at: 2023-11-02T10:37:10Z
updated_at: 2024-02-08T14:14:12Z
url: https://github.com/astral-sh/ruff/issues/8445
synced_at: 2026-01-10T01:22:48Z
---

# Investigate how documentation tooling treats indents in docstrings

---

_Issue opened by @konstin on 2023-11-02 10:37_

In the formatter, we currently normalize docstring indentation in the same way as black and cpython do. For example this is what cpython does, using `str.expandtabs` with a fixed width of 8 on python 3.13:
```
>>> exec('def f():\n\t"""Computes the value.\n\tx = 1\n\t\ty = x + 1\n\t"""')
>>> f.__doc__
'Computes the value.\nx = 1\n        y = x + 1\n'
```
This becomes a problem when using the formatter with tab indentation, esp. with a tab width that is not 8, because we're either inconsistent with tab indentation or mismatch what cpython does. We should investigate how the various docs tools handle docstring indents. If the indent size or kind doesn't matter to them, we have more freedom in e.g. converting to tabs inside docstrings, otherwise we need to know which properties we need to preserve.

---

_Label `docstring` added by @charliermarsh on 2023-11-02 12:55_

---

_Referenced in [astral-sh/ruff#8430](../../astral-sh/ruff/issues/8430.md) on 2023-11-03 01:24_

---

_Comment by @Mutantpenguin on 2023-11-23 13:02_

Is there any way to suppress formatting of docstrings for now?

---

_Comment by @charliermarsh on 2023-11-23 13:16_

You can add `# fmt: skip` to the end:

```python
def func():
    """abc
def
    ghi
    """  # fmt: skip
```

But there is no dedicated setting to enforce this globally (and, candidly, I think we're unlikely to add one).

---

_Comment by @Mutantpenguin on 2023-11-23 13:36_

Thanks, but frankly that's just not feasible for any bigger codebase.

I added some examples to #8430 to show what the current behaviour does to our codebase. That just looks wrong, regardless if E101 is activated or not.

If E101 would ignore multiline strings and the formatter would use the value of `indent-width` then this might be a valid compromise.

---

_Comment by @charliermarsh on 2023-11-23 13:55_

Makes sense! I'm sure we'll make some improvements to the tab-in-docstring behavior (hence this issue). It just hasn't been prioritized yet. But I'd rather spend time fixing the root cause than investing in other workarounds.

---

_Label `formatter` added by @MichaReiser on 2023-11-27 03:09_

---

_Comment by @sciyoshi on 2023-12-11 19:33_

@konstin I don't see the same behavior as you:
```
Python 3.11.6 (main, Oct 23 2023, 22:48:54) [GCC 11.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> exec('def f():\n\t"""Computes the value.\n\tx = 1\n\t\ty = x + 1\n\t"""')
>>> f.__doc__
'Computes the value.\n\tx = 1\n\t\ty = x + 1\n\t'
```

Sphinx seems to be configurable using the `tab-width` option.

---

_Comment by @sciyoshi on 2023-12-11 20:35_

(As pointed out on Discord, this behavior will be changing in Python 3.13.)

---

_Comment by @MichaReiser on 2024-02-06 14:32_

* `pydoc` uses cleandoc, which expands tabs to 8 spaces ([source](https://github.com/python/cpython/blob/7974f714786c5d82ba8aeb4c262ad610ce1f0dcd/Lib/pydoc.py#L170-L184))
* `pydoc` uses `getdoc` which uses `cleandoc` ([source](https://github.com/pdoc3/pdoc/blob/f358893e4fcfd7f29857a7ff5491b606ff146d39/pdoc/__init__.py#L517))
* `doxygen` has a [tab size](https://www.doxygen.nl/manual/config.html#cfg_tab_size) option. But what I understand is that it is only used for code snippets (it uses a DSL for the documentation)
* `sphinx` has a [tab width option](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html)

---

_Comment by @MichaReiser on 2024-02-08 14:14_

I'm closing this issue. Most documentation tools use a version of `inspect.cleandoc`. I leave a more in depth reply in 

https://github.com/astral-sh/ruff/issues/8430

---

_Closed by @MichaReiser on 2024-02-08 14:14_

---
