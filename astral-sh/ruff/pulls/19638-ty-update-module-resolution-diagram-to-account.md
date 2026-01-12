```yaml
number: 19638
title: "[ty] Update module resolution diagram to account for typeshed `VERSIONS` file"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: ag/typeshed-import-resolution
created_at: 2025-07-30T13:44:44Z
updated_at: 2025-07-30T14:35:00Z
url: https://github.com/astral-sh/ruff/pull/19638
synced_at: 2026-01-12T15:56:44Z
```

# [ty] Update module resolution diagram to account for typeshed `VERSIONS` file

---

_@BurntSushi_

This does unfortunately add a fair bit of complexity to the flow
diagram.

Ref https://github.com/astral-sh/ruff/pull/19620#issuecomment-3133684294


---

_Review requested from @carljm by @BurntSushi on 2025-07-30 13:44_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-07-30 13:44_

---

_Review requested from @sharkdp by @BurntSushi on 2025-07-30 13:44_

---

_Review requested from @dcreager by @BurntSushi on 2025-07-30 13:44_

---

_Review request for @dcreager removed by @BurntSushi on 2025-07-30 13:44_

---

_Review request for @carljm removed by @BurntSushi on 2025-07-30 13:44_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-07-30 13:44_

---

_Comment by @github-actions[bot] on 2025-07-30 13:46_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-30 13:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-07-30 13:51_

There's a slightly confusing bubble here in the middle of the diagram that has arrows coming from it but doesn't have any arrows leading into it: 
<img width="2222" height="1244" alt="image" src="https://github.com/user-attachments/assets/d2f5dce5-f929-4b8f-984c-86efd5f3d314" />


---

_Label `ty` added by @AlexWaygood on 2025-07-30 13:56_

---

_Label `internal` added by @AlexWaygood on 2025-07-30 13:57_

---

_Comment by @BurntSushi on 2025-07-30 13:58_

Nice catch. My eyes completely missed that!

---

_@AlexWaygood reviewed on 2025-07-30 14:07_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/import-resolution-diagram.dot`:97 on 2025-07-30 14:07_

It's a bit more  complicated than this (sorry!)

- The "root" package _must_ exist in `VERSIONS`, and it _must_ have a compatible version range. E.g. if we're resolving the module `importlib.metadata._meta`, `importlib` _must_ exist in `VERSIONS` with a compatible version range
- Intermediate modules, and indeed the actual module itself, are allowed to be omitted from `VERSIONS`: if they are omitted, then it is assumed that they have the same versions range as the root module. But if the module itself (`importlib.metadata._meta`) or any intermediate modules (`importlib.metadata`) _are_ present in `VERSIONS`, they must also have a compatible version range.

I think you could maybe just say something vaguer? Something like "Does the module exist on the configured Python version according to typeshed's `VERSIONS` file?"?

---

_@BurntSushi reviewed on 2025-07-30 14:19_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/import-resolution-diagram.dot`:97 on 2025-07-30 14:19_

Ah yes, I see. I did notice that in the version check and I was going for something vague here to capture that. But I agree I didn't go vague enough!

---

_@BurntSushi reviewed on 2025-07-30 14:22_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/import-resolution-diagram.dot`:97 on 2025-07-30 14:22_

OK, fixed. (And I fixed other instances with similar verbiage.)

---

_@AlexWaygood approved on 2025-07-30 14:28_

thank you!

---

_Merged by @BurntSushi on 2025-07-30 14:34_

---

_Closed by @BurntSushi on 2025-07-30 14:34_

---

_Branch deleted on 2025-07-30 14:35_

---
