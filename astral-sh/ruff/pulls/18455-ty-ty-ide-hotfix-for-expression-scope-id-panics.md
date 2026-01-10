```yaml
number: 18455
title: "[ty] ty_ide: Hotfix for `expression_scope_id` panics"
type: pull_request
state: merged
author: sharkdp
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: david/hotfix-playground-scoped_expression_id
created_at: 2025-06-04T08:28:38Z
updated_at: 2025-06-04T15:10:34Z
url: https://github.com/astral-sh/ruff/pull/18455
synced_at: 2026-01-10T18:45:04Z
```

# [ty] ty_ide: Hotfix for `expression_scope_id` panics

---

_Pull request opened by @sharkdp on 2025-06-04 08:28_

## Summary

Implement a hotfix for the playground/LSP crashes related to missing `expression_scope_id`s.

relates to: https://github.com/astral-sh/ty/issues/572

## Test Plan

* Regression tests from https://github.com/astral-sh/ruff/pull/18441
* Ran the playground locally to check if panics occur / completions still work.

---

_Review requested from @carljm by @sharkdp on 2025-06-04 08:28_

---

_Label `server` added by @sharkdp on 2025-06-04 08:28_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-06-04 08:28_

---

_Review requested from @dcreager by @sharkdp on 2025-06-04 08:28_

---

_Review requested from @MichaReiser by @sharkdp on 2025-06-04 08:28_

---

_Label `ty` added by @sharkdp on 2025-06-04 08:28_

---

_Comment by @github-actions[bot] on 2025-06-04 08:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser approved on 2025-06-04 08:38_

ty

---

_Merged by @sharkdp on 2025-06-04 08:39_

---

_Closed by @sharkdp on 2025-06-04 08:39_

---

_Branch deleted on 2025-06-04 08:39_

---

_@carljm reviewed on 2025-06-04 15:01_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_model.rs`:52 on 2025-06-04 15:01_

(Not a request to actually change this comment, but) I'm not convinced we should necessarily ever change this back to using `expression_scope_id`, regardless of what other fixes are made.

---

_@sharkdp reviewed on 2025-06-04 15:10_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_model.rs`:52 on 2025-06-04 15:10_

Admittedly, I didn't follow the full discussion in https://github.com/astral-sh/ruff/pull/18441, so I was potentially overly-cautious here. I opened this PR mainly because I can't work without the ty playground anymore :upside_down_face:.

---
