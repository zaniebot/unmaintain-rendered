```yaml
number: 17550
title: "Use `FxHashSet<PackageName>` instead of `Vec` for `exclude` in `uv pip list`"
type: pull_request
state: merged
author: zanieb
labels:
  - performance
  - internal
assignees: []
merged: true
base: main
head: claude/fix-uv-issue-17546-MvKaA
created_at: 2026-01-17T16:26:27Z
updated_at: 2026-01-17T16:45:19Z
url: https://github.com/astral-sh/uv/pull/17550
synced_at: 2026-01-17T17:17:50Z
```

# Use `FxHashSet<PackageName>` instead of `Vec` for `exclude` in `uv pip list`

---

_@zanieb_

Closes #17546 

and use `FxHashSet` instead of `HashSet` in `uv pip freeze`

---

_Marked ready for review by @zanieb on 2026-01-17 16:38_

---

_Label `performance` added by @zanieb on 2026-01-17 16:38_

---

_Label `internal` added by @zanieb on 2026-01-17 16:38_

---

_Merged by @zanieb on 2026-01-17 16:45_

---

_Closed by @zanieb on 2026-01-17 16:45_

---
