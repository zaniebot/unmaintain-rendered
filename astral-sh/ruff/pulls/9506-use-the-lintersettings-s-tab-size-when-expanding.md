```yaml
number: 9506
title: "Use the `LinterSettings`'s tab size when expanding indent"
type: pull_request
state: merged
author: hoel-bagard
labels:
  - linter
assignees: []
merged: true
base: main
head: use_tab_size_setting
created_at: 2024-01-13T12:59:02Z
updated_at: 2024-01-14T02:06:54Z
url: https://github.com/astral-sh/ruff/pull/9506
synced_at: 2026-01-10T22:57:09Z
```

# Use the `LinterSettings`'s tab size when expanding indent

---

_Pull request opened by @hoel-bagard on 2024-01-13 12:59_

## Summary
In the `logical_lines`'s  `expand_indent` , respect the `LinterSettings::tab_size` setting instead of  hardcoding the size of tabs to 8.

Also see [this conversation](https://github.com/astral-sh/ruff/pull/9266#discussion_r1447102212)

## Test Plan

Tested by running `cargo test`

---

_Comment by @github-actions[bot] on 2024-01-13 13:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "Use the `LinterSettings`'s tab size." to "Use the `LinterSettings`'s tab size when expanding indent" by @hoel-bagard on 2024-01-13 13:49_

---

_@zanieb approved on 2024-01-13 17:24_

---

_Label `linter` added by @zanieb on 2024-01-13 17:24_

---

_@charliermarsh reviewed on 2024-01-13 23:30_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/logical_lines.rs`:17 on 2024-01-13 23:30_

Nit: mind updating this comment?

---

_@hoel-bagard reviewed on 2024-01-14 01:13_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/checkers/logical_lines.rs`:17 on 2024-01-14 01:13_

Done in [ff9743d](https://github.com/astral-sh/ruff/pull/9506/commits/ff9743dea971b18c8cc8ded75e9937154ff78d40)

---

_Merged by @charliermarsh on 2024-01-14 02:06_

---

_Closed by @charliermarsh on 2024-01-14 02:06_

---

_Comment by @charliermarsh on 2024-01-14 02:06_

Thanks!

---
