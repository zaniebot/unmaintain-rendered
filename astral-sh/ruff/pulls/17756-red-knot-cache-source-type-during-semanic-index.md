```yaml
number: 17756
title: "[red-knot] Cache source type during semanic index building"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/cache-source-type
created_at: 2025-05-01T06:33:21Z
updated_at: 2025-05-01T06:51:55Z
url: https://github.com/astral-sh/ruff/pull/17756
synced_at: 2026-01-10T18:57:03Z
```

# [red-knot] Cache source type during semanic index building

---

_Pull request opened by @MichaReiser on 2025-05-01 06:33_

## Summary

I noticed this when debugging a salsa panic because the semantic index building for `types.pyi` created pages of `report_tracked_read(file.path)` calls. 
Cache the source type to avoid a lookup for every single `Definition`

## Test Plan

The pages of `report_tracked_read` are gone :)


---

_Review requested from @carljm by @MichaReiser on 2025-05-01 06:33_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-01 06:33_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-01 06:33_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-01 06:33_

---

_Label `red-knot` added by @MichaReiser on 2025-05-01 06:33_

---

_Review request for @dcreager removed by @MichaReiser on 2025-05-01 06:34_

---

_Review request for @carljm removed by @MichaReiser on 2025-05-01 06:34_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-05-01 06:34_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-05-01 06:34_

---

_Comment by @github-actions[bot] on 2025-05-01 06:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-05-01 06:38_

Looks good, thank you!

(Seems like there is a compilation issue with MSRV, though.)

---

_@AlexWaygood reviewed on 2025-05-01 06:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:122 on 2025-05-01 06:39_

```suggestion
            source_type: file.source_type(db.upcast()),
```

---

_@AlexWaygood approved on 2025-05-01 06:50_

1% speedup on the cold benchmark too — nice!

---

_Merged by @MichaReiser on 2025-05-01 06:51_

---

_Closed by @MichaReiser on 2025-05-01 06:51_

---

_Branch deleted on 2025-05-01 06:51_

---
