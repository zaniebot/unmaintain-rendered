```yaml
number: 2568
title: "Allows UP030 to work better with *args and **kwargs"
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: add_to_up30
created_at: 2023-02-04T15:15:29Z
updated_at: 2023-02-04T22:35:15Z
url: https://github.com/astral-sh/ruff/pull/2568
synced_at: 2026-01-12T15:55:08Z
```

# Allows UP030 to work better with *args and **kwargs

---

_@colin99d_

Fixes #2448

---

_Renamed from "Add to up30" to "Allows UP030 to work better with *args and **kwargs" by @colin99d on 2023-02-04 16:22_

---

_Merged by @charliermarsh on 2023-02-04 22:34_

---

_Closed by @charliermarsh on 2023-02-04 22:34_

---

_@charliermarsh reviewed on 2023-02-04 22:35_

---

_Review comment by @charliermarsh on `src/rules/pyupgrade/rules/format_literals.rs`:96 on 2023-02-04 22:35_

@colin99d - Happy to follow-up and correct if I'm wrong, but I think it's sufficient to just do this check, and nothing else?

---
