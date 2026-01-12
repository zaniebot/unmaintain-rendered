```yaml
number: 17849
title: "[ty] Fix standalone expression type retrieval in presence of cycles"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-17792
created_at: 2025-05-05T10:56:22Z
updated_at: 2025-05-05T11:52:11Z
url: https://github.com/astral-sh/ruff/pull/17849
synced_at: 2026-01-12T15:56:06Z
```

# [ty] Fix standalone expression type retrieval in presence of cycles

---

_@sharkdp_

## Summary

When entering an `infer_expression_types` cycle from `TypeInferenceBuilder::infer_standalone_expression`, we might get back a `TypeInference::cycle_fallback(…)` that doesn't actually contain any new types, but instead it contains a `cycle_fallback_type` which is set to `Some(Type::Never)`. When calling `self.extend(…)`, we therefore don't really pull in a type for the expression we're interested in. This caused us to panic if we tried to call `self.expression_type(…)` after `self.extend(…)`.

The proposed fix here is to retrieve that type from the nested `TypeInferenceBuilder` directly, which will correctly fall back to `cycle_fallback_type`.

## Details

I minimized the second example from #17792 a bit further and used this example for debugging:

```py
from __future__ import annotations

class C: ...

def f(arg: C):
    pass

x, _ = f(1)

assert x
```

This is self-referential because when we check the assignment statement `x, _ = f(1)`, we need to look up the signature of `f`. Since evaluation of annotations is deferred, we look up the public type of `C` for the `arg` parameter. The public use of `C` is visibility-constraint by "`x`" via the `assert` statement. While evaluating this constraint, we need to look up the type of `x`, which in turn leads us back to the `x, _ = f(1)` definition.

The reason why this only showed up in the relatively peculiar case with unpack assignments is the code here:

https://github.com/astral-sh/ruff/blob/78b4c3ccf1d6cb10613671ccec09cafba0d1de72/crates/ty_python_semantic/src/types/infer.rs#L2709-L2718

For a non-unpack assignment like `x = f(1)`, we would not try to infer the right-hand side eagerly. Instead, we would enter a `infer_definition_types` cycle that handles the situation correctly. For unpack assignments, however, we try to infer the type of `value` (`f(1)`) and therefore enter the cycle via `standalone_expression_type => infer_expression_type`.

closes #17792 

## Test Plan

* New regression test
* Made sure that we can now run successfully on scipy => see #17850 

---

_Label `ty` added by @sharkdp on 2025-05-05 10:56_

---

_Comment by @github-actions[bot] on 2025-05-05 10:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Renamed from "[ty] Fix expression retrieval in presence of cycles" to "[ty] Fix standalone expression type retrieval in presence of cycles" by @sharkdp on 2025-05-05 11:19_

---

_Marked ready for review by @sharkdp on 2025-05-05 11:36_

---

_Review requested from @carljm by @sharkdp on 2025-05-05 11:36_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-05 11:36_

---

_Review requested from @dcreager by @sharkdp on 2025-05-05 11:36_

---

_Review requested from @MichaReiser by @sharkdp on 2025-05-05 11:36_

---

_@MichaReiser approved on 2025-05-05 11:48_

Nice find and thanks for adding an inline comment!

---

_Merged by @sharkdp on 2025-05-05 11:52_

---

_Closed by @sharkdp on 2025-05-05 11:52_

---

_Branch deleted on 2025-05-05 11:52_

---
