```yaml
number: 11536
title: "[redknot] unspecified-encoding lint rule"
type: pull_request
state: closed
author: plredmond
labels: []
assignees: []
draft: true
base: main
head: redknot.encodinglint
created_at: 2024-05-24T21:56:24Z
updated_at: 2025-02-20T09:00:25Z
url: https://github.com/astral-sh/ruff/pull/11536
synced_at: 2026-01-12T15:55:38Z
```

# [redknot] unspecified-encoding lint rule

---

_@plredmond_

Implement an [unspecified_encoding.py](https://github.com/augustelalande/ruff/blob/e1029f1230e51ba75c746bbe5ad0c399e2afa39a/crates/ruff_linter/resources/test/fixtures/pylint/unspecified_encoding.py) lint rule like the one in [#11288](https://github.com/astral-sh/ruff/pull/11288).

```python
import tempfile

tempfile.TemporaryFile("w")  # error

tempfile.TemporaryFile("w", encoding="utf-8")  # ok
```
This is still a draft.

---

_Comment by @github-actions[bot] on 2024-05-24 22:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot/src/lint.rs`:203 on 2024-05-28 03:53_

I'm not sure that iterating definitions is actually right for this lint rule. I don't think we really care if it's a definition or not, we'd want to detect all calls to e.g. `tempfile.TemporaryFile` that don't specify an encoding. So I think this lint rule is a better fit for an AST traversal (which we don't currently have an example of in the red-knot lint rules).

I think the algorithm should be, walk all calls, infer type of the `.func` of the call, check if its `tempfile.TemporaryFile`, if so, check the arguments and see if an encoding is specified or not.

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:163 on 2024-05-28 03:54_

I don't think we will need this for this lint rule (it doesn't require actually inferring type of an entire call expression). Of course we will need it eventually.

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:166 on 2024-05-28 03:55_

I think our approach in inference-only mode in cases that should really be a type error should be to substitute `Unknown` type.

---

_@carljm reviewed on 2024-05-28 03:55_

---

_@plredmond reviewed on 2024-05-28 22:09_

---

_Review comment by @plredmond on `crates/red_knot/src/lint.rs`:203 on 2024-05-28 22:09_

Yeah, thought this seemed awkward-- iterating over definitions is great for the type inference algorithm, but would have poor recall in this rule. Should I go ahead and use `ruff_python_ast::visitor` since the AST type used here in redknot is the same as that of ruff?

---

_@plredmond reviewed on 2024-05-28 22:09_

---

_Review comment by @plredmond on `crates/red_knot/src/types/infer.rs`:163 on 2024-05-28 22:09_

I'll remove it. Lmk if you decide I should keep it and I can revert.

---

_@carljm reviewed on 2024-05-28 22:38_

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:163 on 2024-05-28 22:38_

No I don't think it makes sense to add it in this PR.

---

_@carljm reviewed on 2024-05-28 22:40_

---

_Review comment by @carljm on `crates/red_knot/src/lint.rs`:203 on 2024-05-28 22:40_

Yes, you can use `ruff_python_ast::visitor`. Longer term we will want to do something more efficient, e.g. where we do a single AST traversal that lint rules can register callbacks on. But for now given this is our first prototype lint rule doing an AST traversal, it's fine IMO to just traverse internal to the rule.

---

_@MichaReiser reviewed on 2024-05-29 08:14_

---

_Review comment by @MichaReiser on `crates/red_knot/src/lint.rs`:203 on 2024-05-29 08:14_

I would build something similar to the `SyntaxLintVisitor` but for semantic rules where you can call into your lint rule for `CallExpressions`. 

https://github.com/astral-sh/ruff/blob/b6b4ad99497b65ebc6b8c21944fe41115c32477a/crates/red_knot/src/lint.rs#L239-L255

---

_Closed by @MichaReiser on 2024-06-20 07:55_

---

_Branch deleted on 2025-02-20 09:00_

---
