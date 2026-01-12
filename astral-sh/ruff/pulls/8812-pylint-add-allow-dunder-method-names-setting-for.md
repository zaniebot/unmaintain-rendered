```yaml
number: 8812
title: "[`pylint`] Add `allow-dunder-method-names` setting for `bad-dunder-method-name` (`PLW3201`)"
type: pull_request
state: merged
author: ThiefMaster
labels:
  - configuration
assignees: []
merged: true
base: main
head: custom-dunder-names
created_at: 2023-11-21T21:36:00Z
updated_at: 2023-11-21T23:51:53Z
url: https://github.com/astral-sh/ruff/pull/8812
synced_at: 2026-01-10T23:40:55Z
```

# [`pylint`] Add `allow-dunder-method-names` setting for `bad-dunder-method-name` (`PLW3201`)

---

_Pull request opened by @ThiefMaster on 2023-11-21 21:36_

closes #8732

I noticed that the reference to the setting in the rule docs doesn't work, but there seem to be something wrong with pylint settings in general in the docs - the "For related settings, see ...." is also missing there.

---

_Comment by @github-actions[bot] on 2023-11-21 21:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @zanieb on 2023-11-21 23:12_

The related settings section is generated on build e.g. https://github.com/astral-sh/ruff/blob/14e65afdc6067cae521ac094371780db067717e5/crates/ruff_dev/src/generate_docs.rs#L100-L125

---

_Comment by @ThiefMaster on 2023-11-21 23:26_

Huh, I thought I executed that script. anyway, that indeed fixed the problem of the broken linking. Still no idea why the link from the rules page to the settings page isn't generated, but that's unrelated to this PR anyway..

---

_@charliermarsh approved on 2023-11-21 23:38_

Thanks!

---

_Label `configuration` added by @charliermarsh on 2023-11-21 23:39_

---

_Renamed from "Add custom-dunder-method-names setting for PLW3201" to "[`pylint`] Add `allow-dunder-method-names` setting for `bad-dunder-method-name` (`PLW3201`)" by @charliermarsh on 2023-11-21 23:40_

---

_Merged by @charliermarsh on 2023-11-21 23:44_

---

_Closed by @charliermarsh on 2023-11-21 23:44_

---

_Branch deleted on 2023-11-21 23:44_

---
