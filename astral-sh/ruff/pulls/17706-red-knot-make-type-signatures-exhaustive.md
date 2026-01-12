```yaml
number: 17706
title: "[red-knot] Make `Type::signatures()` exhaustive"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/signatures-exhaustive
created_at: 2025-04-29T14:02:21Z
updated_at: 2025-04-29T14:14:10Z
url: https://github.com/astral-sh/ruff/pull/17706
synced_at: 2026-01-12T15:56:04Z
```

# [red-knot] Make `Type::signatures()` exhaustive

---

_@AlexWaygood_

## Summary

I had a bug for a while on my protocols branch because I hadn't added an arm in this `match` statement for the new `Type::Protocol` variant. It would have been easier to track the bug down if I'd had a compile error telling me that I needed to add an arm for the new variant -- but there was no compile error, because of the fallback `_ => Signatures::not_callable(self)` branch at the end here.

This PR makes the `match` exhaustive, so that others don't have this problem in the future! It also looks like there might already be some bugs here due to existing `Type` variants not having dedicated arms in the `match`; I've added some TODOs to the code.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2025-04-29 14:02_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-29 14:02_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-29 14:02_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-29 14:02_

---

_Comment by @github-actions[bot] on 2025-04-29 14:05_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser approved on 2025-04-29 14:08_

---

_Merged by @AlexWaygood on 2025-04-29 14:14_

---

_Closed by @AlexWaygood on 2025-04-29 14:14_

---

_Branch deleted on 2025-04-29 14:14_

---
