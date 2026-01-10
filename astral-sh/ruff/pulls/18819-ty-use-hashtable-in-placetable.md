```yaml
number: 18819
title: "[ty] Use `HashTable` in `PlaceTable`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/place-table-hash-table
created_at: 2025-06-20T09:51:47Z
updated_at: 2025-06-20T16:43:53Z
url: https://github.com/astral-sh/ruff/pull/18819
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Use `HashTable` in `PlaceTable`

---

_Pull request opened by @MichaReiser on 2025-06-20 09:51_

## Summary

Using `HashMap::raw_entry` is error prone. There are so many ways to hold the API wrong. 
Instead, use the new `HashTable` type that avoids those footguns. 

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2025-06-20 09:51_

---

_Label `ty` added by @MichaReiser on 2025-06-20 09:51_

---

_Review requested from @carljm by @MichaReiser on 2025-06-20 09:51_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-20 09:51_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-20 09:51_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-20 09:51_

---

_Comment by @github-actions[bot] on 2025-06-20 09:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-20 10:24_

---

_@dhruvmanila approved on 2025-06-20 11:50_

Looks good to me!

I'd prefer if @carljm could take a quick look as well as he's probably the one most familiar with the `place.rs` code as it was mainly refactored largely to add attribute / subscript expression support.

---

_@dcreager approved on 2025-06-20 12:42_

New `HashTable` API (TIL) looks much easier to use, thank you!

---

_Merged by @MichaReiser on 2025-06-20 13:31_

---

_Closed by @MichaReiser on 2025-06-20 13:31_

---

_Branch deleted on 2025-06-20 13:31_

---

_Comment by @carljm on 2025-06-20 16:43_

Looks great, thank you! I remember in the first version of this code, we did hold the API wrong, and ended up with a symbol table that broke as soon as the internal hash table resized 😆 

---
