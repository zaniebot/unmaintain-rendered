```yaml
number: 15832
title: "[`refurb`] Mark fix as unsafe if there are comments (`FURB171`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: FURB171
created_at: 2025-01-30T17:37:46Z
updated_at: 2025-01-30T23:50:19Z
url: https://github.com/astral-sh/ruff/pull/15832
synced_at: 2026-01-12T15:55:52Z
```

# [`refurb`] Mark fix as unsafe if there are comments (`FURB171`)

---

_@InSyncWithFoo_

## Summary

Resolves #10063 and follow-up to #15521.

The fix is now marked as unsafe if there are any comments within its range. Tests are adapted from that of #15521.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @InSyncWithFoo on 2025-01-30 17:39_

This could be made a little better by not checking for comments within the element's range (they are not removed). Is that desired?

---

_Comment by @github-actions[bot] on 2025-01-30 17:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `fixes` added by @dylwil3 on 2025-01-30 17:46_

---

_Label `preview` added by @dylwil3 on 2025-01-30 17:46_

---

_@dylwil3 approved on 2025-01-30 23:20_

LGTM! I don't think we need to worry about the more refined comments intersection check. Thank you!

---

_Merged by @dylwil3 on 2025-01-30 23:21_

---

_Closed by @dylwil3 on 2025-01-30 23:21_

---

_Branch deleted on 2025-01-30 23:50_

---
