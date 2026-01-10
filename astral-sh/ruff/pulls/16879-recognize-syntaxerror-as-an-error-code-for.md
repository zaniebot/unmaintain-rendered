```yaml
number: 16879
title: "Recognize `SyntaxError:` as an error code for ecosystem checks"
type: pull_request
state: merged
author: ntBre
labels:
  - ci
assignees: []
merged: true
base: main
head: brent/syntax-errors-ecosystem
created_at: 2025-03-20T19:56:19Z
updated_at: 2025-03-20T21:25:43Z
url: https://github.com/astral-sh/ruff/pull/16879
synced_at: 2026-01-10T19:40:36Z
```

# Recognize `SyntaxError:` as an error code for ecosystem checks

---

_Pull request opened by @ntBre on 2025-03-20 19:56_

Summary
--

This updates the regex in `ruff-ecosystem` to catch syntax errors in an effort to prevent bugs like #16874. This should catch `ParseError`s, `UnsupportedSyntaxError`s, and the upcoming `SemanticSyntaxError`s.

Test Plan
--

I ran the ecosystem check locally comparing v0.11.0 and v0.11.1 and saw a large number (2757!) of new syntax errors. I also manually tested the regex on a few lines before that.

If we merge this before #16878, I'd expect to see that number decrease substantially in that PR too, as another test.

---

_Label `ci` added by @ntBre on 2025-03-20 19:56_

---

_Comment by @github-actions[bot] on 2025-03-20 20:05_

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

_Comment by @MichaReiser on 2025-03-20 21:22_

Oh wow. I wasn't aware that they get filtered out. Nice find 

---

_@MichaReiser approved on 2025-03-20 21:22_

---

_Merged by @ntBre on 2025-03-20 21:25_

---

_Closed by @ntBre on 2025-03-20 21:25_

---

_Branch deleted on 2025-03-20 21:25_

---
