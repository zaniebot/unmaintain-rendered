```yaml
number: 11604
title: "[redknot] complete symbol table by populating Symbol::kind field"
type: pull_request
state: closed
author: plredmond
labels:
  - ty
assignees: []
draft: true
base: main
head: redknot.symbolkind
created_at: 2024-05-29T21:02:16Z
updated_at: 2025-02-20T09:00:27Z
url: https://github.com/astral-sh/ruff/pull/11604
synced_at: 2026-01-10T19:57:22Z
```

# [redknot] complete symbol table by populating Symbol::kind field

---

_Pull request opened by @plredmond on 2024-05-29 21:02_

followup to #11134 

* [x] Set `MARKED_NONLOCAL` and `MARKED_GLOBAL` flags during 1st pass (over AST)
* [ ] Add back the `Symbol::kind : Kind` field (or `Symbol::kind : Option<Kind>`?) with an appropriate default
* [ ] During 2nd pass (over symbol table hierarchy) set `Symbol::kind`
* [ ] Tests for each and tests to show precedence when conditions overlap

---

_Renamed from "set MARKED_GLOBAL and MARKED_NONLOCAL on symbols as appropriate" to "complete symbol table by populating Symbol::kind field" by @plredmond on 2024-05-29 21:02_

---

_Renamed from "complete symbol table by populating Symbol::kind field" to "[redknot] complete symbol table by populating Symbol::kind field" by @plredmond on 2024-05-29 21:02_

---

_Comment by @github-actions[bot] on 2024-05-29 21:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @carljm on 2024-06-04 23:54_

---

_Closed by @MichaReiser on 2024-06-20 07:55_

---

_Branch deleted on 2025-02-20 09:00_

---
