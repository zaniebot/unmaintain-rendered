```yaml
number: 18067
title: "[ty] Add tests for `else` branches of `hasattr()` narrowing"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/hasattr-else-test
created_at: 2025-05-13T13:13:51Z
updated_at: 2025-05-13T13:58:41Z
url: https://github.com/astral-sh/ruff/pull/18067
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Add tests for `else` branches of `hasattr()` narrowing

---

_Pull request opened by @AlexWaygood on 2025-05-13 13:13_

## Summary

This addresses @sharkdp's post-merge review in https://github.com/astral-sh/ruff/pull/18053#discussion_r2086190617

## Test Plan

`cargo test -p ty_python_semantic`


---

_Review requested from @carljm by @AlexWaygood on 2025-05-13 13:13_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-13 13:13_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-13 13:13_

---

_Label `internal` added by @AlexWaygood on 2025-05-13 13:13_

---

_Label `testing` added by @AlexWaygood on 2025-05-13 13:13_

---

_Label `ty` added by @AlexWaygood on 2025-05-13 13:13_

---

_Comment by @github-actions[bot] on 2025-05-13 13:17_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ error[type-assertion-failure] tests/annotations/declarations.py:956:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/annotations/declarations.py:961:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- Found 647 diagnostics
+ Found 649 diagnostics

```
</details>


---

_Label `internal` removed by @MichaReiser on 2025-05-13 13:27_

---

_Comment by @sharkdp on 2025-05-13 13:51_

Wait, why do we have a mypy_primer diff on this PR? :thinking: 

---

_@sharkdp approved on 2025-05-13 13:51_

Thank you!

---

_Merged by @AlexWaygood on 2025-05-13 13:57_

---

_Closed by @AlexWaygood on 2025-05-13 13:57_

---

_Branch deleted on 2025-05-13 13:57_

---

_Comment by @AlexWaygood on 2025-05-13 13:58_

> Wait, why do we have a mypy_primer diff on this PR? 🤔

No idea... Those specific hits seem to be showing up on a bunch of PRs right now...

---
