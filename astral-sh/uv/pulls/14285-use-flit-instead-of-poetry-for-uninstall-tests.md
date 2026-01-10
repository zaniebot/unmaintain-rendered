```yaml
number: 14285
title: Use Flit instead of Poetry for uninstall tests
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/uninstall-edit
created_at: 2025-06-26T16:54:26Z
updated_at: 2025-06-26T18:10:56Z
url: https://github.com/astral-sh/uv/pull/14285
synced_at: 2026-01-10T11:10:43Z
```

# Use Flit instead of Poetry for uninstall tests

---

_Pull request opened by @zanieb on 2025-06-26 16:54_

Investigating https://github.com/astral-sh/uv/issues/14158

---

_Label `testing` added by @zanieb on 2025-06-26 16:54_

---

_Comment by @zanieb on 2025-06-26 17:33_

I think these tests are just faster like this, and we don't need Poetry

```
        PASS [   2.921s] uv::it pip_uninstall::uninstall_duplicate_by_path
        PASS [   3.404s] uv::it pip_uninstall::uninstall_duplicate
```

---

_Marked ready for review by @zanieb on 2025-06-26 17:33_

---

_Review comment by @konstin on `scripts/packages/flit_editable/pyproject.toml`:9 on 2025-06-26 17:34_

```suggestion
requires-python = ">=3.11"
```

---

_Review comment by @konstin on `scripts/packages/flit_editable/pyproject.toml`:10 on 2025-06-26 17:36_

```suggestion
license = "MIT OR Apache-2.0"
```

---

_@konstin approved on 2025-06-26 17:36_

---

_@zanieb reviewed on 2025-06-26 17:39_

---

_Review comment by @zanieb on `scripts/packages/flit_editable/pyproject.toml`:10 on 2025-06-26 17:39_

I copied these from another flit package in our repository, does this matter?

---

_@zanieb reviewed on 2025-06-26 17:45_

---

_Review comment by @zanieb on `scripts/packages/flit_editable/pyproject.toml`:10 on 2025-06-26 17:45_

(done anyway)

---

_@zanieb reviewed on 2025-06-26 17:59_

---

_Review comment by @zanieb on `scripts/packages/flit_editable/pyproject.toml`:10 on 2025-06-26 17:59_

_sigh_ this actually broke things lol

---

_Merged by @zanieb on 2025-06-26 18:09_

---

_Closed by @zanieb on 2025-06-26 18:09_

---

_Branch deleted on 2025-06-26 18:09_

---

_Review comment by @konstin on `scripts/packages/flit_editable/pyproject.toml`:10 on 2025-06-26 18:10_

I wanted to be a good example with PEP 639, but alas it happened after our exclude newer cutoff https://github.com/pypa/flit/issues/692

---

_@konstin reviewed on 2025-06-26 18:10_

---
