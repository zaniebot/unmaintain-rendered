```yaml
number: 979
title: Improve simple no version messages using complement of range
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/no-versions
created_at: 2024-01-18T23:36:36Z
updated_at: 2024-01-19T16:48:21Z
url: https://github.com/astral-sh/uv/pull/979
synced_at: 2026-01-12T16:04:20Z
```

# Improve simple no version messages using complement of range

---

_@zanieb_

Improves some of the "no versions of <package> are available" messages by showing the complement or inversion of the package.

Does not address cases like

```
Because there are no versions of crow that satisfy any of:
    crow>1.0.0,<2.0.0a5
    crow>2.0.0a7,<2.0.0b1
    crow>2.0.0b1,<2.0.0b5
...
```

which are a bit more complicated; I'll focus on those cases in a follow-up.

---

_Renamed from "zb/no versions" to "Improve simple no version messages using complement of range" by @zanieb on 2024-01-18 23:37_

---

_Label `error messages` added by @zanieb on 2024-01-19 00:00_

---

_@charliermarsh approved on 2024-01-19 03:42_

Yeah I think this is generally clearer.

---

_Merged by @zanieb on 2024-01-19 16:48_

---

_Closed by @zanieb on 2024-01-19 16:48_

---

_Branch deleted on 2024-01-19 16:48_

---
