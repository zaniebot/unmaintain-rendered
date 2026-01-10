```yaml
number: 18134
title: "Use `insta` settings instead of `cfg`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: brent/tidy-windows-cfg
created_at: 2025-05-16T14:20:28Z
updated_at: 2025-05-16T14:55:35Z
url: https://github.com/astral-sh/ruff/pull/18134
synced_at: 2026-01-10T18:51:01Z
```

# Use `insta` settings instead of `cfg`

---

_Pull request opened by @ntBre on 2025-05-16 14:20_

Summary
--

I noticed these `cfg` directives while working on diagnostics. I think it makes more sense to apply an `insta` filter in the test instead. I copied this filter from a CLI test for the same rule.

Test Plan
--

Existing tests, especially Windows CI on this PR


---

_Label `internal` added by @ntBre on 2025-05-16 14:20_

---

_Label `testing` added by @ntBre on 2025-05-16 14:20_

---

_Comment by @github-actions[bot] on 2025-05-16 14:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-05-16 14:31_

---

_@MichaReiser approved on 2025-05-16 14:53_

---

_Merged by @ntBre on 2025-05-16 14:55_

---

_Closed by @ntBre on 2025-05-16 14:55_

---

_Branch deleted on 2025-05-16 14:55_

---
