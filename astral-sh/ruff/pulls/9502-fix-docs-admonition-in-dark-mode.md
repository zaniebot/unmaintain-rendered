```yaml
number: 9502
title: "fix(docs): admonition in dark mode"
type: pull_request
state: merged
author: pabepadu
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix/admonition-dark-mode
created_at: 2024-01-13T03:57:41Z
updated_at: 2024-01-13T12:43:13Z
url: https://github.com/astral-sh/ruff/pull/9502
synced_at: 2026-01-12T15:55:29Z
```

# fix(docs): admonition in dark mode

---

_@pabepadu_

## Summary

The admonition in dark mode are in actually same as in light mode here: https://docs.astral.sh/ruff/formatter/#ruff-format

I propose to move admonition in real dark mode as shown below:
- Before the PR https://github.com/astral-sh/ruff/pull/9385
![image](https://github.com/astral-sh/ruff/assets/45884742/abb7e0db-bf12-49cd-9f9b-e353accd571f)
- Current version
![image](https://github.com/astral-sh/ruff/assets/45884742/da8ab46d-dc87-412e-be2b-ac937cd87666)
- Proposal
![image](https://github.com/astral-sh/ruff/assets/45884742/7279cb02-e6af-4320-9dee-486fbfddcbc8)

Close #9501

## Test Plan

Documentation was regenerated via mkdocs and the supplied requirements.


---

_Renamed from "fix: admonition in dark mode" to "fix(docs): admonition in dark mode" by @pabepadu on 2024-01-13 04:29_

---

_@zanieb approved on 2024-01-13 04:39_

Thanks! 

cc @charliermarsh I think there were some other instances of this problem in an issue but I don't remember where.

---

_Label `documentation` added by @zanieb on 2024-01-13 04:39_

---

_@charliermarsh approved on 2024-01-13 12:43_

Looks nice, thanks!

---

_Merged by @charliermarsh on 2024-01-13 12:43_

---

_Closed by @charliermarsh on 2024-01-13 12:43_

---
