```yaml
number: 784
title: Implement auto-fix for E711 and E712
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: fix-E711-E712
created_at: 2022-11-17T11:47:40Z
updated_at: 2022-11-18T04:15:15Z
url: https://github.com/astral-sh/ruff/pull/784
synced_at: 2026-01-12T05:48:45Z
```

# Implement auto-fix for E711 and E712

---

_Pull request opened by @harupy on 2022-11-17 11:47_

#722

---

_@charliermarsh reviewed on 2022-11-17 16:43_

---

_Review comment by @charliermarsh on `src/pycodestyle/plugins.rs`:43 on 2022-11-17 16:43_

@harupy - The one issue with this approach is that if we start to show users fix suggestions in the `ruff` output (similar to Clippy), we'll have to be careful in how we handle this case.


---

_Merged by @charliermarsh on 2022-11-17 16:43_

---

_Closed by @charliermarsh on 2022-11-17 16:43_

---

_Branch deleted on 2022-11-18 04:15_

---
