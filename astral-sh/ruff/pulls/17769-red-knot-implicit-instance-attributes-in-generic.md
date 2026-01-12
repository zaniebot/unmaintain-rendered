```yaml
number: 17769
title: "[red-knot] Implicit instance attributes in generic methods"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-16928
created_at: 2025-05-01T15:28:54Z
updated_at: 2025-05-02T15:57:36Z
url: https://github.com/astral-sh/ruff/pull/17769
synced_at: 2026-01-12T15:56:05Z
```

# [red-knot] Implicit instance attributes in generic methods

---

_@sharkdp_

## Summary

Add the ability to detect instance attribute assignments in class methods that are generic.

This does not address the code duplication mentioned in #16928. I can open a ticket for this after this has been merged.

closes #16928

## Test Plan

Added regression test.

---

_Label `red-knot` added by @sharkdp on 2025-05-01 15:28_

---

_Comment by @github-actions[bot] on 2025-05-01 15:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@sharkdp reviewed on 2025-05-01 20:05_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index.rs`:118 on 2025-05-01 20:05_

I could add something like the following here, if we think it's useful?
```rs
debug_assert!(descendants.start.as_u32() + 1 == descendants.end.as_u32());
```

---

_Marked ready for review by @sharkdp on 2025-05-01 20:07_

---

_Review requested from @carljm by @sharkdp on 2025-05-01 20:07_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-01 20:07_

---

_Review requested from @dcreager by @sharkdp on 2025-05-01 20:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:118 on 2025-05-02 00:26_

Seems like it doesn't hurt? No strong feeling either way.

---

_@carljm approved on 2025-05-02 00:26_

Looks good!

---

_Closed by @sharkdp on 2025-05-02 06:32_

---

_Reopened by @sharkdp on 2025-05-02 06:32_

---

_@sharkdp reviewed on 2025-05-02 06:46_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index.rs`:118 on 2025-05-02 06:46_

Glad we added it:
```
fatal[panic] Panicked at crates/red_knot_python_semantic/src/semantic_index.rs:120:17 when checking `/tmp/mypy_primer/projects/mitmproxy/mitmproxy/tools/web/app.py`: `assertion failed: descendants.start.as_u32() + 1 == descendants.end.as_u32()`
```

---

_@sharkdp reviewed on 2025-05-02 08:07_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index.rs`:118 on 2025-05-02 08:07_

There is just one *child* scope, but there can be multiple *descendant* scopes (if there are nested functions inside that generic method). Updated the comment and removed the assertion.

---

_@AlexWaygood reviewed on 2025-05-02 08:12_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index.rs`:118 on 2025-05-02 08:12_

Should we also add a corpus test or mdtest with a snippet like that, so we don't have to rely on the fuzzer or primer in CI for coverage of that kind of pattern?

---

_Review requested from @MichaReiser by @sharkdp on 2025-05-02 08:17_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index.rs`:118 on 2025-05-02 08:17_

Yes, thanks. Added.

---

_@sharkdp reviewed on 2025-05-02 08:17_

---

_Merged by @sharkdp on 2025-05-02 08:20_

---

_Closed by @sharkdp on 2025-05-02 08:20_

---

_Branch deleted on 2025-05-02 08:20_

---

_Review comment by @dcreager on `crates/red_knot_project/resources/test/corpus/88_regression_generic_method_with_nested_function.py`:1 on 2025-05-02 13:43_

TIL about this directory! As an aside, do we have a convention for choosing which number to use as the prefix for the test cases in here?

---

_@dcreager reviewed on 2025-05-02 13:45_

---

_@carljm reviewed on 2025-05-02 15:57_

---

_Review comment by @carljm on `crates/red_knot_project/resources/test/corpus/88_regression_generic_method_with_nested_function.py`:1 on 2025-05-02 15:57_

Not really, and I think we've even added some without numbers. The numbers are legacy (we inherited this corpus) and I don't think they serve much purpose, we could just do a pass to remove them. I guess if they serve any purpose it's to keep related tests next to each other in a lexicographic ordering? But that can also just be done with naming.

---
