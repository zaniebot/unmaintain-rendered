```yaml
number: 19757
title: Autofix for ISC003 produces malformed code
type: issue
state: closed
author: LKajan
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-08-05T09:30:48Z
updated_at: 2025-11-22T01:22:37Z
url: https://github.com/astral-sh/ruff/issues/19757
synced_at: 2026-01-12T15:54:57Z
```

# Autofix for ISC003 produces malformed code

---

_@LKajan_

### Summary

Autofix fix for ISC003 (Explicitly concatenated string should be implicitly concatenated) produces `TypeError: 'str' object is not callable` in the following situation.
```python
result = "Line1\n" + (
    "Line2\n"
    "Line3"
)
```
`ruff check --fix` produces:
```python
result = "Line1\n" (
    "Line2\n"
    "Line3"
)
```
Which gives a `TypeError: 'str' object is not callable` error obiously.

Link to playground: https://play.ruff.rs/a9d9ac04-ef00-4ddf-a3a6-9e61fa9f8840

### Version

0.12.7

---

_Label `bug` added by @ntBre on 2025-08-05 12:32_

---

_Label `fixes` added by @ntBre on 2025-08-05 12:32_

---

_Label `help wanted` added by @ntBre on 2025-08-05 12:32_

---

_Comment by @ntBre on 2025-08-05 12:34_

Thanks for the report!

---

_Comment by @mikeleppane on 2025-08-05 16:14_

I will take a look at this.

---

_Assigned to @mikeleppane by @ntBre on 2025-08-05 18:22_

---

_Comment by @prakhar1144 on 2025-11-07 20:30_

I'm working on a fix for this.

(Posting to avoid duplicate work/effort. Thanks!)

---

_Unassigned @mikeleppane by @ntBre on 2025-11-07 20:32_

---

_Assigned to @prakhar1144 by @ntBre on 2025-11-07 20:32_

---

_Closed by @amyreese on 2025-11-22 01:22_

---
