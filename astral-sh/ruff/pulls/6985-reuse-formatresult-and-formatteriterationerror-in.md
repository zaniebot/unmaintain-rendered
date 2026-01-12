```yaml
number: 6985
title: "Reuse `FormatResult` and `FormatterIterationError` in `format_stdin.rs`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/format-errors
created_at: 2023-08-29T17:29:22Z
updated_at: 2023-08-29T17:48:08Z
url: https://github.com/astral-sh/ruff/pull/6985
synced_at: 2026-01-12T15:55:23Z
```

# Reuse `FormatResult` and `FormatterIterationError` in `format_stdin.rs`

---

_@charliermarsh_

## Summary

Ensures that we use the same error types and messages. Also renames those struct to `FormatCommand*` for consistency, and removes the `FormatCommandResult::Skipped` variant in favor of skipping in the iterator directly.

---

_Label `formatter` added by @charliermarsh on 2023-08-29 17:29_

---

_Merged by @charliermarsh on 2023-08-29 17:41_

---

_Closed by @charliermarsh on 2023-08-29 17:41_

---

_Branch deleted on 2023-08-29 17:41_

---

_Comment by @github-actions[bot] on 2023-08-29 17:48_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
