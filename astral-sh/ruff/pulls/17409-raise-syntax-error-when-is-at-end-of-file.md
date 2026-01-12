```yaml
number: 17409
title: "Raise syntax error when `\\` is at end of file"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: dhruv/parser-line-continuation-at-eof
created_at: 2025-04-15T14:55:24Z
updated_at: 2025-04-15T15:56:14Z
url: https://github.com/astral-sh/ruff/pull/17409
synced_at: 2026-01-12T15:56:02Z
```

# Raise syntax error when `\` is at end of file

---

_@dhruvmanila_

## Summary

This PR fixes a bug in the lexer specifically around line continuation character at end of file.

The reason this was occurring is because the lexer wouldn't check for EOL _after_ consuming the escaped newline but only if the EOL was right after the line continuation character.

fixes: #17398 

## Test Plan

Add tests for the scenarios where this should occur mainly (a) when the state is `AfterNewline` and (b) when the state is `Other`.




---

_Label `bug` added by @dhruvmanila on 2025-04-15 14:55_

---

_Label `parser` added by @dhruvmanila on 2025-04-15 14:55_

---

_Comment by @github-actions[bot] on 2025-04-15 15:05_

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

_Marked ready for review by @dhruvmanila on 2025-04-15 15:23_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-04-15 15:23_

---

_Review requested from @ntBre by @dhruvmanila on 2025-04-15 15:23_

---

_@ntBre approved on 2025-04-15 15:29_

Nice! Thanks for doing this.

---

_Merged by @dhruvmanila on 2025-04-15 15:56_

---

_Closed by @dhruvmanila on 2025-04-15 15:56_

---

_Branch deleted on 2025-04-15 15:56_

---
