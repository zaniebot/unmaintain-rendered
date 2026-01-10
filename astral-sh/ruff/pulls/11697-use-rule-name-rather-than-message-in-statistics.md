```yaml
number: 11697
title: "Use rule name rather than message in `--statistics`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - breaking
  - cli
assignees: []
merged: true
base: ruff-0.5
head: charlie/name
created_at: 2024-06-02T18:19:11Z
updated_at: 2024-06-24T12:23:26Z
url: https://github.com/astral-sh/ruff/pull/11697
synced_at: 2026-01-10T21:56:00Z
```

# Use rule name rather than message in `--statistics`

---

_Pull request opened by @charliermarsh on 2024-06-02 18:19_

## Summary

Since this changes the output schema of the JSON format for `--statistics`, let's include it in v0.5.0.

Closes https://github.com/astral-sh/ruff/issues/11097.


---

_Label `breaking` added by @charliermarsh on 2024-06-02 18:19_

---

_Label `cli` added by @charliermarsh on 2024-06-02 18:19_

---

_Added to milestone `v0.5.0` by @charliermarsh on 2024-06-02 18:28_

---

_Review requested from @zanieb by @zanieb on 2024-06-02 18:29_

---

_Comment by @github-actions[bot] on 2024-06-02 18:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2024-06-03 11:34_

---

_Review comment by @dhruvmanila on `crates/ruff/tests/integration_test.rs`:858 on 2024-06-03 11:34_

nit: can we add a test case for the JSON output while we're here?

---

_@MichaReiser approved on 2024-06-24 12:04_

---

_Merged by @MichaReiser on 2024-06-24 12:23_

---

_Closed by @MichaReiser on 2024-06-24 12:23_

---

_Branch deleted on 2024-06-24 12:23_

---
