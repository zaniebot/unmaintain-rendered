```yaml
number: 9321
title: Add row and column numbers to formatted parse errors
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: charlie/display-parse-error
created_at: 2023-12-30T19:50:54Z
updated_at: 2023-12-31T12:10:47Z
url: https://github.com/astral-sh/ruff/pull/9321
synced_at: 2026-01-10T23:07:18Z
```

# Add row and column numbers to formatted parse errors

---

_Pull request opened by @charliermarsh on 2023-12-30 19:50_

## Summary

We now render parse errors in the formatter identically to those in the linter, e.g.:

```
❯ cargo run -p ruff_cli -- format foo.py
error: Failed to parse foo.py:1:17: Unexpected token '='
```

Closes https://github.com/astral-sh/ruff/issues/8338.

Closes https://github.com/astral-sh/ruff/issues/9311.


---

_Review comment by @charliermarsh on `crates/ruff_cli/tests/format.rs`:351 on 2023-12-30 19:52_

Need to fix.

---

_@charliermarsh reviewed on 2023-12-30 19:52_

---

_Comment by @github-actions[bot] on 2023-12-30 20:06_

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

_Review requested from @zanieb by @charliermarsh on 2023-12-30 21:18_

---

_Label `bug` added by @charliermarsh on 2023-12-30 21:18_

---

_Label `cli` added by @charliermarsh on 2023-12-30 21:18_

---

_Marked ready for review by @charliermarsh on 2023-12-30 21:20_

---

_@konstin approved on 2023-12-31 11:57_

---

_Merged by @charliermarsh on 2023-12-31 12:10_

---

_Closed by @charliermarsh on 2023-12-31 12:10_

---

_Branch deleted on 2023-12-31 12:10_

---
