```yaml
number: 2569
title: Enable unnamed requirements for direct URLs
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/bare-ii
created_at: 2024-03-20T18:46:51Z
updated_at: 2024-03-21T03:39:03Z
url: https://github.com/astral-sh/uv/pull/2569
synced_at: 2026-01-12T16:05:06Z
```

# Enable unnamed requirements for direct URLs

---

_@charliermarsh_

## Summary

This PR enables `uv pip install` to accept unnamed requirements, as long as the requirement ends with the wheel or source distribution archive name. For example: `cargo run pip install ~/Downloads/anyio-4.3.0.tar.gz`.

In subsequent PRs, I'll expand the scope of supported archives and patterns.

Part of: https://github.com/astral-sh/uv/issues/313.


---

_Marked ready for review by @charliermarsh on 2024-03-20 19:32_

---

_Label `enhancement` added by @charliermarsh on 2024-03-20 19:32_

---

_Review requested from @zanieb by @charliermarsh on 2024-03-20 23:40_

---

_Review requested from @konstin by @charliermarsh on 2024-03-20 23:40_

---

_Merged by @charliermarsh on 2024-03-21 03:39_

---

_Closed by @charliermarsh on 2024-03-21 03:39_

---

_Branch deleted on 2024-03-21 03:39_

---
