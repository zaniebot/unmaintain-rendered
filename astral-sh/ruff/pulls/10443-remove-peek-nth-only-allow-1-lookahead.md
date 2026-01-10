```yaml
number: 10443
title: "Remove `peek_nth`, only allow 1 lookahead"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/remove-peek-nth
created_at: 2024-03-18T05:07:38Z
updated_at: 2024-03-20T17:31:56Z
url: https://github.com/astral-sh/ruff/pull/10443
synced_at: 2026-01-10T22:47:02Z
```

# Remove `peek_nth`, only allow 1 lookahead

---

_Pull request opened by @dhruvmanila on 2024-03-18 05:07_

## Summary

This PR updates the parser to avoid having the ability to do infinite lookaheads. We replace this with a new `peek` method introduced in #10418. The `peek_nth` method was only used in the old with item parsing logic which used this to determine the with item kind without consuming the tokens and only then perform the actual parsing.


---

_Label `parser` added by @dhruvmanila on 2024-03-18 05:07_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-18 05:07_

---

_Comment by @github-actions[bot] on 2024-03-18 05:27_

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

_@MichaReiser approved on 2024-03-18 07:39_

---

_Merged by @dhruvmanila on 2024-03-20 17:12_

---

_Closed by @dhruvmanila on 2024-03-20 17:12_

---

_Branch deleted on 2024-03-20 17:12_

---
