```yaml
number: 10190
title: "Removed unused variable in `TRY300`'s example"
type: pull_request
state: merged
author: trag1c
labels:
  - documentation
assignees: []
merged: true
base: main
head: correct-try300
created_at: 2024-03-01T21:06:30Z
updated_at: 2024-03-01T23:50:56Z
url: https://github.com/astral-sh/ruff/pull/10190
synced_at: 2026-01-12T15:55:31Z
```

# Removed unused variable in `TRY300`'s example

---

_@trag1c_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Removes the unnecessary `exc` variable in `TRY300`'s docs example.

## Test Plan
```
python scripts/generate_mkdocs.py; mkdocs serve -f mkdocs.public.yml
```

---

_Comment by @github-actions[bot] on 2024-03-01 21:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-03-01 23:50_

---

_Label `documentation` added by @charliermarsh on 2024-03-01 23:50_

---

_Merged by @charliermarsh on 2024-03-01 23:50_

---

_Closed by @charliermarsh on 2024-03-01 23:50_

---

_Comment by @charliermarsh on 2024-03-01 23:50_

Thanks!

---
