```yaml
number: 9329
title: "Remove `flake8-to-ruff`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/flake8-to-ruff
created_at: 2023-12-31T12:19:02Z
updated_at: 2024-01-05T09:41:55Z
url: https://github.com/astral-sh/ruff/pull/9329
synced_at: 2026-01-12T15:55:28Z
```

# Remove `flake8-to-ruff`

---

_@charliermarsh_

## Summary

We stopped releasing this a while ago and no longer advertise it anywhere. IMO, we should remove it so that we stop paying the cost of maintaining it. If we want to revive it, we can always do so from Git.


---

_Review requested from @zanieb by @charliermarsh on 2023-12-31 12:19_

---

_Marked ready for review by @charliermarsh on 2023-12-31 12:19_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-12-31 12:27_

---

_Review requested from @konstin by @charliermarsh on 2023-12-31 12:27_

---

_Label `internal` added by @charliermarsh on 2023-12-31 12:27_

---

_Comment by @github-actions[bot] on 2023-12-31 12:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@zanieb approved on 2023-12-31 15:57_

---

_Merged by @charliermarsh on 2023-12-31 16:09_

---

_Closed by @charliermarsh on 2023-12-31 16:09_

---

_Branch deleted on 2023-12-31 16:09_

---

_Comment by @akx on 2024-01-05 09:41_

Ah, it might be a good idea to push one more (binary-identical?) 0.0.233.1 release to PyPI with a prominent "This project has been archived, see https://github.com/astral-sh/ruff/pull/9329" warning in the readme...

---
