```yaml
number: 18931
title: Move big rule implementations
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - internal
assignees: []
merged: true
base: main
head: move-big-rule-imeplementations
created_at: 2025-06-25T04:22:07Z
updated_at: 2025-06-25T15:47:27Z
url: https://github.com/astral-sh/ruff/pull/18931
synced_at: 2026-01-10T18:39:09Z
```

# Move big rule implementations

---

_Pull request opened by @MeGaGiGaGon on 2025-06-25 04:22_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Here's the part that was split out of #18906. I wanted to move these into the rule files since the rest of the rules in `deferred_scope`/`statement` have that same structure of implementations being in the rule definition file. It also resolves the dilemma of where to put the comment, at least for these rules.

## Test Plan

<!-- How was it tested? -->

N/A, no test/functionality affected

---

_Comment by @github-actions[bot] on 2025-06-25 04:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "Move big rule imeplementations" to "Move big rule implementations" by @AlexWaygood on 2025-06-25 05:04_

---

_Label `internal` added by @MichaReiser on 2025-06-25 07:05_

---

_@MichaReiser approved on 2025-06-25 07:06_

---

_Comment by @MichaReiser on 2025-06-25 07:06_

I let @ntBre have the final review on this. Looks reasonable to me.

---

_@ntBre approved on 2025-06-25 14:46_

Thank you!

---

_Merged by @ntBre on 2025-06-25 14:46_

---

_Closed by @ntBre on 2025-06-25 14:46_

---

_Branch deleted on 2025-06-25 15:47_

---
