```yaml
number: 7838
title: Write summary messages to stderr when fixing via stdin (instead of omitting them)
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/stderr-summary
created_at: 2023-10-06T16:21:46Z
updated_at: 2023-10-06T17:11:04Z
url: https://github.com/astral-sh/ruff/pull/7838
synced_at: 2026-01-12T02:32:41Z
```

# Write summary messages to stderr when fixing via stdin (instead of omitting them)

---

_Pull request opened by @zanieb on 2023-10-06 16:21_

Previously we just omitted diagnostic summaries when using `--fix` or `--diff` with a stdin file. Now, we still write the summaries to stderr instead of the main writer (which is generally stdout but could be changed by `--output-file`).

---

_@zanieb reviewed on 2023-10-06 16:28_

---

_Review comment by @zanieb on `crates/ruff_cli/src/lib.rs`:211 on 2023-10-06 16:28_

Should I move this down? Maybe. Should we write other things to stderr? Maybe.

---

_Comment by @zanieb on 2023-10-06 16:29_

Useful for testing in https://github.com/astral-sh/ruff/pull/7790 and arguably a better UX.

---

_Comment by @github-actions[bot] on 2023-10-06 16:38_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@charliermarsh approved on 2023-10-06 17:05_

---

_Merged by @zanieb on 2023-10-06 17:11_

---

_Closed by @zanieb on 2023-10-06 17:11_

---

_Branch deleted on 2023-10-06 17:11_

---
