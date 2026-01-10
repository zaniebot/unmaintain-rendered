```yaml
number: 4382
title: "Ignore query errors during `uv toolchain list`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: zb/list-error
created_at: 2024-06-18T14:11:32Z
updated_at: 2024-06-18T14:53:00Z
url: https://github.com/astral-sh/uv/pull/4382
synced_at: 2026-01-10T13:54:02Z
```

# Ignore query errors during `uv toolchain list`

---

_Pull request opened by @zanieb on 2024-06-18 14:11_

Closes #4380 

~This is the same logic as `should_stop_discovery` but I changed the log level and duplicated it because I don't really want that method to be public. Maybe it should be though?~

---

_Label `bug` added by @zanieb on 2024-06-18 14:11_

---

_Label `preview` added by @zanieb on 2024-06-18 14:11_

---

_Marked ready for review by @zanieb on 2024-06-18 14:16_

---

_Review requested from @BurntSushi by @zanieb on 2024-06-18 14:16_

---

_Review requested from @konstin by @zanieb on 2024-06-18 14:16_

---

_@BurntSushi approved on 2024-06-18 14:39_

LGTM. I think this code looks like it has enough details where it might be worth making it DRY, but I don't feel strongly.

---

_Comment by @zanieb on 2024-06-18 14:43_

Refactoring...

---

_Merged by @zanieb on 2024-06-18 14:52_

---

_Closed by @zanieb on 2024-06-18 14:53_

---

_Branch deleted on 2024-06-18 14:53_

---
