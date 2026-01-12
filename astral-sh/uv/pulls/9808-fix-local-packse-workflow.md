```yaml
number: 9808
title: " Fix local packse workflow "
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/fix-packse-workflow
created_at: 2024-12-11T12:06:34Z
updated_at: 2024-12-11T15:32:48Z
url: https://github.com/astral-sh/uv/pull/9808
synced_at: 2026-01-12T16:08:59Z
```

#  Fix local packse workflow 

---

_@konstin_

Make the local packse workflow work again:

```
# In packse:
uv run --extra index --extra serve packse serve --no-hash scenarios &
# In uv:
UV_TEST_INDEX_URL="http://localhost:3141/simple/" ./scripts/scenarios/generate.py
```

Bugs fixed:
* The default scenario pattern didn't match anything.
* The snapshot update test command was wrong since the test centralization
* Snapshot update failures would not be reported

---

_Label `internal` added by @konstin on 2024-12-11 12:06_

---

_Review requested from @zanieb by @konstin on 2024-12-11 12:06_

---

_@zanieb approved on 2024-12-11 15:32_

---

_Merged by @zanieb on 2024-12-11 15:32_

---

_Closed by @zanieb on 2024-12-11 15:32_

---

_Branch deleted on 2024-12-11 15:32_

---
