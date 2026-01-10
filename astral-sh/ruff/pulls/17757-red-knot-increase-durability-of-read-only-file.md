```yaml
number: 17757
title: "[red-knot] Increase durability of read-only `File` fields"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/increase-file-durability
created_at: 2025-05-01T06:41:41Z
updated_at: 2025-05-01T07:25:50Z
url: https://github.com/astral-sh/ruff/pull/17757
synced_at: 2026-01-10T18:57:03Z
```

# [red-knot] Increase durability of read-only `File` fields

---

_Pull request opened by @MichaReiser on 2025-05-01 06:41_

## Summary

Increase the durability of read-only fields on `File` to `HIGH`

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-05-01 06:41_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-01 06:41_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-01 06:41_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-01 06:41_

---

_Label `red-knot` added by @MichaReiser on 2025-05-01 06:41_

---

_Renamed from "[red-knot] Improve durability of File fields" to "[red-knot] Increase durability of File fields" by @MichaReiser on 2025-05-01 06:42_

---

_Renamed from "[red-knot] Increase durability of File fields" to "[red-knot] Increase durability of read-only `File` fields" by @MichaReiser on 2025-05-01 06:42_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-01 06:52_

---

_Review comment by @carljm on `crates/ruff_db/src/files.rs`:168 on 2025-05-01 06:53_

What happens if permissions on a file do change? Do we treat them as immutable and ignore such changes? Do we not even watch for permission changes? I assume permissions aren't part of the file identity...

---

_@carljm approved on 2025-05-01 06:53_

Makes sense.

---

_@MichaReiser reviewed on 2025-05-01 07:25_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files.rs`:168 on 2025-05-01 07:25_

Virtual files don't have a permission, it's always set to `None`

---

_Merged by @MichaReiser on 2025-05-01 07:25_

---

_Closed by @MichaReiser on 2025-05-01 07:25_

---

_Branch deleted on 2025-05-01 07:25_

---
