```yaml
number: 14754
title: "Add tests for \"keyword as identifier\" syntax errors"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/keyword-as-identifier
created_at: 2024-12-03T11:25:28Z
updated_at: 2024-12-05T12:02:51Z
url: https://github.com/astral-sh/ruff/pull/14754
synced_at: 2026-01-10T20:42:27Z
```

# Add tests for "keyword as identifier" syntax errors

---

_Pull request opened by @dhruvmanila on 2024-12-03 11:25_

## Summary

This is related to #13778, more specifically https://github.com/astral-sh/ruff/issues/13778#issuecomment-2513556004.

This PR adds various test cases where a keyword is being where an identifier is expected. The tests are to make sure that red knot doesn't panic, raises the syntax error and the identifier is added to the symbol table. The final part allows editor related features like renaming the symbol.


---

_Label `red-knot` added by @dhruvmanila on 2024-12-03 11:25_

---

_Closed by @dhruvmanila on 2024-12-03 12:53_

---

_Reopened by @dhruvmanila on 2024-12-03 12:53_

---

_Comment by @github-actions[bot] on 2024-12-03 12:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dhruvmanila on 2024-12-05 09:23_

---

_Review requested from @carljm by @dhruvmanila on 2024-12-05 09:23_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-12-05 09:23_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-12-05 09:23_

---

_Review requested from @sharkdp by @dhruvmanila on 2024-12-05 09:23_

---

_@MichaReiser reviewed on 2024-12-05 09:59_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/invalid_syntax.md`:29 on 2024-12-05 09:59_

That's interesting

---

_@MichaReiser approved on 2024-12-05 10:00_

Thank you

---

_Merged by @dhruvmanila on 2024-12-05 12:02_

---

_Closed by @dhruvmanila on 2024-12-05 12:02_

---

_Branch deleted on 2024-12-05 12:02_

---
