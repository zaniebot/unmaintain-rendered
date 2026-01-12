```yaml
number: 16308
title: "[WIP] Detect several simple syntax errors in the parser"
type: pull_request
state: closed
author: ntBre
labels: []
assignees: []
draft: true
base: brent/syntax-errors-parser
head: brent/simple-syntax-errors
created_at: 2025-02-21T18:37:57Z
updated_at: 2025-05-01T15:31:47Z
url: https://github.com/astral-sh/ruff/pull/16308
synced_at: 2026-01-12T15:55:54Z
```

# [WIP] Detect several simple syntax errors in the parser

---

_@ntBre_

## Summary

Currently stacked on #16090, this PR uses the framework there to detect several more simple version-related syntax errors in the parser:
* Assignment expressions before 3.8
* `except*` before 3.11
* `type` statements before 3.12
* type parameter lists before 3.12
* type parameter defaults before 3.13

## Test Plan

New CLI tests


---

_Comment by @github-actions[bot] on 2025-02-21 20:00_

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

_Comment by @MichaReiser on 2025-02-22 08:05_

This might have been your plan all along but I think I'd prefer a PR for each specific check vs one large PR that implements all of them. It should make them easier to review and I don't think it's necessary to ship them all at once

---

_Comment by @ntBre on 2025-02-22 18:37_

Sounds good, I'll close this and split it up once https://github.com/astral-sh/ruff/pull/16257 and https://github.com/astral-sh/ruff/pull/16090/ are merged since it's waiting on those anyway.

---

_Comment by @ntBre on 2025-02-24 21:34_

I'll go ahead and close this. I absorbed some of the changes into #16090, and I'll add each of the new errors in its own PR.

---

_Closed by @ntBre on 2025-02-24 21:34_

---

_Branch deleted on 2025-05-01 15:31_

---
