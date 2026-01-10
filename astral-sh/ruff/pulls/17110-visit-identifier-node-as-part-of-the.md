```yaml
number: 17110
title: "Visit `Identifier` node as part of the `SourceOrderVisitor`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/source-order-visitor-visit-identifier
created_at: 2025-04-01T08:58:55Z
updated_at: 2025-04-01T14:58:12Z
url: https://github.com/astral-sh/ruff/pull/17110
synced_at: 2026-01-10T19:40:37Z
```

# Visit `Identifier` node as part of the `SourceOrderVisitor`

---

_Pull request opened by @MichaReiser on 2025-04-01 08:58_

## Summary

I don't remember exactly when we made `Identifier` a node but it is now considered a node (it implements `AnyNodeRef`, it has a range). However, we never updated
the `SourceOrderVisitor` to visit identifiers because we never had a use case for it and visiting new nodes can change how the formatter associates comments (breaking change!). 
This PR updates the `SourceOrderVisitor` to visit identifiers and changes the formatter comment visitor to skip identifiers (updating the visitor might be desired because it could help simplifying some comment placement logic but this is out of scope for this PR). 

## Test Plan

Tests, updated snapshot tests


---

_Label `internal` added by @MichaReiser on 2025-04-01 08:59_

---

_Comment by @github-actions[bot] on 2025-04-01 09:07_

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

_@ntBre approved on 2025-04-01 14:55_

Nice! I might have to try using this for the `match` pattern visitor from #16957.

---

_Comment by @MichaReiser on 2025-04-01 14:57_

Yeah, I was too lazy to also change Visitor but I think we could (we just need to be careful and review the call sites to skip visiting if it can change behavior). But you can also just use the `SourceOrderVisitor` if you don't require semantic order

---

_Merged by @MichaReiser on 2025-04-01 14:58_

---

_Closed by @MichaReiser on 2025-04-01 14:58_

---

_Branch deleted on 2025-04-01 14:58_

---
