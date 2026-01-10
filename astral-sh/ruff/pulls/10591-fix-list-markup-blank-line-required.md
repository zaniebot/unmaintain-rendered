```yaml
number: 10591
title: "Fix list markup: blank line required"
type: pull_request
state: merged
author: aureliojargas
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-03-25T23:43:05Z
updated_at: 2024-03-26T05:50:00Z
url: https://github.com/astral-sh/ruff/pull/10591
synced_at: 2026-01-10T22:47:02Z
```

# Fix list markup: blank line required

---

_Pull request opened by @aureliojargas on 2024-03-25 23:43_

Judging from the other lists in this same file, it looks like a blank line is required to separate the previous paragraph from the first list item.

Otherwise the list is considered part of the paragraph:

- https://docs.astral.sh/ruff/settings/#lint_flake8-copyright_notice-rgx

<img width="400" alt="image" src="https://github.com/astral-sh/ruff/assets/282592/663fa5b4-f787-4fcd-bdd6-0b891d7ad72b">

- https://docs.astral.sh/ruff/settings/#format_quote-style

<img width="400" alt="image" src="https://github.com/astral-sh/ruff/assets/282592/809735eb-c546-4348-9311-877441bae84e">

After this change hopefully those will be rendered as proper bullet lists.

---

_@charliermarsh approved on 2024-03-25 23:43_

Thanks.

---

_Label `documentation` added by @charliermarsh on 2024-03-25 23:43_

---

_Merged by @charliermarsh on 2024-03-26 00:18_

---

_Closed by @charliermarsh on 2024-03-26 00:18_

---

_Comment by @github-actions[bot] on 2024-03-26 00:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Branch deleted on 2024-03-26 05:50_

---
