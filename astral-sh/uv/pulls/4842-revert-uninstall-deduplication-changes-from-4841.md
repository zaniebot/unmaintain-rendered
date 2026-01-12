```yaml
number: 4842
title: "Revert `uninstall` deduplication changes from #4841"
type: pull_request
state: merged
author: j178
labels:
  - internal
assignees: []
merged: true
base: main
head: revert-uninstall-dedeup
created_at: 2024-07-06T05:22:15Z
updated_at: 2024-07-07T02:28:39Z
url: https://github.com/astral-sh/uv/pull/4842
synced_at: 2026-01-12T16:06:29Z
```

# Revert `uninstall` deduplication changes from #4841

---

_@j178_

`matching_installations` is BTreeSet already, no need to deduplicate it.

https://github.com/astral-sh/uv/pull/4841#discussion_r1667241722

---

_Review requested from @zanieb by @charliermarsh on 2024-07-06 19:16_

---

_Merged by @charliermarsh on 2024-07-06 19:16_

---

_Closed by @charliermarsh on 2024-07-06 19:16_

---

_Label `internal` added by @charliermarsh on 2024-07-06 19:16_

---

_Branch deleted on 2024-07-07 02:28_

---
