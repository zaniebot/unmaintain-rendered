```yaml
number: 18670
title: "[ty] Avoid accessing class literal with incorrect AST"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: ibraheem/infer-context-diagnostic-module
created_at: 2025-06-13T21:41:17Z
updated_at: 2025-06-14T05:02:58Z
url: https://github.com/astral-sh/ruff/pull/18670
synced_at: 2026-01-12T15:56:23Z
```

# [ty] Avoid accessing class literal with incorrect AST

---

_@ibraheemdev_

## Summary

Resolves https://github.com/astral-sh/ty/issues/652.

## Test Plan

I verified the fix locally, but this could use a test.


---

_Review requested from @carljm by @ibraheemdev on 2025-06-13 21:41_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-06-13 21:41_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-06-13 21:41_

---

_Review requested from @dcreager by @ibraheemdev on 2025-06-13 21:41_

---

_@ibraheemdev reviewed on 2025-06-13 21:43_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/class.rs`:2037 on 2025-06-13 21:43_

I moved the `parsed_module` query inside of `ClassLiteral::header_range` to avoid this footgun in the future, but this likely doesn't apply to all uses of `header_range`, so we could instead make the correct query at call-site. @MichaReiser let me know if you would prefer that, because you mentioned you wanted to remain consistent with non-salsa-queries taking a `&ParsedModuleRef`.

---

_Comment by @github-actions[bot] on 2025-06-13 21:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-06-13 21:56_

---

_Renamed from "Avoid accessing class literal with incorrect AST" to "[ty] Avoid accessing class literal with incorrect AST" by @AlexWaygood on 2025-06-13 21:57_

---

_@carljm approved on 2025-06-13 22:35_

This looks good to me; thanks for the fix! There is only one possible `ParsedModule` that is correct to use here, and we already have the information to know which it is, so there's no reason to leave open the possibility of a caller passing in the wrong one. Especially given that this results in a hard-to-understand crash, and the bug clearly can go undetected until we have a very particular cross-module scenario, so it may not be caught by tests.

It's true that this means calling `header_range` or `header_span` on a class implicitly means adopting a direct dependency on the AST of the module in which the class is defined, but that's the unavoidable nature of asking for a range/span, and it seems clear enough in the method names.

I don't mind waiting to land this until @MichaReiser has a chance to review it, though.

---

_@MichaReiser approved on 2025-06-14 05:02_

I agree with @carljm's assessment here. I think it's fine for functions to call parse internally if there's only one correct parsed module

---

_Merged by @MichaReiser on 2025-06-14 05:02_

---

_Closed by @MichaReiser on 2025-06-14 05:02_

---

_Branch deleted on 2025-06-14 05:02_

---

_Label `bug` added by @MichaReiser on 2025-06-14 05:02_

---
