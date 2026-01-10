```yaml
number: 19609
title: "Add `Checker::context` method, deduplicate Unicode checks"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/ambiguous-unicode-characters
created_at: 2025-07-29T01:09:02Z
updated_at: 2025-07-29T20:07:57Z
url: https://github.com/astral-sh/ruff/pull/19609
synced_at: 2026-01-10T17:58:13Z
```

# Add `Checker::context` method, deduplicate Unicode checks

---

_Pull request opened by @ntBre on 2025-07-29 01:09_

Summary
--

This PR adds a `Checker::context` method that returns the underlying `LintContext` to unify `Candidate::into_diagnostic` and `Candidate::report_diagnostic` in our ambiguous Unicode character checks. This avoids some duplication and also avoids collecting a `Vec` of `Candidate`s only to iterate over it later.

Test Plan
--

Existing tests

---

_Label `internal` added by @ntBre on 2025-07-29 01:09_

---

_Comment by @github-actions[bot] on 2025-07-29 01:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-07-29 06:29_

It seems to me that the only problem is that you can't go from `Checker` to `LintContext` but I don't see a reason why that should be, given that `Checker` stores a `LintContext`. Is there something that prevents us from adding a `context()` method to `Checker` that returns the `LintContext`?

---

_Comment by @ntBre on 2025-07-29 12:07_

I think we previously wanted to avoid providing two paths to `report_diagnostic` (and now `settings`), at least that's what I remember somewhat vaguely. That's definitely simpler if we don't mind adding the method, though!

---

_Renamed from "Add `DiagnosticReporter` trait, deduplicate Unicode checks" to "Add `Checker::context` method, deduplicate Unicode checks" by @ntBre on 2025-07-29 19:18_

---

_Marked ready for review by @ntBre on 2025-07-29 19:22_

---

_@MichaReiser approved on 2025-07-29 19:37_

Nice

---

_Merged by @ntBre on 2025-07-29 20:07_

---

_Closed by @ntBre on 2025-07-29 20:07_

---

_Branch deleted on 2025-07-29 20:07_

---
