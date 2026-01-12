```yaml
number: 13380
title: Respect default extras in uv remove
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/re
created_at: 2025-05-10T19:09:32Z
updated_at: 2025-05-18T21:28:52Z
url: https://github.com/astral-sh/uv/pull/13380
synced_at: 2026-01-12T16:10:40Z
```

# Respect default extras in uv remove

---

_@charliermarsh_

## Summary

Using "all extras" in `uv remove` will cause errors for projects with conflicting extras. Now that we have a concept of "default extras", it seems better to respect those defaults like we do for dependency groups.

Closes https://github.com/astral-sh/uv/issues/12770.


---

_Review requested from @Gankra by @charliermarsh on 2025-05-10 19:12_

---

_Review requested from @konstin by @charliermarsh on 2025-05-10 19:12_

---

_Label `bug` added by @charliermarsh on 2025-05-10 19:12_

---

_Marked ready for review by @charliermarsh on 2025-05-10 19:12_

---

_Comment by @charliermarsh on 2025-05-10 20:42_

Oh, I guess this needs https://github.com/astral-sh/uv/pull/12965? I didn't realize that the current defaults were a stub, hah.

---

_Merged by @charliermarsh on 2025-05-18 21:28_

---

_Closed by @charliermarsh on 2025-05-18 21:28_

---

_Branch deleted on 2025-05-18 21:28_

---
