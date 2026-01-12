```yaml
number: 14165
title: "[`flake8-logging-format`] Fix invalid formatting value in docs of `logging-extra-attr-clash` (`G101`)"
type: pull_request
state: merged
author: sbrugman
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2024-11-07T15:47:13Z
updated_at: 2024-11-07T21:09:20Z
url: https://github.com/astral-sh/ruff/pull/14165
synced_at: 2026-01-12T15:55:47Z
```

# [`flake8-logging-format`] Fix invalid formatting value in docs of `logging-extra-attr-clash` (`G101`)

---

_@sbrugman_

Fixes ValueError in example

---

_Label `documentation` added by @MichaReiser on 2024-11-07 15:56_

---

_@MichaReiser reviewed on 2024-11-07 15:57_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_logging_format/violations.rs`:435 on 2024-11-07 15:57_

Should we instead adjust the `basicConfig` call above so that the two examples are consistent?

---

_Comment by @github-actions[bot] on 2024-11-07 16:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sbrugman reviewed on 2024-11-07 16:14_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_logging_format/violations.rs`:435 on 2024-11-07 16:14_

The other rules also use `user_id`, so I believe this is the consistent choice. The variable name `username` is on purpose different from the keyword to make the relation between variable and log formatting clear.

---

_Renamed from "[`flake8-logging-format`] Fix invalid syntax in docs of `logging-extra-attr-clash` (`G101`)" to "[`flake8-logging-format`] Fix invalid formatting value in docs of `logging-extra-attr-clash` (`G101`)" by @sbrugman on 2024-11-07 16:15_

---

_@MichaReiser approved on 2024-11-07 19:59_

---

_Merged by @MichaReiser on 2024-11-07 20:00_

---

_Closed by @MichaReiser on 2024-11-07 20:00_

---

_Branch deleted on 2024-11-07 21:09_

---
