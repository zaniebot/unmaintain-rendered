```yaml
number: 11487
title: "Respect `UV_PYTHON` in `uv python install`"
type: pull_request
state: merged
author: zanieb
labels:
  - breaking
assignees: []
merged: true
base: tracking/060
head: zb/python-install-var
created_at: 2025-02-13T18:42:54Z
updated_at: 2025-02-13T20:59:01Z
url: https://github.com/astral-sh/uv/pull/11487
synced_at: 2026-01-10T11:10:38Z
```

# Respect `UV_PYTHON` in `uv python install`

---

_Pull request opened by @zanieb on 2025-02-13 18:42_

Unlike https://github.com/astral-sh/uv/pull/10222, this does not respect `UV_PYTHON` in `uv python uninstall` (continuing to require an explicit target there) which I think is simpler and matches our `.python-version` file behavior.

---

_Added to milestone `v0.6.0` by @zanieb on 2025-02-13 18:49_

---

_Label `breaking` added by @zanieb on 2025-02-13 18:49_

---

_Marked ready for review by @zanieb on 2025-02-13 18:50_

---

_Review requested from @Gankra by @zanieb on 2025-02-13 19:37_

---

_Review requested from @charliermarsh by @zanieb on 2025-02-13 19:37_

---

_@Gankra approved on 2025-02-13 20:47_

---

_Merged by @zanieb on 2025-02-13 20:58_

---

_Closed by @zanieb on 2025-02-13 20:58_

---

_Branch deleted on 2025-02-13 20:59_

---
