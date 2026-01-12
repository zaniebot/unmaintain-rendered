```yaml
number: 18291
title: "[pyupgrade] make fix unsafe if it deletes comments (UP010, unnecessary-future-import)"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
assignees: []
merged: true
base: main
head: main
created_at: 2025-05-24T18:13:10Z
updated_at: 2025-05-25T10:44:21Z
url: https://github.com/astral-sh/ruff/pull/18291
synced_at: 2026-01-12T15:56:16Z
```

# [pyupgrade] make fix unsafe if it deletes comments (UP010, unnecessary-future-import)

---

_@chirizxc_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->
/closes #18282
## Summary

The [`unnecessary-future-import`](https://docs.astral.sh/ruff/rules/unnecessary-future-import/) (UP010) fix should be marked as unsafe when it deletes a comment on the same line as the import. Currently, the fix removes the entire line including comments, which may cause unintended code/comment loss. This change ensures that such fixes are flagged as unsafe and require manual review.

## Test Plan

<!-- How was it tested? -->


---

_Label `fixes` added by @MichaReiser on 2025-05-25 10:21_

---

_@MichaReiser approved on 2025-05-25 10:22_

Thank you

---

_Comment by @github-actions[bot] on 2025-05-25 10:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @MichaReiser on 2025-05-25 10:44_

---

_Closed by @MichaReiser on 2025-05-25 10:44_

---
