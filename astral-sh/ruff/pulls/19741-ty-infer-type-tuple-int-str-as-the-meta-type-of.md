```yaml
number: 19741
title: "[ty] Infer `type[tuple[int, str]]` as the meta-type of `tuple[int, str]`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/tuple-meta-type
created_at: 2025-08-04T11:26:02Z
updated_at: 2025-08-04T13:10:50Z
url: https://github.com/astral-sh/ruff/pull/19741
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Infer `type[tuple[int, str]]` as the meta-type of `tuple[int, str]`

---

_Pull request opened by @AlexWaygood on 2025-08-04 11:26_

This PR is based on top of #19735; review that one first!

## Summary

The type `tuple[str, int]` does not only have exact instances of `tuple` as its inhabitants: its inhabitants also include any instances of subclasses of `tuple[str, int]`. As such, the meta-type of `tuple[str, int]` should be `type[tuple[str, int]]` rather than `<class 'tuple[str, int]'>`. The former accurately reflects the fact that given an instance of `tuple[str, int]`, we do not know exactly what the `__class__` of that instance will be: we only know that it will be a subclass of `tuple[str, int]`. The latter (what we currently infer) is incorrectly precise: it implies that all instances of `tuple[str, int]` have the runtime object `tuple` as their `__class__`, which isn't true.

## Test Plan

Mdtests updated.


---

_Label `ty` added by @AlexWaygood on 2025-08-04 11:26_

---

_Comment by @github-actions[bot] on 2025-08-04 11:27_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-04 11:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-08-04 11:38_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-04 11:38_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-04 11:38_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-04 11:38_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-08-04 12:48_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:6091 on 2025-08-04 12:58_

Would it be sufficient to just call `class.is_not_generic()` here? I think that will handle all three cases (class literal, generic alias, subclass-of) correctly

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:6091 on 2025-08-04 12:59_

Does this comment no longer apply? It doesn't look like this PR changes the logic that this refers to

---

_@dcreager approved on 2025-08-04 13:00_

couple of nits, otherwise looks good

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:6091 on 2025-08-04 13:02_

oops, didn't mean to delete that! Thanks

---

_@AlexWaygood reviewed on 2025-08-04 13:02_

---

_@AlexWaygood reviewed on 2025-08-04 13:05_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:6091 on 2025-08-04 13:05_

🤦 that's so much simpler; not sure why I didn't think of that!

---

_@AlexWaygood reviewed on 2025-08-04 13:07_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:6091 on 2025-08-04 13:07_

added it back

---

_Merged by @AlexWaygood on 2025-08-04 13:10_

---

_Closed by @AlexWaygood on 2025-08-04 13:10_

---

_Branch deleted on 2025-08-04 13:10_

---
