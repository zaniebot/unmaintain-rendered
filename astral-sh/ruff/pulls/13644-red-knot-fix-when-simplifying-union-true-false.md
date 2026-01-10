```yaml
number: 13644
title: "[red-knot] fix: when simplifying union, True & False -> instance(bool)"
type: pull_request
state: merged
author: Slyces
labels:
  - ty
assignees: []
merged: true
base: main
head: fix/#13642-bool-union-should-be-simplified-to-instance
created_at: 2024-10-05T17:10:49Z
updated_at: 2024-10-07T08:43:25Z
url: https://github.com/astral-sh/ruff/pull/13644
synced_at: 2026-01-10T20:59:36Z
```

# [red-knot] fix: when simplifying union, True & False -> instance(bool)

---

_Pull request opened by @Slyces on 2024-10-05 17:10_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #13642

## Test Plan

- Edit the existing test
- Add a test in `infer.rs` using `reveal_type`

---

_Review requested from @carljm by @Slyces on 2024-10-05 17:10_

---

_Review requested from @MichaReiser by @Slyces on 2024-10-05 17:10_

---

_Review requested from @AlexWaygood by @Slyces on 2024-10-05 17:10_

---

_@AlexWaygood reviewed on 2024-10-05 17:16_

Looks great! We could also add a test to `infer.rs` using reveal type, with a snippet like this:

```py
from typing_extensions import reveal_type

def returns_bool() -> bool: ...

if returns_bool():
    x = True
else:
    x = False

reveal_type(x)
```

Calling `assert_file_diagnostics` on a file with this example in it should give us ```["Revealed type is `bool`"]``` rather than ```["Revealed type is `Literal[bool]`"]```.


---

_Comment by @Slyces on 2024-10-05 17:17_

Great comment, I was thinking that indeed the current tests are tightly coupled. Doing this now :)

---

_@AlexWaygood reviewed on 2024-10-05 17:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4560 on 2024-10-05 17:22_

Ah, at some point we'll start checking return types, which might lead to us emitting a spurious diagnostic in this test snippet. We can make it robust to future improvements like so:

```suggestion
            def returns_bool() -> bool:
                return True
```

---

_Comment by @github-actions[bot] on 2024-10-05 17:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-10-05 17:54_

Thanks!

---

_Merged by @AlexWaygood on 2024-10-05 18:01_

---

_Closed by @AlexWaygood on 2024-10-05 18:01_

---

_Branch deleted on 2024-10-05 18:07_

---

_Comment by @carljm on 2024-10-05 19:16_

Thank you!!

---

_Label `red-knot` added by @MichaReiser on 2024-10-07 08:43_

---
