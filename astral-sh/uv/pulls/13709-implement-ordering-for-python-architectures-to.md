```yaml
number: 13709
title: Implement ordering for Python architectures to prefer native installations
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/ord-arch
created_at: 2025-05-28T20:47:38Z
updated_at: 2025-05-29T19:06:34Z
url: https://github.com/astral-sh/uv/pull/13709
synced_at: 2026-01-12T16:10:49Z
```

# Implement ordering for Python architectures to prefer native installations

---

_@zanieb_

Resolves https://github.com/astral-sh/uv/pull/13474#discussion_r2112586405

This kind of dynamic ordering freaks me out a little, but I think it's probably the best solution and is static at compile-time.

Currently, we're just sorting by the stringified representation! which is just convenient for reproducibility, but we rely on these orderings for prioritization in discovery.

---

_Review requested from @Gankra by @zanieb on 2025-05-28 20:58_

---

_Assigned to @Gankra by @zanieb on 2025-05-28 20:58_

---

_@zanieb reviewed on 2025-05-28 20:58_

---

_Review comment by @zanieb on `crates/uv-python/src/platform.rs`:43 on 2025-05-28 20:58_

Actually ensuring the variant orderings are correct will be part of #9788 

---

_Marked ready for review by @zanieb on 2025-05-28 20:59_

---

_@Gankra approved on 2025-05-29 17:55_

---

_Comment by @zanieb on 2025-05-29 17:58_

Rebased onto main, which drops the diff to the snapshot but is the "proper" ordering for merges.

---

_Merged by @zanieb on 2025-05-29 19:06_

---

_Closed by @zanieb on 2025-05-29 19:06_

---

_Branch deleted on 2025-05-29 19:06_

---
