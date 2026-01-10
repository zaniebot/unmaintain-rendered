```yaml
number: 9422
title: "Avoid extra `String` allocation during parsing"
type: pull_request
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
draft: true
base: main
head: charlie/string
created_at: 2024-01-07T15:35:57Z
updated_at: 2024-01-07T17:36:53Z
url: https://github.com/astral-sh/ruff/pull/9422
synced_at: 2026-01-10T22:57:09Z
```

# Avoid extra `String` allocation during parsing

---

_Pull request opened by @charliermarsh on 2024-01-07 15:35_

## Summary

We don't need to allocate the string contents in the lexer, since we only use the owned `&str` in the parser, and it's a direct range from the source code. I'd expect this to improve both lexing and parsing.

---

_Label `performance` added by @charliermarsh on 2024-01-07 15:36_

---

_Comment by @github-actions[bot] on 2024-01-07 15:53_

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

_Comment by @charliermarsh on 2024-01-07 17:36_

Closing for now, I am going to revisit all of this once the parser has merged. I think we can reduce the size of `Token` and remove a lot of allocations here.

---

_Closed by @charliermarsh on 2024-01-07 17:36_

---
