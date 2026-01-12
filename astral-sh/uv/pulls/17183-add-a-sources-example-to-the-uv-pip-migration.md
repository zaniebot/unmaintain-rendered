```yaml
number: 17183
title: Add a sources example to the uv pip migration guide
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/sources-example-i
created_at: 2025-12-18T22:41:07Z
updated_at: 2025-12-19T14:07:41Z
url: https://github.com/astral-sh/uv/pull/17183
synced_at: 2026-01-12T16:12:39Z
```

# Add a sources example to the uv pip migration guide

---

_@zanieb_

Documents https://github.com/astral-sh/uv/issues/6275

---

_Label `documentation` added by @zanieb on 2025-12-18 22:42_

---

_Marked ready for review by @zanieb on 2025-12-18 23:02_

---

_@EliteTK reviewed on 2025-12-19 12:27_

---

_Review comment by @EliteTK on `docs/guides/migration/pip-to-project.md`:442 on 2025-12-19 12:27_

"e.g." and "as in the following file" are mutually redundant

```suggestion
When importing requirements on local paths or Git repositories, for example:
```

---

_@EliteTK approved on 2025-12-19 12:27_

---

_@konstin reviewed on 2025-12-19 13:56_

---

_Review comment by @konstin on `docs/guides/migration/pip-to-project.md`:463 on 2025-12-19 13:56_

```suggestion
editable-path-dep = { path = "./editable-path-dep", editable = true }
```

---

_@konstin approved on 2025-12-19 13:56_

---

_Merged by @zanieb on 2025-12-19 14:07_

---

_Closed by @zanieb on 2025-12-19 14:07_

---

_Branch deleted on 2025-12-19 14:07_

---
