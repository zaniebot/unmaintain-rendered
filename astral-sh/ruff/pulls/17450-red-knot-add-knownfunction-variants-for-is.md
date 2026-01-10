```yaml
number: 17450
title: "[red-knot] Add `KnownFunction` variants for `is_protocol`, `get_protocol_members` and `runtime_checkable`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/protocol-knownfunctions
created_at: 2025-04-17T13:07:01Z
updated_at: 2025-04-17T13:49:54Z
url: https://github.com/astral-sh/ruff/pull/17450
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Add `KnownFunction` variants for `is_protocol`, `get_protocol_members` and `runtime_checkable`

---

_Pull request opened by @AlexWaygood on 2025-04-17 13:07_

## Summary

We want to special-case these functions to allow us to introspect red-knot's inference of protocol types (see tests at https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/protocols.md). Doing this is a necessary precondition for adding the special casing.

## Test Plan

I updated the `known_function_roundtrip_from_str` unit test. This PR doesn't actually add the special casing yet, so no new mdtests for now.


---

_Label `red-knot` added by @AlexWaygood on 2025-04-17 13:07_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-17 13:07_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-17 13:07_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-17 13:07_

---

_Comment by @github-actions[bot] on 2025-04-17 13:09_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:5900 on 2025-04-17 13:19_

No, no, no! This is clearly the place where [`typing.dataclass_transform`](https://github.com/astral-sh/ruff/pull/17445) belongs.

---

_@sharkdp approved on 2025-04-17 13:19_

---

_Merged by @AlexWaygood on 2025-04-17 13:49_

---

_Closed by @AlexWaygood on 2025-04-17 13:49_

---

_Branch deleted on 2025-04-17 13:49_

---
