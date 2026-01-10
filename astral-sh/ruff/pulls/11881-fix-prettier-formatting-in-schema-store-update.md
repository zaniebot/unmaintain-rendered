```yaml
number: 11881
title: Fix prettier formatting in schema store update script
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/update-schemastore
created_at: 2024-06-14T21:47:59Z
updated_at: 2024-06-14T21:56:33Z
url: https://github.com/astral-sh/ruff/pull/11881
synced_at: 2026-01-10T21:56:00Z
```

# Fix prettier formatting in schema store update script

---

_Pull request opened by @zanieb on 2024-06-14 21:47_

How was this working for anyone else? The `prettier` path did not exist on my machine. Also added `--force` to the push because otherwise you can't re-run the script for a given Ruff commit.

---

_Label `internal` added by @zanieb on 2024-06-14 21:48_

---

_Renamed from "Fix formatting of schemastore" to "Fix prettier formatting in schema store update script" by @zanieb on 2024-06-14 21:48_

---

_Marked ready for review by @zanieb on 2024-06-14 21:50_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-14 21:55_

---

_Review requested from @AlexWaygood by @zanieb on 2024-06-14 21:55_

---

_@snowsignal approved on 2024-06-14 21:56_

Confirmed that this fixes the script 👍 

---

_Comment by @zanieb on 2024-06-14 21:56_

@snowsignal reproduced the error I was encountering and that this fixes it!

---

_Comment by @zanieb on 2024-06-14 21:56_

:D

---

_Merged by @zanieb on 2024-06-14 21:56_

---

_Closed by @zanieb on 2024-06-14 21:56_

---

_Branch deleted on 2024-06-14 21:56_

---
