```yaml
number: 20642
title: "[`cli`] Add conflict between `--add-noqa` and `--diff` options"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-20609
created_at: 2025-09-29T23:53:48Z
updated_at: 2025-09-30T10:57:07Z
url: https://github.com/astral-sh/ruff/pull/20642
synced_at: 2026-01-12T15:57:06Z
```

# [`cli`] Add conflict between `--add-noqa` and `--diff` options

---

_@danparizher_

## Summary

Fixes #20609


---

_Renamed from "[cli] Add conflict between `--add-noqa` and `--diff` options" to "[`cli`] Add conflict between `--add-noqa` and `--diff` options" by @danparizher on 2025-09-29 23:54_

---

_Comment by @danparizher on 2025-09-30 00:06_

I'm not sure if we even want a test for conflicts like this, as I haven't seen any other tests similar to it. Let me know if we want it removed.

---

_Comment by @github-actions[bot] on 2025-09-30 00:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @danparizher on 2025-09-30 00:13_

Actually, I can see that the executable in the CI has different syntax depending on the OS, so I'll just remove the test.

---

_Label `bug` added by @MichaReiser on 2025-09-30 06:34_

---

_Label `fixes` added by @MichaReiser on 2025-09-30 06:34_

---

_Merged by @MichaReiser on 2025-09-30 06:34_

---

_Closed by @MichaReiser on 2025-09-30 06:34_

---

_Branch deleted on 2025-09-30 10:57_

---
