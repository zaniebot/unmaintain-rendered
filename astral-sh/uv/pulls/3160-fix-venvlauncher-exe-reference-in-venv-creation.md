```yaml
number: 3160
title: "Fix `venvlauncher.exe` reference in venv creation"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/3.13
created_at: 2024-04-20T14:01:16Z
updated_at: 2024-04-20T14:16:48Z
url: https://github.com/astral-sh/uv/pull/3160
synced_at: 2026-01-10T14:43:31Z
```

# Fix `venvlauncher.exe` reference in venv creation

---

_Pull request opened by @charliermarsh on 2024-04-20 14:01_

I can't get this to reproduce on GitHub Actions -- maybe the builds there differ, or maybe the builds changed since we added this fix? I'll check locally, but regardless, this is a typo.

Closes #3158.

---

_Comment by @charliermarsh on 2024-04-20 14:04_

Hoping (?) that the 3.13 test fails.

---

_Renamed from "Test launcher script behavior" to "Fix `venvlauncher.exe` reference in venv creation" by @charliermarsh on 2024-04-20 14:11_

---

_Label `bug` added by @charliermarsh on 2024-04-20 14:11_

---

_Label `windows` added by @charliermarsh on 2024-04-20 14:11_

---

_Marked ready for review by @charliermarsh on 2024-04-20 14:11_

---

_Merged by @charliermarsh on 2024-04-20 14:16_

---

_Closed by @charliermarsh on 2024-04-20 14:16_

---

_Branch deleted on 2024-04-20 14:16_

---
