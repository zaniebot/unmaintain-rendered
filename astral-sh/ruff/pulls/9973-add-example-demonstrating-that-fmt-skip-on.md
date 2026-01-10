```yaml
number: 9973
title: "Add example demonstrating that `fmt: skip` on expression level is not supported"
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
assignees: []
merged: true
base: main
head: document-fmt-skip-expression-level
created_at: 2024-02-13T11:51:23Z
updated_at: 2024-02-13T15:35:28Z
url: https://github.com/astral-sh/ruff/pull/9973
synced_at: 2026-01-10T22:57:09Z
```

# Add example demonstrating that `fmt: skip` on expression level is not supported

---

_Pull request opened by @MichaReiser on 2024-02-13 11:51_

## Summary

The fact that ruff doesn't support expression level suppression comments while black does has caused some confusion. 
We plan to add a lint rule warning against said suppression comments in #9899 but we should also improve our documentation to mention the limitation explicilty. 

This PR adds such documentation to the suppression comments section. 

Closes https://github.com/astral-sh/ruff/issues/8319 

## Test Plan

![image](https://github.com/astral-sh/ruff/assets/1203881/5841eb75-412f-4a4d-bb53-1ae93020ad2d)

---

_Label `documentation` added by @MichaReiser on 2024-02-13 11:59_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-02-13 11:59_

---

_@charliermarsh reviewed on 2024-02-13 14:30_

---

_Review comment by @charliermarsh on `docs/formatter.md`:313 on 2024-02-13 14:30_

Should this one be removed?

---

_@charliermarsh approved on 2024-02-13 14:30_

---

_@MichaReiser reviewed on 2024-02-13 15:26_

---

_Review comment by @MichaReiser on `docs/formatter.md`:313 on 2024-02-13 15:26_

:roll_eyes: 

---

_Merged by @MichaReiser on 2024-02-13 15:35_

---

_Closed by @MichaReiser on 2024-02-13 15:35_

---

_Branch deleted on 2024-02-13 15:35_

---
