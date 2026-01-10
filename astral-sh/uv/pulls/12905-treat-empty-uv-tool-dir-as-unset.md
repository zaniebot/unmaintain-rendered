```yaml
number: 12905
title: "Treat empty `UV_TOOL_DIR` as unset"
type: pull_request
state: merged
author: zanieb
labels:
  - breaking
assignees: []
merged: true
base: release/070
head: zb/tool-dir-parse
created_at: 2025-04-15T20:44:48Z
updated_at: 2025-04-17T16:36:07Z
url: https://github.com/astral-sh/uv/pull/12905
synced_at: 2026-01-10T11:10:40Z
```

# Treat empty `UV_TOOL_DIR` as unset

---

_Pull request opened by @zanieb on 2025-04-15 20:44_

Closes https://github.com/astral-sh/uv/issues/8608

---

_Label `breaking` added by @zanieb on 2025-04-15 20:44_

---

_Added to milestone `v0.7.0` by @zanieb on 2025-04-15 20:50_

---

_Comment by @konstin on 2025-04-16 07:41_

Do we intend to do this for all path variables? The Google Colab bug is caused by the same problem but for the constraints file and the prerelease setting.

---

_Comment by @zanieb on 2025-04-16 19:41_

I guess so? Right now, this uses the pwd which is arguably worse than a failure.

---

_Review requested from @konstin by @zanieb on 2025-04-16 19:41_

---

_@konstin approved on 2025-04-17 15:44_

---

_Merged by @zanieb on 2025-04-17 16:36_

---

_Closed by @zanieb on 2025-04-17 16:36_

---

_Branch deleted on 2025-04-17 16:36_

---
