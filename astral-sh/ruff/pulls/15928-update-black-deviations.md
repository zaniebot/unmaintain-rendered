```yaml
number: 15928
title: Update black deviations
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
assignees: []
merged: true
base: main
head: micha/update-black-deviations-2025
created_at: 2025-02-04T11:30:21Z
updated_at: 2025-02-04T14:04:25Z
url: https://github.com/astral-sh/ruff/pull/15928
synced_at: 2026-01-10T19:57:22Z
```

# Update black deviations

---

_Pull request opened by @MichaReiser on 2025-02-04 11:30_

## Summary

Update the black deviations document to reflect the deviations between Black 2025 (before 2024) and Ruff 2025.

Closes https://github.com/astral-sh/ruff/issues/15737


---

_Label `documentation` added by @MichaReiser on 2025-02-04 11:30_

---

_Review comment by @dhruvmanila on `docs/formatter/black.md`:743 on 2025-02-04 13:08_

I think this should include the parentheses for the Ruff section:
```py
items = [(True)]
items = [(True)]
items = {(123)}
```

---

_@dhruvmanila reviewed on 2025-02-04 13:08_

---

_@dhruvmanila approved on 2025-02-04 13:08_

---

_@MichaReiser reviewed on 2025-02-04 13:59_

---

_Review comment by @MichaReiser on `docs/formatter/black.md`:743 on 2025-02-04 13:59_

Oh yeah. Thanks for catching this

---

_Merged by @MichaReiser on 2025-02-04 14:04_

---

_Closed by @MichaReiser on 2025-02-04 14:04_

---

_Branch deleted on 2025-02-04 14:04_

---
