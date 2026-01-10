```yaml
number: 7709
title: Escape glob patterns
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/globwalk-workspace
created_at: 2024-09-26T14:29:32Z
updated_at: 2024-09-26T14:54:30Z
url: https://github.com/astral-sh/uv/pull/7709
synced_at: 2026-01-10T12:53:53Z
```

# Escape glob patterns

---

_Pull request opened by @konstin on 2024-09-26 14:29_

Previously, we could have interpreted the base directory as part of the glob.

I initially tried to use globwalk, which also supports using multiple patterns at once, but it always seems to descend and not stop at emitting the exact matched directory, it also emits the children.

---

_Review requested from @BurntSushi by @konstin on 2024-09-26 14:29_

---

_Label `bug` added by @konstin on 2024-09-26 14:29_

---

_Converted to draft by @konstin on 2024-09-26 14:31_

---

_Renamed from "Use globwalk to use base dir with glob" to "Escape glob patterns" by @konstin on 2024-09-26 14:46_

---

_Marked ready for review by @konstin on 2024-09-26 14:52_

---

_Merged by @konstin on 2024-09-26 14:54_

---

_Closed by @konstin on 2024-09-26 14:54_

---

_Branch deleted on 2024-09-26 14:54_

---
