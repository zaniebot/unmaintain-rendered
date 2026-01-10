```yaml
number: 15422
title: "Remove `UpgradeSelection` struct"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/up
created_at: 2025-08-21T16:59:07Z
updated_at: 2025-08-21T18:35:08Z
url: https://github.com/astral-sh/uv/pull/15422
synced_at: 2026-01-10T06:44:33Z
```

# Remove `UpgradeSelection` struct

---

_Pull request opened by @charliermarsh on 2025-08-21 16:59_

## Summary

After #15395, I realized that we didn't actually need a separate struct for this since we now pass it around as an `Option`. (The key change from #15395 is that when combining, we treat the options as a single unit.)


---

_Label `internal` added by @charliermarsh on 2025-08-21 16:59_

---

_Marked ready for review by @charliermarsh on 2025-08-21 17:00_

---

_@konstin approved on 2025-08-21 17:19_

---

_Merged by @charliermarsh on 2025-08-21 18:35_

---

_Closed by @charliermarsh on 2025-08-21 18:35_

---

_Branch deleted on 2025-08-21 18:35_

---
