```yaml
number: 362
title: Avoid removing progress bars
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/poison
created_at: 2023-11-07T18:54:37Z
updated_at: 2023-11-07T18:58:18Z
url: https://github.com/astral-sh/uv/pull/362
synced_at: 2026-01-10T15:50:28Z
```

# Avoid removing progress bars

---

_Pull request opened by @charliermarsh on 2023-11-07 18:54_

This was dumb of me. We pass out indexes when adding progress bars, but were then removing entries on completion, so any outstanding indexes were now _invalid_. We just shouldn't remove them. The `MultiProgress` retains a reference anyway, IIUC.

Closes https://github.com/astral-sh/puffin/issues/360.

---

_Label `bug` added by @charliermarsh on 2023-11-07 18:54_

---

_@BurntSushi approved on 2023-11-07 18:57_

---

_Merged by @charliermarsh on 2023-11-07 18:58_

---

_Closed by @charliermarsh on 2023-11-07 18:58_

---

_Branch deleted on 2023-11-07 18:58_

---
