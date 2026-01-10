```yaml
number: 19074
title: "[ty] Add some missing calls to `normalized_impl`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/normalized-impl
created_at: 2025-07-01T16:14:44Z
updated_at: 2025-07-01T16:57:53Z
url: https://github.com/astral-sh/ruff/pull/19074
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Add some missing calls to `normalized_impl`

---

_Pull request opened by @AlexWaygood on 2025-07-01 16:14_

## Summary

I hoped this might fix the latest stack overflows on https://github.com/astral-sh/ruff/pull/18659... it doesn't look like it does, but these changes seem like they're probably correct anyway...?

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @AlexWaygood on 2025-07-01 16:17_

---

_Comment by @github-actions[bot] on 2025-07-01 16:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
jinja (https://github.com/pallets/jinja)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~97MB

black (https://github.com/psf/black)
- TOTAL MEMORY USAGE: ~129MB
+ TOTAL MEMORY USAGE: ~142MB

alerta (https://github.com/alerta/alerta)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~117MB

nox (https://github.com/wntrblm/nox)
-     memo fields = ~72MB
+     memo fields = ~66MB

mkosi (https://github.com/systemd/mkosi)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~129MB
-     memo fields = ~97MB
+     memo fields = ~106MB

tornado (https://github.com/tornadoweb/tornado)
-     memo fields = ~142MB
+     memo fields = ~129MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~171MB
+     memo fields = ~156MB

altair (https://github.com/vega/altair)
-     memo fields = ~251MB
+     memo fields = ~228MB

rotki (https://github.com/rotki/rotki)
-     memo fields = ~445MB
+     memo fields = ~490MB

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- TOTAL MEMORY USAGE: ~717MB
+ TOTAL MEMORY USAGE: ~652MB

```
</details>


---

_Marked ready for review by @AlexWaygood on 2025-07-01 16:21_

---

_Review requested from @carljm by @AlexWaygood on 2025-07-01 16:21_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-01 16:21_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-01 16:21_

---

_Label `internal` added by @AlexWaygood on 2025-07-01 16:22_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/tuple.rs`:657 on 2025-07-01 16:55_

This is the thing I'm least happy with in the current approach, is that it's so easy to just call `.normalized(db)` on a nested type when it should be `.normalized_impl(db, visitor)`. But I haven't come up with any good ideas for preventing that mistake, because `Type::normalized` does need to exist, for "external" use -- it just shouldn't be used inside another `normalized_impl` method.

---

_@carljm approved on 2025-07-01 16:55_

Thank you!

---

_Merged by @AlexWaygood on 2025-07-01 16:57_

---

_Closed by @AlexWaygood on 2025-07-01 16:57_

---

_Branch deleted on 2025-07-01 16:57_

---
