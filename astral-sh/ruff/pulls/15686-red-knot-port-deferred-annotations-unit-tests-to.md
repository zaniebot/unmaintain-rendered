```yaml
number: 15686
title: "[red-knot] Port 'deferred annotations' unit tests to Markdown"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/port-deferred-annotations-tests
created_at: 2025-01-23T10:10:34Z
updated_at: 2025-01-23T11:45:06Z
url: https://github.com/astral-sh/ruff/pull/15686
synced_at: 2026-01-12T15:55:52Z
```

# [red-knot] Port 'deferred annotations' unit tests to Markdown

---

_@sharkdp_

## Summary

- Port "deferred annotations" unit tests to Markdown
- Port `implicit_global_in_function` unit test to Markdown
- Removed `resolve_method` and `local_inference` unit tests. These seem like relics from a time where type inference was in it's early stages. There is no way that these tests would fail today without lots of other things going wrong as well.

part of #13696
based on #15683 

## Test Plan

New MD tests for existing Rust unit tests.


---

_Label `red-knot` added by @sharkdp on 2025-01-23 10:10_

---

_Review requested from @carljm by @sharkdp on 2025-01-23 10:10_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-23 10:10_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-23 10:10_

---

_@sharkdp reviewed on 2025-01-23 10:11_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:6084 on 2025-01-23 10:11_

Removed without replacement.

---

_@sharkdp reviewed on 2025-01-23 10:11_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:6265 on 2025-01-23 10:11_

Removed without replacement.

---

_Comment by @github-actions[bot] on 2025-01-23 10:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/deferred.md`:13 on 2025-01-23 10:40_

huh. I'm not sure what this was originally testing, but it now seems hopelessly superseded by the tests in https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/mro.md. I think we can probably just delete it?

---

_@AlexWaygood approved on 2025-01-23 10:43_

Thank you!

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/annotations/deferred.md`:13 on 2025-01-23 11:35_

> huh. I'm not sure what this was originally testing

Glad it's not just me :smile:. I'll delete it.

---

_@sharkdp reviewed on 2025-01-23 11:35_

---

_Merged by @sharkdp on 2025-01-23 11:45_

---

_Closed by @sharkdp on 2025-01-23 11:45_

---

_Branch deleted on 2025-01-23 11:45_

---
