```yaml
number: 11327
title: "[red-knot] fix re-hashing in Files and SymbolTable"
type: pull_request
state: merged
author: carljm
labels:
  - internal
assignees: []
merged: true
base: main
head: cjm/hash-bug
created_at: 2024-05-07T21:07:24Z
updated_at: 2024-05-08T12:31:20Z
url: https://github.com/astral-sh/ruff/pull/11327
synced_at: 2026-01-10T22:05:26Z
```

# [red-knot] fix re-hashing in Files and SymbolTable

---

_Pull request opened by @carljm on 2024-05-07 21:07_

## Summary

In both `FilesInner` and `SymbolTable`, we pull this hashmap trick where we keep a `HashMap<Id, ()>` that is not actually hashed by the Id, but by a name, so it works like a `HashMap<Name, Id>` (allows looking up an id by name), but without double-storing the name (since we also keep a separate id-to-name map.)

This was breaking as soon as these hashmaps hit size 4+. As it turns out, this is sort of my fault, due to this comment: https://github.com/astral-sh/ruff/pull/10849/commits/2bfbb59aad3b96767d9043240f2f8a68c65a40f3#r1560182319

The hasher provided to `insert_with_hasher` is not specific to the entry -- it isn't stored, but will be used immediately to rehash _all_ entries in the hashmap, if a resize is needed to make room for the new entry. Reusing the hash we just calculated meant that on resize we were rehashing all existing entries to exactly the same hash (the one from the new entry), which of course broke lookup for all the older entries.

## Test Plan

Added tests for both FilesInner and SymbolTable, to test them with 4+ entries. These tests failed before this PR, and pass with it.


---

_Review requested from @MichaReiser by @carljm on 2024-05-07 21:08_

---

_Comment by @github-actions[bot] on 2024-05-07 21:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `internal` added by @carljm on 2024-05-07 22:44_

---

_@MichaReiser approved on 2024-05-08 05:35_

Uhh right. I'm anyway inclined to remove this trick :laughing: at least for files... 

---

_Merged by @carljm on 2024-05-08 12:31_

---

_Closed by @carljm on 2024-05-08 12:31_

---

_Branch deleted on 2024-05-08 12:31_

---
