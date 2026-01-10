```yaml
number: 8857
title: "Add progress bar for `uv cache clean`"
type: pull_request
state: merged
author: j178
labels: []
assignees: []
merged: true
base: main
head: clean-progress
created_at: 2024-11-06T08:53:08Z
updated_at: 2024-11-07T11:10:10Z
url: https://github.com/astral-sh/uv/pull/8857
synced_at: 2026-01-10T12:00:00Z
```

# Add progress bar for `uv cache clean`

---

_Pull request opened by @j178 on 2024-11-06 08:53_

## Summary

Closes #8786



---

_Marked ready for review by @j178 on 2024-11-06 09:22_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-06 16:18_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_sync.rs`:1705 on 2024-11-06 16:42_

Why did we remove these?

---

_@charliermarsh reviewed on 2024-11-06 16:42_

---

_Merged by @charliermarsh on 2024-11-06 16:43_

---

_Closed by @charliermarsh on 2024-11-06 16:43_

---

_Branch deleted on 2024-11-07 10:25_

---

_@j178 reviewed on 2024-11-07 11:10_

---

_Review comment by @j178 on `crates/uv/tests/it/pip_sync.rs`:1705 on 2024-11-07 11:10_

After adding the package cleaning progress bar, to show summary for each package, we have to collect each summary into a HashMap and display them at the end. For simplicity (or perhaps laziness), I've decided to show only the total summary instead. :)

---
