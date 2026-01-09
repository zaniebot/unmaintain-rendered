---
number: 7808
title: "Simplify `Q003` implementation"
type: issue
state: closed
author: dhruvmanila
labels:
  - internal
assignees: []
created_at: 2023-10-04T10:51:23Z
updated_at: 2024-04-14T18:14:14Z
url: https://github.com/astral-sh/ruff/issues/7808
synced_at: 2026-01-07T13:12:15-06:00
---

# Simplify `Q003` implementation

---

_Issue opened by @dhruvmanila on 2023-10-04 10:51_

The current implementation of `Q003` rule to accommodate the nested f-strings is a bit too complex. It can possibly be simplified using two loops instead of one. The outer loop is the regular token loop while the inner loop would be for the f-strings. When the outer loop encounters the `FStringStart` token, then the control would go to the inner loop where it would be easier to just `break` if it's not possible to perform the check.

This is low priority and an internal refactor for readability purpose. 

---

_Label `internal` added by @dhruvmanila on 2023-10-04 10:51_

---

_Referenced in [astral-sh/ruff#10923](../../astral-sh/ruff/pulls/10923.md) on 2024-04-14 16:19_

---

_Closed by @dhruvmanila on 2024-04-14 18:14_

---
