```yaml
number: 10634
title: Add tests for list comprehension
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/test-list-comp
created_at: 2024-03-27T19:53:18Z
updated_at: 2024-03-29T08:39:10Z
url: https://github.com/astral-sh/ruff/pull/10634
synced_at: 2026-01-12T15:55:32Z
```

# Add tests for list comprehension

---

_@dhruvmanila_

## Summary

This PR adds tests for list comprehension.

It also adds a new parser error type which will be raised if a starred expression is being used in a comprehension.

```py
  [*x for x in y]
#  ^^
#  iterable unpacking cannot be used in a comprehension
```


---

_Label `parser` added by @dhruvmanila on 2024-03-27 19:53_

---

_Label `testing` added by @dhruvmanila on 2024-03-27 19:53_

---

_Marked ready for review by @dhruvmanila on 2024-03-28 06:56_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-28 06:56_

---

_@MichaReiser approved on 2024-03-28 08:32_

You packing the changes into small PRs makes it really nice to review them. I appreciate it, because I know it's a lot of work to keep the changes separate, stacking the PRs etc. Thank you

---

_Merged by @dhruvmanila on 2024-03-29 08:20_

---

_Closed by @dhruvmanila on 2024-03-29 08:20_

---

_Branch deleted on 2024-03-29 08:20_

---

_Comment by @github-actions[bot] on 2024-03-29 08:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
