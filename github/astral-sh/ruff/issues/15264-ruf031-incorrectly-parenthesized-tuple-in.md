---
number: 15264
title: "`RUF031` (`incorrectly-parenthesized-tuple-in-subscript`) - false positive on implicit type alias"
type: issue
state: closed
author: Garrett-R
labels: []
assignees: []
created_at: 2025-01-05T03:48:27Z
updated_at: 2025-01-05T03:55:02Z
url: https://github.com/astral-sh/ruff/issues/15264
synced_at: 2026-01-07T13:12:16-06:00
---

# `RUF031` (`incorrectly-parenthesized-tuple-in-subscript`) - false positive on implicit type alias

---

_Issue opened by @Garrett-R on 2025-01-05 03:48_

EDIT: Ah, I see that this issue is known and described in bullet point 3 of [this comment](https://github.com/astral-sh/ruff/issues/13065#issuecomment-2306282918).  Decided to keep this issue since duplicates can help people find the original issue.

_______________________

## Description

With setting the config: `"parenthesize-tuple-in-subscript": true`, I get this:

```python
type Foo = dict[str, str]
Foo = dict[str, str]  # (RUF031:Use parentheses for tuples in subscripts)
```
([playground](https://play.ruff.rs/4df86970-fc1e-408e-aa37-05324792c217))

## Context

There's no plan to deprecate the implicit type alias syntax, at least as of the introduction of [PEP 613](https://peps.python.org/pep-0613/#backwards-compatibility).

---

_Closed by @Garrett-R on 2025-01-05 03:48_

---
