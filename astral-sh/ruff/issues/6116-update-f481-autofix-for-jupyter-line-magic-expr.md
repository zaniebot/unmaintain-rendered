```yaml
number: 6116
title: "Update `F481` autofix for Jupyter line magic expr"
type: issue
state: closed
author: dhruvmanila
labels:
  - rule
assignees: []
created_at: 2023-07-27T05:15:47Z
updated_at: 2023-08-05T00:45:03Z
url: https://github.com/astral-sh/ruff/issues/6116
synced_at: 2026-01-12T15:54:45Z
```

# Update `F481` autofix for Jupyter line magic expr

---

_@dhruvmanila_

If the assignment value is a line magic expression, the statement shouldn't be removed entirely i.e., the assignment statement should be refactored to a line magic statement:

```diff
- dir = !pwd
+ !pwd
```

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-07-27 05:16_

---

_Label `rule` added by @dhruvmanila on 2023-07-27 05:16_

---

_Closed by @dhruvmanila on 2023-08-05 00:45_

---
