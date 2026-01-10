```yaml
number: 15523
title: "[`pyflakes`] Show syntax error message for `F722`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - rule
  - diagnostics
assignees: []
merged: true
base: main
head: dhruv/annotation-syntax-error
created_at: 2025-01-16T06:52:59Z
updated_at: 2025-01-16T16:20:59Z
url: https://github.com/astral-sh/ruff/pull/15523
synced_at: 2026-01-10T20:34:00Z
```

# [`pyflakes`] Show syntax error message for `F722`

---

_Pull request opened by @dhruvmanila on 2025-01-16 06:52_

## Summary

Ref: https://github.com/astral-sh/ruff/pull/15387#discussion_r1917796907

This PR updates `F722` to show syntax error message instead of the string content.

I think it's more useful to show the syntax error message than the string content. In the future, when the diagnostics renderer is more capable, we could even highlight the exact location of the syntax error along with the annotation string.

This is also in line with how we show the diagnostic in red knot.

## Test Plan

Update existing test snapshots.


---

_Label `rule` added by @dhruvmanila on 2025-01-16 06:52_

---

_Label `diagnostics` added by @dhruvmanila on 2025-01-16 06:52_

---

_Review requested from @carljm by @dhruvmanila on 2025-01-16 06:53_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-01-16 06:53_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-01-16 06:53_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-01-16 06:53_

---

_Comment by @dhruvmanila on 2025-01-16 06:53_

(Tagging @BurntSushi as it's related to diagnostics.)

---

_Comment by @github-actions[bot] on 2025-01-16 06:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-01-16 07:05_

Nice. 

the ideal result here might be to actually support diagnostic chains, where a diagnostic can have an inner diagnostic.

---

_Merged by @dhruvmanila on 2025-01-16 07:14_

---

_Closed by @dhruvmanila on 2025-01-16 07:14_

---

_Branch deleted on 2025-01-16 07:14_

---

_@BurntSushi reviewed on 2025-01-16 16:20_

Nice! LGTM!

---
