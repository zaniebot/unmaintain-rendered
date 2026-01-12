```yaml
number: 10553
title: Complete end expression token set
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/end-expr-set
created_at: 2024-03-25T03:14:16Z
updated_at: 2024-03-25T12:39:42Z
url: https://github.com/astral-sh/ruff/pull/10553
synced_at: 2026-01-12T15:55:32Z
```

# Complete end expression token set

---

_@dhruvmanila_

## Summary

This PR completes the end expression set which is a set of tokens that can come after an expression. These were taken from the [Python Grammar] searching for the `expression` and looking at the kind of tokens after that.

It also renames the `parse_expression` to `parse_expressions` as the corresponding grammar is called "expressions".



---

_Label `parser` added by @dhruvmanila on 2024-03-25 03:14_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-25 03:14_

---

_@MichaReiser approved on 2024-03-25 08:36_

---

_Merged by @dhruvmanila on 2024-03-25 12:39_

---

_Closed by @dhruvmanila on 2024-03-25 12:39_

---

_Branch deleted on 2024-03-25 12:39_

---
