```yaml
number: 15080
title: "Make `symbol_by_id` a salsa query"
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
draft: true
base: main
head: micha/symbol_by_id_query
created_at: 2024-12-20T10:42:59Z
updated_at: 2024-12-21T10:33:49Z
url: https://github.com/astral-sh/ruff/pull/15080
synced_at: 2026-01-10T20:42:27Z
```

# Make `symbol_by_id` a salsa query

---

_Pull request opened by @MichaReiser on 2024-12-20 10:42_

## Summary

I'm just curious what this does to performance.

## Test Plan



---

_Label `red-knot` added by @MichaReiser on 2024-12-20 10:43_

---

_Comment by @github-actions[bot] on 2024-12-20 10:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-12-20 10:53_

@sharkdp this might be an easy enough fix... Should I just land it?

---

_Comment by @AlexWaygood on 2024-12-20 12:43_

> I'm just curious what this does to performance.

Looks like a 2% speedup on the incremental benchmark https://codspeed.io/astral-sh/ruff/branches/micha%2Fsymbol_by_id_query

---

_Comment by @sharkdp on 2024-12-21 10:33_

Thank you @MichaReiser. I included this in #15019 

---

_Closed by @sharkdp on 2024-12-21 10:33_

---
