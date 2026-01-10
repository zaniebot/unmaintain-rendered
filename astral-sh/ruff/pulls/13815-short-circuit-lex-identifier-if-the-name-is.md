```yaml
number: 13815
title: "Short circuit `lex_identifier` if the name is longer or shorter than any known keyword"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - parser
assignees: []
merged: true
base: main
head: micha/lexer-perf
created_at: 2024-10-19T10:06:20Z
updated_at: 2024-10-19T11:21:47Z
url: https://github.com/astral-sh/ruff/pull/13815
synced_at: 2026-01-10T20:59:37Z
```

# Short circuit `lex_identifier` if the name is longer or shorter than any known keyword

---

_Pull request opened by @MichaReiser on 2024-10-19 10:06_


## Summary

This PR address the regression introduced by https://github.com/astral-sh/ruff/pull/13808 when upgrading Ruff to Rust 1.82. 

I tried to debug the difference in compilerexplorer but it doesn't support Rust 1.82 yet.

## Test Plan

`cargo bench`


---

_Review requested from @dhruvmanila by @MichaReiser on 2024-10-19 10:06_

---

_Label `internal` added by @MichaReiser on 2024-10-19 10:10_

---

_Label `parser` added by @MichaReiser on 2024-10-19 10:10_

---

_Comment by @github-actions[bot] on 2024-10-19 10:25_

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

_Merged by @MichaReiser on 2024-10-19 11:07_

---

_Closed by @MichaReiser on 2024-10-19 11:07_

---

_Branch deleted on 2024-10-19 11:07_

---
