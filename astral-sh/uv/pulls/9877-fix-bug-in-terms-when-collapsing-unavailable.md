```yaml
number: 9877
title: Fix bug in terms when collapsing unavailable versions in resolver errors
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-terms-2
created_at: 2024-12-13T19:32:45Z
updated_at: 2024-12-13T21:06:40Z
url: https://github.com/astral-sh/uv/pull/9877
synced_at: 2026-01-12T16:09:01Z
```

# Fix bug in terms when collapsing unavailable versions in resolver errors

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/9861
Closes https://github.com/pubgrub-rs/pubgrub/issues/297

---

_Label `bug` added by @zanieb on 2024-12-13 19:32_

---

_Marked ready for review by @zanieb on 2024-12-13 19:37_

---

_@zanieb reviewed on 2024-12-13 19:48_

---

_Review comment by @zanieb on `crates/uv/tests/it/cache_prune.rs`:269 on 2024-12-13 19:48_

In brief, this "conclusion" is based on the _terms_ which were previously not changed from their original contents during collapse of unavailable package versions. The conclusion was consequently only one part of the overall conclusion, but was being used for the merged nodes.

---

_Review requested from @charliermarsh by @zanieb on 2024-12-13 20:10_

---

_@charliermarsh approved on 2024-12-13 20:57_

---

_Merged by @zanieb on 2024-12-13 21:06_

---

_Closed by @zanieb on 2024-12-13 21:06_

---

_Branch deleted on 2024-12-13 21:06_

---
