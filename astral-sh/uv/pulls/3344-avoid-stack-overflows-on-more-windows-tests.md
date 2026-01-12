```yaml
number: 3344
title: Avoid stack overflows on more windows tests
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/avoid-stack-overflow-on-windows-tests
created_at: 2024-05-03T12:11:18Z
updated_at: 2024-05-03T12:21:01Z
url: https://github.com/astral-sh/uv/pull/3344
synced_at: 2026-01-12T16:05:35Z
```

# Avoid stack overflows on more windows tests

---

_@konstin_

Fix windows CI by increasing the debug stack size on windows:
* https://github.com/astral-sh/uv/actions/runs/8938560618/job/24553000399?pr=3340
* https://github.com/astral-sh/uv/actions/runs/8937835055/job/24550949991
* https://github.com/astral-sh/uv/actions/runs/8937835055/job/24550949261
* https://github.com/astral-sh/uv/actions/runs/8937835055/job/24550810405


---

_Review requested from @BurntSushi by @konstin on 2024-05-03 12:11_

---

_@BurntSushi approved on 2024-05-03 12:11_

---

_Merged by @konstin on 2024-05-03 12:21_

---

_Closed by @konstin on 2024-05-03 12:21_

---

_Branch deleted on 2024-05-03 12:21_

---
