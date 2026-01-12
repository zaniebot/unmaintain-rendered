```yaml
number: 13029
title: Show full error context in server messages
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: main
head: dhruv/error-context
created_at: 2024-08-21T09:54:13Z
updated_at: 2024-08-21T10:08:41Z
url: https://github.com/astral-sh/ruff/pull/13029
synced_at: 2026-01-12T15:55:42Z
```

# Show full error context in server messages

---

_@dhruvmanila_

## Summary

Reference: https://docs.rs/anyhow/latest/anyhow/struct.Error.html#display-representations

Closes: #13022 

## Test Plan

```
2024-08-21 15:21:24.831 [info] [Trace - 3:21:24 PM]    0.017255167s ERROR ThreadId(04) ruff_server::session::index::ruff_settings: Failed to parse /Users/dhruv/playground/ruff/pyproject.toml: TOML parse error at line 1, column 1
  |
1 | [tool.ruff.lint]
  | ^^^^^^^^^^^^^^^^
Unknown rule selector: `ME102`
```

Or,
```
2024-08-21 15:23:47.993 [info] [Trace - 3:23:47 PM]  143.179857375s ERROR ThreadId(66) ruff_server::session::index::ruff_settings: Failed to parse /Users/dhruv/playground/ruff/pyproject.toml: TOML parse error at line 2, column 42
  |
2 | select = ["ALL", "TD006", "TD007", "FIX"
  |                                          ^
invalid array
expected `]`
```


---

_Label `server` added by @dhruvmanila on 2024-08-21 09:54_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-21 09:55_

---

_@MichaReiser approved on 2024-08-21 09:58_

---

_Merged by @dhruvmanila on 2024-08-21 10:06_

---

_Closed by @dhruvmanila on 2024-08-21 10:06_

---

_Branch deleted on 2024-08-21 10:06_

---

_Comment by @github-actions[bot] on 2024-08-21 10:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
