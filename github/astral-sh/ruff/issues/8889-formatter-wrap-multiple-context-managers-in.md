---
number: 8889
title: "Formatter: `wrap_multiple_context_managers_in_parens` preview style"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
created_at: 2023-11-29T01:16:49Z
updated_at: 2023-12-22T03:41:05Z
url: https://github.com/astral-sh/ruff/issues/8889
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: `wrap_multiple_context_managers_in_parens` preview style

---

_Issue opened by @MichaReiser on 2023-11-29 01:16_

Implement Black's [`wrap_multiple_context_managers_in_parens`](https://github.com/psf/black/pull/3489) as a preview style. The new style only applies to Py39+. 

```python
with \
     make_context_manager1() as cm1, \
     make_context_manager2() as cm2, \
     make_context_manager3() as cm3, \
     make_context_manager4() as cm4 \
:
    pass
```

Gets formatted as

```python
with (
    make_context_manager1() as cm1,
    make_context_manager2() as cm2,
    make_context_manager3() as cm3,
    make_context_manager4() as cm4,
):
    pass
```


Black automatically detects the python version used in a file based on the syntax used. E.g. a file using match statements is Py310+. I'm unsure if we should autodetect the python version based on the syntax or rely on `requires-python` 

---

_Referenced in [astral-sh/ruff#8678](../../astral-sh/ruff/issues/8678.md) on 2023-11-29 01:16_

---

_Label `formatter` added by @MichaReiser on 2023-11-29 01:17_

---

_Label `preview` added by @MichaReiser on 2023-11-29 01:17_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-11-29 01:17_

---

_Referenced in [astral-sh/ruff#8890](../../astral-sh/ruff/issues/8890.md) on 2023-11-29 01:23_

---

_Renamed from "`wrap_multiple_context_managers_in_parens`: Requires Py39+" to "Formatter: `wrap_multiple_context_managers_in_parens` preview style" by @MichaReiser on 2023-11-29 01:48_

---

_Comment by @charliermarsh on 2023-12-02 13:57_

> I'm unsure if we should autodetect the python version based on the syntax or rely on `requires-python`.

I would prefer to stick to our existing configuration semantics, and not introduce any autodetection.


---

_Comment by @charliermarsh on 2023-12-03 20:49_

It looks like the original intent, per [the docs](https://black.readthedocs.io/en/stable/the_black_code_style/future_style.html#using-backslashes-for-with-statements), is to reformat as:

```python
# Input
with make_context_manager1() as cm1, make_context_manager2() as cm2, make_context_manager3() as cm3, make_context_manager4() as cm4:
    pass

# < Python 3.9
with \
     make_context_manager1() as cm1, \
     make_context_manager2() as cm2, \
     make_context_manager3() as cm3, \
     make_context_manager4() as cm4 \
:
    pass

# >= Python 3.9
with (
    make_context_manager1() as cm1,
    make_context_manager2() as cm2,
    make_context_manager3() as cm3,
    make_context_manager4() as cm4,
):
    pass
```

But AFAICT Black only implemented the second piece, and not the continuation escapes.


---

_Assigned to @MichaReiser by @MichaReiser on 2023-12-21 03:22_

---

_Referenced in [astral-sh/ruff#9222](../../astral-sh/ruff/pulls/9222.md) on 2023-12-21 03:52_

---

_Closed by @MichaReiser on 2023-12-22 03:41_

---
