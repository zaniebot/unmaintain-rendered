```yaml
number: 14613
title: "Add missing comma in `projects/dependencies.md`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - documentation
assignees: []
merged: true
base: main
head: typo
created_at: 2025-07-14T18:53:26Z
updated_at: 2025-07-14T19:21:45Z
url: https://github.com/astral-sh/uv/pull/14613
synced_at: 2026-01-12T16:11:18Z
```

# Add missing comma in `projects/dependencies.md`

---

_@InSyncWithFoo_

## Summary

Diff:

```diff
 [dependency-groups]
 dev = [
-  {include-group = "lint"}
+  {include-group = "lint"},
   {include-group = "test"}
 ]
```

## Test Plan

None.


---

_@zanieb approved on 2025-07-14 19:06_

Thank you!

---

_Merged by @zanieb on 2025-07-14 19:06_

---

_Closed by @zanieb on 2025-07-14 19:06_

---

_Label `documentation` added by @zanieb on 2025-07-14 19:06_

---

_Branch deleted on 2025-07-14 19:21_

---
