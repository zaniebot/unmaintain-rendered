---
number: 7691
title: Update logical line rules for the new f-string tokens
type: issue
state: closed
author: dhruvmanila
labels:
  - rule
  - python312
assignees: []
created_at: 2023-09-28T03:59:43Z
updated_at: 2023-09-28T05:22:17Z
url: https://github.com/astral-sh/ruff/issues/7691
synced_at: 2026-01-07T13:12:15-06:00
---

# Update logical line rules for the new f-string tokens

---

_Issue opened by @dhruvmanila on 2023-09-28 03:59_

- [x] Ignore `E225` for the `=` operator
- [x] Ignore `E231` for the `:` operator
- [x] Ignore `E201`, `E202` for `{` and `}` tokens

I think these shouldn't be blocking for the release as the rules are in preview but also it's difficult to differentiate between usages of certain tokens (`:`, `{`) in the context of f-strings.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-09-28 03:59_

---

_Label `rule` added by @dhruvmanila on 2023-09-28 05:11_

---

_Label `python312` added by @dhruvmanila on 2023-09-28 05:11_

---

_Closed by @dhruvmanila on 2023-09-28 05:22_

---
