```yaml
number: 18068
title: "[ty] Add a note to the diagnostic if a new builtin is used on an old Python version"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/new-builtins-note
created_at: 2025-05-13T13:36:57Z
updated_at: 2025-05-13T14:15:02Z
url: https://github.com/astral-sh/ruff/pull/18068
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Add a note to the diagnostic if a new builtin is used on an old Python version

---

_Pull request opened by @AlexWaygood on 2025-05-13 13:36_

## Summary

If the user tries to use a new builtin on an old Python version, tell them what Python version the builtin was added on, what our inferred Python version is for their project, and what configuration settings they can tweak to fix the error.

## Test Plan

Snapshots and screenshots:

![image](https://github.com/user-attachments/assets/767d570e-7af1-4e1f-98cf-50e4311db511)



---

_Review requested from @BurntSushi by @AlexWaygood on 2025-05-13 13:36_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-05-13 13:36_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-13 13:36_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-13 13:36_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-13 13:36_

---

_Label `ty` added by @AlexWaygood on 2025-05-13 13:36_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-13 13:36_

---

_Comment by @github-actions[bot] on 2025-05-13 13:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[type-assertion-failure] tests/annotations/declarations.py:956:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- error[type-assertion-failure] tests/annotations/declarations.py:961:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- Found 649 diagnostics
+ Found 647 diagnostics

```
</details>


---

_@BurntSushi approved on 2025-05-13 13:44_

Love it!

---

_@AlexWaygood reviewed on 2025-05-13 13:50_

---

_Review comment by @AlexWaygood on `crates/ty/docs/rules.md`:1 on 2025-05-13 13:50_

all the line numbers here changed because I added an import to the top of `diagnostic.rs`...

---

_@MichaReiser reviewed on 2025-05-13 14:03_

---

_Review comment by @MichaReiser on `crates/ty/docs/rules.md`:1 on 2025-05-13 14:03_

how dare you

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/diagnostic.rs`:1665 on 2025-05-13 14:05_

This shouldn't be too hard. We have the information if it comes from the CLI or configuration but we throw it away when converting it to a `ProgramSettings`. 

---

_@MichaReiser approved on 2025-05-13 14:06_

Nice

---

_Merged by @AlexWaygood on 2025-05-13 14:08_

---

_Closed by @AlexWaygood on 2025-05-13 14:08_

---

_Branch deleted on 2025-05-13 14:08_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:1663 on 2025-05-13 14:12_

It seems a bit odd to call it the "inferred" Python version if it came from eg a CLI flag. But I guess this is part of the TODO above?

---

_@carljm reviewed on 2025-05-13 14:12_

This is awesome!

---

_@AlexWaygood reviewed on 2025-05-13 14:15_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1663 on 2025-05-13 14:15_

Yes and yes 

---
