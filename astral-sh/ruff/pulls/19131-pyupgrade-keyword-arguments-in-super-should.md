```yaml
number: 19131
title: "[`pyupgrade`] Keyword arguments in `super` should suppress the `UP008` fix"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-19096
created_at: 2025-07-03T21:25:06Z
updated_at: 2025-07-09T19:20:58Z
url: https://github.com/astral-sh/ruff/pull/19131
synced_at: 2026-01-10T18:33:12Z
```

# [`pyupgrade`] Keyword arguments in `super` should suppress the `UP008` fix

---

_Pull request opened by @danparizher on 2025-07-03 21:25_

## Summary

Fixes #19096


---

_Comment by @github-actions[bot] on 2025-07-03 21:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-07-04 19:51_

---

_Label `fixes` added by @ntBre on 2025-07-04 19:51_

---

_@ntBre approved on 2025-07-04 20:01_

Thanks! I think this is totally reasonable, but another option would be still to emit the diagnostic but suppress the fix. What do you think?

The main benefit there is that if you get another diagnostic, maybe from a type checker, telling you to remove the kwargs, you don't get a new diagnostic from Ruff afterwards. I think the only real downside is that we'd have to downgrade the `FIX_AVAILABILITY` to `Sometimes` here.

---

_Comment by @danparizher on 2025-07-04 21:06_

I think that's a better approach - thanks for the feedback, I went ahead and implemented the changes.

---

_@ntBre approved on 2025-07-09 19:12_

Thanks!

---

_Renamed from "[`pyupgrade`] Keyword arguments in `super` should suppress the UP008 fix" to "[`pyupgrade`] Keyword arguments in `super` should suppress the `UP008` fix" by @ntBre on 2025-07-09 19:13_

---

_Merged by @ntBre on 2025-07-09 19:13_

---

_Closed by @ntBre on 2025-07-09 19:13_

---

_Branch deleted on 2025-07-09 19:20_

---
