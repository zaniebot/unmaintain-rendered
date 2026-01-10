```yaml
number: 16582
title: "[red-knot] Reduce Salsa lookups in `Type::find_name_in_mro`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/salsa-lookups
created_at: 2025-03-09T23:21:05Z
updated_at: 2025-03-10T06:55:24Z
url: https://github.com/astral-sh/ruff/pull/16582
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Reduce Salsa lookups in `Type::find_name_in_mro`

---

_Pull request opened by @AlexWaygood on 2025-03-09 23:21_

## Summary

Theoretically this should be slightly more performant, since the `class.is_known()` calls each do a separate Salsa lookup, which we can avoid if we do a single `match` on the value of `class.known()`. It also ends up being two lines less code overall!

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2025-03-09 23:21_

---

_Review requested from @carljm by @AlexWaygood on 2025-03-09 23:21_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-03-09 23:21_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-03-09 23:21_

---

_Comment by @AlexWaygood on 2025-03-09 23:27_

I got a 1% speedup locally but it's neutral on codspeed 🤷‍♂️

---

_@sharkdp approved on 2025-03-10 06:53_

Thank you for getting to it first!

---

_Merged by @sharkdp on 2025-03-10 06:55_

---

_Closed by @sharkdp on 2025-03-10 06:55_

---

_Branch deleted on 2025-03-10 06:55_

---
