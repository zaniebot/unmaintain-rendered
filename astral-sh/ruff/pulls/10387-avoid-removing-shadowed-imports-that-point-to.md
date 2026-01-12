```yaml
number: 10387
title: Avoid removing shadowed imports that point to different symbols
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/f
created_at: 2024-03-13T15:36:03Z
updated_at: 2024-03-13T17:37:55Z
url: https://github.com/astral-sh/ruff/pull/10387
synced_at: 2026-01-12T15:55:32Z
```

# Avoid removing shadowed imports that point to different symbols

---

_@charliermarsh_

This ensures that we don't have incorrect, automated fixes for shadowed names that actually point to different imports.

See: https://github.com/astral-sh/ruff/issues/10384.


---

_Label `bug` added by @charliermarsh on 2024-03-13 15:36_

---

_Merged by @charliermarsh on 2024-03-13 15:44_

---

_Closed by @charliermarsh on 2024-03-13 15:44_

---

_Branch deleted on 2024-03-13 15:44_

---

_Comment by @github-actions[bot] on 2024-03-13 15:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @zanieb on 2024-03-13 17:34_

@charliermarsh you merge machine — why not https://github.com/astral-sh/ruff/pull/10388? doesn't it make more sense to remove the first import?

---

_Comment by @charliermarsh on 2024-03-13 17:36_

I explained a bit in the PR summary why that isn't really ideal. These are typically going to be "the same" import anyway so this felt ok to me.

---

_Comment by @zanieb on 2024-03-13 17:37_

Hm okay, can revisit later if we need to. Thanks!

---
