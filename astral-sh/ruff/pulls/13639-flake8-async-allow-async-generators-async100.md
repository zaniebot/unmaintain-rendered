```yaml
number: 13639
title: "[`flake8-async`] allow async generators (`ASYNC100`)"
type: pull_request
state: merged
author: autinerd
labels:
  - rule
assignees: []
merged: true
base: main
head: async100-allow-async-generators
created_at: 2024-10-05T09:02:04Z
updated_at: 2024-10-11T12:04:10Z
url: https://github.com/astral-sh/ruff/pull/13639
synced_at: 2026-01-10T20:59:36Z
```

# [`flake8-async`] allow async generators (`ASYNC100`)

---

_Pull request opened by @autinerd on 2024-10-05 09:02_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Treat async generators as "await" in ASYNC100.

Fixes #13637

## Test Plan

Updated snapshot


---

_Comment by @github-actions[bot] on 2024-10-05 09:20_

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

_@zanieb approved on 2024-10-05 15:19_

---

_Comment by @zanieb on 2024-10-05 15:22_

Actually, if I add this test case to `main` I don't see a violation (i.e., the snapshots do not change) but if I use your example from the issue they do. It seems like your new test case is not sufficient, can you add one that raises the false positive?

---

_@zanieb requested changes on 2024-10-05 15:22_

(needs described test change)

---

_Comment by @autinerd on 2024-10-05 19:25_

I have changed the test.

---

_Label `rule` added by @MichaReiser on 2024-10-07 09:20_

---

_Review requested from @zanieb by @MichaReiser on 2024-10-07 09:20_

---

_@MichaReiser approved on 2024-10-07 09:20_

---

_@zanieb approved on 2024-10-07 12:25_

---

_Merged by @zanieb on 2024-10-07 12:25_

---

_Closed by @zanieb on 2024-10-07 12:25_

---

_Branch deleted on 2024-10-11 12:04_

---
