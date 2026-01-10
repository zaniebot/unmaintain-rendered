```yaml
number: 16888
title: Cache source reads during resolution
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/source-cache
created_at: 2025-11-28T22:32:40Z
updated_at: 2025-12-02T09:38:58Z
url: https://github.com/astral-sh/uv/pull/16888
synced_at: 2026-01-10T05:49:14Z
```

# Cache source reads during resolution

---

_Pull request opened by @charliermarsh on 2025-11-28 22:32_

## Summary

If you have requirements files that are included multiple times, we can avoid going back to disk. This also guards against accidental repeated reads on standard input streams.


---

_Review requested from @zanieb by @charliermarsh on 2025-11-28 22:50_

---

_Review requested from @konstin by @charliermarsh on 2025-11-28 22:50_

---

_Marked ready for review by @charliermarsh on 2025-11-28 22:50_

---

_Comment by @charliermarsh on 2025-11-28 22:51_

This isn't actually necessary for https://github.com/astral-sh/uv/pull/16889, but I implemented it first and it seems not-wrong.

---

_@konstin approved on 2025-12-01 08:36_

---

_Comment by @zanieb on 2025-12-02 09:25_

Ah gee. I didn't notice it was a stack. Sorry. Will fix.

---

_Merged by @zanieb on 2025-12-02 09:38_

---

_Closed by @zanieb on 2025-12-02 09:38_

---

_Branch deleted on 2025-12-02 09:38_

---
