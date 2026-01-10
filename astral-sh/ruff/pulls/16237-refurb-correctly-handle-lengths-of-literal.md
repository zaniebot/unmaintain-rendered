```yaml
number: 16237
title: "[`refurb`] Correctly handle lengths of literal strings in `slice-to-remove-prefix-or-suffix` (`FURB188`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
assignees: []
merged: true
base: main
head: removeaffix-literal-len
created_at: 2025-02-18T18:29:42Z
updated_at: 2025-02-18T18:52:26Z
url: https://github.com/astral-sh/ruff/pull/16237
synced_at: 2026-01-10T19:57:23Z
```

# [`refurb`] Correctly handle lengths of literal strings in `slice-to-remove-prefix-or-suffix` (`FURB188`)

---

_Pull request opened by @dylwil3 on 2025-02-18 18:29_

Fixes false negative when slice bound uses length of string literal.

We were meant to check the following, for example. Given:

```python
  text[:bound] if text.endswith(suffix) else text
```
We want to know whether:
   - `suffix` is a string literal and `bound` is a number literal
   - `suffix` is an expression and `bound` is
       exactly `-len(suffix)` (as AST nodes, prior to evaluation.)
       
The issue is that negative number literals like `-10` are stored as unary operators applied to a number literal in the AST. So when `suffix` was a string literal but `bound` was `-len(suffix)` we were getting caught in the match arm where `bound` needed to be a number. This is now fixed with a guard.


Closes #16231


---

_Label `bug` added by @dylwil3 on 2025-02-18 18:29_

---

_Comment by @github-actions[bot] on 2025-02-18 18:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2025-02-18 18:37_

---

_Merged by @dylwil3 on 2025-02-18 18:52_

---

_Closed by @dylwil3 on 2025-02-18 18:52_

---
