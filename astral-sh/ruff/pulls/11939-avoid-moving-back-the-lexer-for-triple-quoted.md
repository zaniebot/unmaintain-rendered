```yaml
number: 11939
title: Avoid moving back the lexer for triple-quoted fstring
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: dhruv/re-lexing-fstring-bug
created_at: 2024-06-19T12:20:17Z
updated_at: 2024-06-20T10:57:37Z
url: https://github.com/astral-sh/ruff/pull/11939
synced_at: 2026-01-12T15:55:39Z
```

# Avoid moving back the lexer for triple-quoted fstring

---

_@dhruvmanila_

## Summary

This PR avoids moving back the lexer for a triple-quoted f-string during the re-lexing phase.

The reason this is a problem is that for a triple-quoted f-string the newlines are part of the f-string itself, specifically they'll be part of the `FStringMiddle` token. So, if we moved the lexer back, there would be a `Newline` token whose range would be in between an `FStringMiddle` token. This creates a panic in downstream usage.

fixes: #11937 

## Test Plan

Add test cases and validate the snapshots.

---

_Label `bug` added by @dhruvmanila on 2024-06-19 12:20_

---

_Label `parser` added by @dhruvmanila on 2024-06-19 12:20_

---

_Comment by @github-actions[bot] on 2024-06-19 12:39_

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

_Marked ready for review by @dhruvmanila on 2024-06-20 09:00_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-20 09:00_

---

_@MichaReiser approved on 2024-06-20 10:44_

---

_Merged by @dhruvmanila on 2024-06-20 10:57_

---

_Closed by @dhruvmanila on 2024-06-20 10:57_

---

_Branch deleted on 2024-06-20 10:57_

---
