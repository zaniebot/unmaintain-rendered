```yaml
number: 4478
title: "Break `PubGrubReportFormatter::hints` into methods"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/simplify-PubGrubReportFormatter-hints
created_at: 2024-06-24T16:57:27Z
updated_at: 2024-06-24T17:14:10Z
url: https://github.com/astral-sh/uv/pull/4478
synced_at: 2026-01-10T13:48:28Z
```

# Break `PubGrubReportFormatter::hints` into methods

---

_Pull request opened by @konstin on 2024-06-24 16:57_

I have to add yet another indentation level to the prerelease-available check in `PubGrubReportFormatter::hints` for #4435, so i've broken the code into methods and decreased indentation in this split out refactoring-only change.

---

_Label `internal` added by @konstin on 2024-06-24 16:57_

---

_@charliermarsh approved on 2024-06-24 17:00_

---

_Merged by @konstin on 2024-06-24 17:14_

---

_Closed by @konstin on 2024-06-24 17:14_

---

_Branch deleted on 2024-06-24 17:14_

---
