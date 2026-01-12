```yaml
number: 8393
title: Respect --force-exclude for lint.exclude and format.exclude
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: charlie/force-exclude
created_at: 2023-10-31T21:08:21Z
updated_at: 2023-10-31T21:45:55Z
url: https://github.com/astral-sh/ruff/pull/8393
synced_at: 2026-01-12T15:55:26Z
```

# Respect --force-exclude for lint.exclude and format.exclude

---

_@charliermarsh_

## Summary

We typically avoid enforcing exclusions if a file was passed to Ruff directly on the CLI. However, we also allow `--force-exclude`, which ignores excluded files _even_ if they're passed to Ruff directly. This is really important for pre-commit, which always passes changed files -- we need to exclude files passed by pre-commit if they're in the `exclude` lists.

Turns out the new `lint.exclude` and `format.exclude` settings weren't respecting `--force-exclude`.

Closes https://github.com/astral-sh/ruff/issues/8391.

---

_Label `bug` added by @charliermarsh on 2023-10-31 21:08_

---

_Label `cli` added by @charliermarsh on 2023-10-31 21:08_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-10-31 21:14_

---

_Review requested from @zanieb by @charliermarsh on 2023-10-31 21:14_

---

_Comment by @github-actions[bot] on 2023-10-31 21:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2023-10-31 21:45_

Thanks

---

_Merged by @charliermarsh on 2023-10-31 21:45_

---

_Closed by @charliermarsh on 2023-10-31 21:45_

---

_Branch deleted on 2023-10-31 21:45_

---

_Comment by @charliermarsh on 2023-10-31 21:45_

No prob

---
