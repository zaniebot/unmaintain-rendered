```yaml
number: 15533
title: "[red-knot] Migrate `is_fully_static`/`is_single_valued`/`is_singleton` unit tests to Markdown tests"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-pt-rest
created_at: 2025-01-16T14:59:30Z
updated_at: 2025-01-16T15:40:55Z
url: https://github.com/astral-sh/ruff/pull/15533
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Migrate `is_fully_static`/`is_single_valued`/`is_singleton` unit tests to Markdown tests

---

_Pull request opened by @InSyncWithFoo on 2025-01-16 14:59_

## Summary

Part of #15397.

## Test Plan

Markdown tests.

---

_Review requested from @carljm by @InSyncWithFoo on 2025-01-16 14:59_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-01-16 14:59_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-01-16 14:59_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-01-16 14:59_

---

_Label `red-knot` added by @AlexWaygood on 2025-01-16 15:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_fully_static.md`:42 on 2025-01-16 15:16_

```suggestion
from knot_extensions import Intersection, Not, TypeOf, Unknown, is_fully_static, static_assert
```

---

_@AlexWaygood reviewed on 2025-01-16 15:16_

---

_@AlexWaygood reviewed on 2025-01-16 15:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_fully_static.md`:41 on 2025-01-16 15:16_

```suggestion
from typing_extensions import Any, Literal, LiteralString
```

---

_@AlexWaygood reviewed on 2025-01-16 15:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_single_valued.md`:6 on 2025-01-16 15:17_

```suggestion
from typing_extensions import Any, Literal, LiteralString, Never
```

---

_Comment by @github-actions[bot] on 2025-01-16 15:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2025-01-16 15:39_

Looks good, thank you!

---

_Merged by @carljm on 2025-01-16 15:40_

---

_Closed by @carljm on 2025-01-16 15:40_

---

_Branch deleted on 2025-01-16 15:40_

---
