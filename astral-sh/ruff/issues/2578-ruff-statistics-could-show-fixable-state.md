```yaml
number: 2578
title: ruff --statistics could show fixable state
type: issue
state: closed
author: spaceone
labels:
  - cli
assignees: []
created_at: 2023-02-05T10:36:42Z
updated_at: 2023-02-10T02:00:41Z
url: https://github.com/astral-sh/ruff/issues/2578
synced_at: 2026-01-12T15:54:43Z
```

# ruff --statistics could show fixable state

---

_@spaceone_

ruff now shows fixable items:
```PIE790 [*] Unnecessary `pass` statement```

ruff --statistics just shows:
```1\tPIE790\tUnnecessary `pass` statement```

→ the information would be nice as well:

maybe like this?:

for fixable items:
```1\tPIE790\t[*]\tUnnecessary `pass` statement```
for unfixable items:
```1\tPIE790\t[]\tUnnecessary `pass` statement```

---

_Label `cli` added by @charliermarsh on 2023-02-05 23:54_

---

_Closed by @charliermarsh on 2023-02-10 02:00_

---
