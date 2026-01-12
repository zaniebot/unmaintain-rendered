```yaml
number: 718
title: Implement B012
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: B012
created_at: 2022-11-13T14:11:01Z
updated_at: 2022-11-13T16:55:34Z
url: https://github.com/astral-sh/ruff/pull/718
synced_at: 2026-01-12T15:55:05Z
```

# Implement B012

---

_@harupy_

#389 

---

_@charliermarsh reviewed on 2022-11-13 15:59_

---

_Review comment by @charliermarsh on `src/checks.rs`:1415 on 2022-11-13 15:59_

Can we include which of these cases was triggered directly in the rule? So that it's like `return inside finally blocks...` or as relevant?

---

_@harupy reviewed on 2022-11-13 16:11_

---

_Review comment by @harupy on `src/checks.rs`:1415 on 2022-11-13 16:11_

Got it, will fix

---

_Merged by @charliermarsh on 2022-11-13 16:55_

---

_Closed by @charliermarsh on 2022-11-13 16:55_

---
