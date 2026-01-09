---
number: 21986
title: Remove redundant parentheses around tuple literals keys in dict indexing
type: issue
state: closed
author: injust
labels: []
assignees: []
created_at: 2025-12-15T11:26:43Z
updated_at: 2025-12-15T11:37:25Z
url: https://github.com/astral-sh/ruff/issues/21986
synced_at: 2026-01-07T13:12:16-06:00
---

# Remove redundant parentheses around tuple literals keys in dict indexing

---

_Issue opened by @injust on 2025-12-15 11:26_

### Summary

I'd like to have a rule to enforce a consistent style for tuple literals keys in dict indexing.

```python
foo: dict[tuple[int, int], str] = {}
foo[(1, 2)] = "hi"
print(foo[1, 2])
```

`foo[1, 2]` and `foo[(1, 2)]` are equivalent -- the parentheses are redundant.

---

_Comment by @AlexWaygood on 2025-12-15 11:31_

Hi! Does https://docs.astral.sh/ruff/rules/incorrectly-parenthesized-tuple-in-subscript/ do this for you?

---

_Comment by @injust on 2025-12-15 11:36_

Oh nice, that's literally the exact rule that I wanted.

I generally don't use/look at the preview rules. Sorry for the noise!

---

_Closed by @injust on 2025-12-15 11:36_

---
