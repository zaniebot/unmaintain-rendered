---
number: 8888
title: "Formatter: `no_blank_line_before_class_docstring` preview style"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - help wanted
  - preview
assignees: []
created_at: 2023-11-29T01:13:40Z
updated_at: 2023-12-19T06:43:22Z
url: https://github.com/astral-sh/ruff/issues/8888
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: `no_blank_line_before_class_docstring` preview style

---

_Issue opened by @MichaReiser on 2023-11-29 01:13_

Implement the [`no_blank_lines_before_class_docstring`](https://github.com/psf/black/pull/3692) as a preview style. 

The new style aligns the docstring formatting between functions and classes. 


```python
class C:

    """The line above stays."""
```

Gets reformatted to 

```python
class C:
    """The line above stays."""
```

as it is already the case for functions.

Task: Copy over the test files, implement the new style behind a preview flag.

---

_Referenced in [astral-sh/ruff#8678](../../astral-sh/ruff/issues/8678.md) on 2023-11-29 01:13_

---

_Renamed from "`no_blank_line_before_class_docstring`: Aligns the docstring formatting between functions and classes. Seems reasonable." to "Formatter: `no_blank_line_before_class_docstring`" by @MichaReiser on 2023-11-29 01:14_

---

_Label `formatter` added by @MichaReiser on 2023-11-29 01:14_

---

_Label `preview` added by @MichaReiser on 2023-11-29 01:14_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-11-29 01:14_

---

_Label `help wanted` added by @MichaReiser on 2023-11-29 01:31_

---

_Renamed from "Formatter: `no_blank_line_before_class_docstring`" to "Formatter: `no_blank_line_before_class_docstring` preview style" by @MichaReiser on 2023-11-29 01:48_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-12-15 18:24_

---

_Referenced in [astral-sh/ruff#9154](../../astral-sh/ruff/pulls/9154.md) on 2023-12-16 01:03_

---

_Closed by @dhruvmanila on 2023-12-19 06:43_

---
