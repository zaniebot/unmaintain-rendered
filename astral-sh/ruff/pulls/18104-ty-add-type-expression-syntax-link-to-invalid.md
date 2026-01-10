```yaml
number: 18104
title: "[ty] Add type-expression syntax link to invalid-type-expression"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: david/invalid-type-expression-diag-info
created_at: 2025-05-14T18:28:12Z
updated_at: 2025-05-14T18:57:32Z
url: https://github.com/astral-sh/ruff/pull/18104
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Add type-expression syntax link to invalid-type-expression

---

_Pull request opened by @sharkdp on 2025-05-14 18:28_

## Summary

Add a link to [this page](https://typing.python.org/en/latest/spec/annotations.html#type-and-annotation-expressions) when emitting `invalid-type-expression` diagnostics:

![image](https://github.com/user-attachments/assets/b3cc8938-9f6d-4922-960b-2412a15579ed)

---

_Review requested from @carljm by @sharkdp on 2025-05-14 18:28_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-14 18:28_

---

_Review requested from @dcreager by @sharkdp on 2025-05-14 18:28_

---

_Label `ty` added by @sharkdp on 2025-05-14 18:28_

---

_Label `diagnostics` added by @sharkdp on 2025-05-14 18:28_

---

_@sharkdp reviewed on 2025-05-14 18:29_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:7748 on 2025-05-14 18:29_

I remember @AlexWaygood saying that the spec is not supposed to be user facing, but this seems clearly helpful. Or are there any objections?

---

_@AlexWaygood reviewed on 2025-05-14 18:30_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:7748 on 2025-05-14 18:30_

I think this is perfect!

---

_Comment by @AlexWaygood on 2025-05-14 18:30_

Could you also add the link to this one: https://github.com/astral-sh/ruff/blob/4b003d97592dfade2b38f133e256f8d2c754aec7/crates/ty_python_semantic/src/types/infer.rs#L8574-L8582

---

_Comment by @github-actions[bot] on 2025-05-14 18:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-05-14 18:32_

And maybe this one? https://github.com/astral-sh/ruff/blob/4b003d97592dfade2b38f133e256f8d2c754aec7/crates/ty_python_semantic/src/types/infer.rs#L8803-L8809

---

_Comment by @AlexWaygood on 2025-05-14 18:34_

And a link in the rule's docs themselves might also be great: https://github.com/astral-sh/ruff/blob/68559fc17d492f335dbab93214f6862b6b8c5515/crates/ty_python_semantic/src/types/diagnostic.rs#L789-L810

---

_@AlexWaygood reviewed on 2025-05-14 18:34_

Thank you!!

---

_Review requested from @MichaReiser by @sharkdp on 2025-05-14 18:51_

---

_@sharkdp reviewed on 2025-05-14 18:52_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/diagnostic.rs`:806 on 2025-05-14 18:52_

oops, this should be one line below.

---

_@AlexWaygood reviewed on 2025-05-14 18:54_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:13 on 2025-05-14 18:54_

if you do this then maybe you don't get any line numbers changing in the generated docs :P

```suggestion
use crate::types::{protocol_class::ProtocolClassLiteral, KnownFunction, KnownInstanceType, LintDiagnosticGuard, Type};
```

---

_@AlexWaygood approved on 2025-05-14 18:55_

---

_Merged by @sharkdp on 2025-05-14 18:56_

---

_Closed by @sharkdp on 2025-05-14 18:56_

---

_Branch deleted on 2025-05-14 18:56_

---
