```yaml
number: 12033
title: Match import name ignores against both name and alias
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ignore
created_at: 2024-06-25T21:18:43Z
updated_at: 2024-06-25T22:47:20Z
url: https://github.com/astral-sh/ruff/pull/12033
synced_at: 2026-01-10T21:56:00Z
```

# Match import name ignores against both name and alias

---

_Pull request opened by @charliermarsh on 2024-06-25 21:18_

## Summary

Right now, it's inconsistent... We sometimes match against the name, and sometimes against the alias (`asname`). I could see a case for always matching against the name, but matching against both seems fine too, since the rule is really about the combination of the two?

Closes https://github.com/astral-sh/ruff/issues/12031.


---

_Label `bug` added by @charliermarsh on 2024-06-25 21:18_

---

_Comment by @github-actions[bot] on 2024-06-25 21:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-06-25 22:47_

---

_Closed by @charliermarsh on 2024-06-25 22:47_

---

_Branch deleted on 2024-06-25 22:47_

---
