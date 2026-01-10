```yaml
number: 868
title: Upgrade autofix to fix-until-stable
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2022-11-22T02:22:20Z
updated_at: 2022-11-22T21:37:53Z
url: https://github.com/astral-sh/ruff/issues/868
synced_at: 2026-01-10T12:09:58Z
```

# Upgrade autofix to fix-until-stable

---

_Issue opened by @charliermarsh on 2022-11-22 02:22_

See: #660. The idea is that, if we apply an autofix, we should re-lint the fixed file, apply any fixes, and continue until the source code has stabilized (up to some reasonable limit). We may want to make this opt-in to start.


---

_Label `enhancement` added by @charliermarsh on 2022-11-22 02:22_

---

_Closed by @charliermarsh on 2022-11-22 21:37_

---
