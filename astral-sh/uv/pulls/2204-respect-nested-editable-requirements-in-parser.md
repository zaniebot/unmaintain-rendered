```yaml
number: 2204
title: Respect nested editable requirements in parser
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ed
created_at: 2024-03-05T14:32:12Z
updated_at: 2024-03-05T16:47:38Z
url: https://github.com/astral-sh/uv/pull/2204
synced_at: 2026-01-12T16:04:55Z
```

# Respect nested editable requirements in parser

---

_@charliermarsh_

Closes https://github.com/astral-sh/uv/issues/2198.

---

_Label `bug` added by @charliermarsh on 2024-03-05 14:32_

---

_Marked ready for review by @charliermarsh on 2024-03-05 14:32_

---

_@charliermarsh reviewed on 2024-03-05 14:32_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:474 on 2024-03-05 14:32_

We missed incorporating these extra fields when they were added to `RequirementsTxt`.

---

_@zanieb approved on 2024-03-05 14:59_

---

_Merged by @charliermarsh on 2024-03-05 16:47_

---

_Closed by @charliermarsh on 2024-03-05 16:47_

---

_Branch deleted on 2024-03-05 16:47_

---
