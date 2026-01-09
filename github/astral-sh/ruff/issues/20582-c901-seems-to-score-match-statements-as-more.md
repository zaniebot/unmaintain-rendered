---
number: 20582
title: C901 seems to score match statements as more complex than they are
type: issue
state: closed
author: flying-sheep
labels: []
assignees: []
created_at: 2025-09-25T18:36:34Z
updated_at: 2025-09-25T18:41:37Z
url: https://github.com/astral-sh/ruff/issues/20582
synced_at: 2026-01-07T13:12:16-06:00
---

# C901 seems to score match statements as more complex than they are

---

_Issue opened by @flying-sheep on 2025-09-25 18:36_

### Summary

See https://play.ruff.rs/50b9289d-e9dc-4a5a-afeb-c2fa14b6cf53

The code is nearly identical, the only difference is that the first version uses a `match` statement to *lower* complexity (fewer nested if statements than the second one)

But Ruff seems to think that the first copy is somehow much more complex than the second.

Looks to me like `match` statements get unfairly penalized by your McCabe implementation.

### Version

0.13.2

---

_Comment by @flying-sheep on 2025-09-25 18:41_

Hm actually it looks like they’re just apart by a count of 1 and Ruff can’t identify the last `case` as exhaustive. Which I guess is fair, would be asking a bit much to analyze the code to find exhautive patterns like this.

---

_Closed by @flying-sheep on 2025-09-25 18:41_

---
