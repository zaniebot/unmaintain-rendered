---
number: 16860
title: Check for missing arguments
type: issue
state: open
author: DamyanBG
labels:
  - rule
  - type-inference
assignees: []
created_at: 2025-03-19T21:53:01Z
updated_at: 2025-03-20T03:18:52Z
url: https://github.com/astral-sh/ruff/issues/16860
synced_at: 2026-01-07T13:12:16-06:00
---

# Check for missing arguments

---

_Issue opened by @DamyanBG on 2025-03-19 21:53_

### Question

Do you think to add check for missing arguments for a method or function?

For example I have function:

`
     def example_function(a, b, c):
      result = a + b * c
      return result
`

and I am calling this function - example_function(2, 5)

### Version

_No response_

---

_Label `question` added by @DamyanBG on 2025-03-19 21:53_

---

_Comment by @ntBre on 2025-03-20 03:18_

I think it would be a helpful rule, but it will require type inference and multi-file analysis to detect accurately.

---

_Label `type-inference` added by @ntBre on 2025-03-20 03:18_

---

_Label `rule` added by @ntBre on 2025-03-20 03:18_

---

_Label `question` removed by @ntBre on 2025-03-20 03:18_

---
