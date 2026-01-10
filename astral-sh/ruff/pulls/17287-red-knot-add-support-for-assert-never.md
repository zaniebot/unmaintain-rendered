```yaml
number: 17287
title: "[red-knot] Add support for `assert_never`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/assert-never
created_at: 2025-04-07T20:49:04Z
updated_at: 2025-04-08T07:31:50Z
url: https://github.com/astral-sh/ruff/pull/17287
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Add support for `assert_never`

---

_Pull request opened by @sharkdp on 2025-04-07 20:49_

## Summary

We already have partial "support" for `assert_never`, because it is annotated as 
```pyi
def assert_never(arg: Never, /) -> Never: ...
```
in typeshed. So we already emit a `invalid-argument-type` diagnostic if the argument type to `assert_never` is not assignable to `Never`.

That is not enough, however. Gradual types like `Any`, `Unknown`, `@Todo(…)` or `Any & int` can be assignable to `Never`. Which means that we didn't issue any diagnostic in those cases.

Also, it seems like `assert_never` deserves a dedicated diagnostic message, not just a generic "invalid argument type" error.

## Test Plan

New Markdown tests.


---

_Label `red-knot` added by @sharkdp on 2025-04-07 20:49_

---

_Review requested from @carljm by @sharkdp on 2025-04-07 20:49_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-07 20:49_

---

_Review requested from @dcreager by @sharkdp on 2025-04-07 20:49_

---

_@sharkdp reviewed on 2025-04-07 20:50_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:685 on 2025-04-07 20:50_

I did consider making more changes in the lint description here. I also considered a dedicated new diagnostic. But eventually thought that this might be enough? Let me know if you have a different opinion.

---

_Comment by @github-actions[bot] on 2025-04-07 20:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/directives/assert_never.md`:5 on 2025-04-07 21:01_

Should this be assert_never?

---

_@MichaReiser reviewed on 2025-04-07 21:02_

---

_@carljm approved on 2025-04-07 21:11_

---

_Merged by @sharkdp on 2025-04-08 07:31_

---

_Closed by @sharkdp on 2025-04-08 07:31_

---

_Branch deleted on 2025-04-08 07:31_

---
