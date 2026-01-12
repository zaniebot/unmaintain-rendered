```yaml
number: 9041
title: "Avoid invalid combination of `force-sort-within-types` and `lines-between-types`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - isort
assignees: []
merged: true
base: main
head: charlie/force-sort
created_at: 2023-12-07T03:57:05Z
updated_at: 2023-12-07T04:56:15Z
url: https://github.com/astral-sh/ruff/pull/9041
synced_at: 2026-01-10T23:40:55Z
```

# Avoid invalid combination of `force-sort-within-types` and `lines-between-types`

---

_Pull request opened by @charliermarsh on 2023-12-07 03:57_

Closes https://github.com/astral-sh/ruff/issues/8792.

---

_Label `bug` added by @charliermarsh on 2023-12-07 03:57_

---

_Label `isort` added by @charliermarsh on 2023-12-07 03:57_

---

_Marked ready for review by @charliermarsh on 2023-12-07 03:57_

---

_Comment by @github-actions[bot] on 2023-12-07 04:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2023-12-07 04:21_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:2113 on 2023-12-07 04:21_

Shouldn't we warn in the settings instead? Invalid configurations are otherwise still possible when using `extend`.

It may also be worth to aborting in that case to prevent that we change a large set of files because we ignored the setting.

---

_@charliermarsh reviewed on 2023-12-07 04:31_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/options.rs`:2113 on 2023-12-07 04:31_

We already ignore this in the underlying implementation, and warning here is consistent with the other validation in this method. If we want to warn on `Settings`, I think that would be a separate change.

---

_@MichaReiser reviewed on 2023-12-07 04:44_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:2113 on 2023-12-07 04:44_

Interesting, the formatter validations are done inside of settings to avoid these inconsistencies.

I think validating in settings only is sufficient. Only validating in options has the downside that we might ignore the option without a warning if one isort options comes from the extended configuration

---

_@MichaReiser reviewed on 2023-12-07 04:46_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:2113 on 2023-12-07 04:46_

Interesting, the formatter validations are done inside of settings to avoid these inconsistencies.

I think validating in settings only is sufficient. Only validating in options has the downside that we might ignore the option without a warning if one isort options comes from the extended configuration

---

_Merged by @charliermarsh on 2023-12-07 04:56_

---

_Closed by @charliermarsh on 2023-12-07 04:56_

---

_Branch deleted on 2023-12-07 04:56_

---
