```yaml
number: 18936
title: "[`pylint`] Allow fix with comments and document performance implications (`PLW3301`)"
type: pull_request
state: merged
author: ntBre
labels:
  - documentation
  - rule
assignees: []
merged: true
base: main
head: brent/plw3301
created_at: 2025-06-25T13:19:18Z
updated_at: 2025-06-25T13:29:25Z
url: https://github.com/astral-sh/ruff/pull/18936
synced_at: 2026-01-10T18:39:09Z
```

# [`pylint`] Allow fix with comments and document performance implications (`PLW3301`)

---

_Pull request opened by @ntBre on 2025-06-25 13:19_

Summary
--

Closes #18849 by adding a `## Known issues` section describing the potential performance issues when fixing nested iterables. I also deleted the comment check since the fix is already unsafe and added a note to the `## Fix safety` docs.

Test Plan
--

Existing tests, updated to allow a fix when comments are present since the fix is already unsafe.

---

_Label `documentation` added by @ntBre on 2025-06-25 13:19_

---

_Label `rule` added by @ntBre on 2025-06-25 13:19_

---

_Review requested from @MichaReiser by @ntBre on 2025-06-25 13:19_

---

_@MichaReiser approved on 2025-06-25 13:25_

thank you

---

_Comment by @github-actions[bot] on 2025-06-25 13:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @ntBre on 2025-06-25 13:29_

---

_Closed by @ntBre on 2025-06-25 13:29_

---

_Branch deleted on 2025-06-25 13:29_

---
