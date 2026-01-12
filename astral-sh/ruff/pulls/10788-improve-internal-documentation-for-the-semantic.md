```yaml
number: 10788
title: Improve internal documentation for the semantic model
type: pull_request
state: merged
author: AlexWaygood
labels:
  - documentation
  - internal
assignees: []
merged: true
base: main
head: semantic-model-docs
created_at: 2024-04-05T11:11:09Z
updated_at: 2024-07-01T10:29:49Z
url: https://github.com/astral-sh/ruff/pull/10788
synced_at: 2026-01-12T15:55:33Z
```

# Improve internal documentation for the semantic model

---

_@AlexWaygood_

## Summary

This PR improves internal documentation around several parts of the semantic model. The clarifications here come from reading @charliermarsh's helpful summary document he recently sent round internally, and work done with @carljm yesterday trying to figure out how to solve https://github.com/astral-sh/ruff/issues/3011.

Changes made:
1. Clarifications are made to `SemanticModelFlags::FUTURE_ANNOTATIONS`:

	a). This flag is no longer set in all stub files, as was previously the case. Setting this flag in stub files was somewhat confusing as, although stub files have PEP-563 semantics at all times, they very rarely actually have `from __future__ import annotations` at the top of the module.

	b). `SemanticModel::future_annotations()` is renamed to `SemanticModel::future_annotations_or_stub()`, and now checks whether _either_ `SemanticModelFlags::FUTURE_ANNOTATIONS` is set _or_ `SemanticModelFlags::STUB_FILE` is set. This more accurately reflects what this function has always done (return `true` if the model was _either_ visiting a stub _or_ visiting a runtime module with `from __future__ import annotations`) since previously `SemanticModelFlags::FUTURE_ANNOTATIONS` was set in all stub files.

2. Several doc-comments are added, corrected, or expanded upon
3. A debug assertion is added to `Checker::visit_deferred_future_type_definitions()`, to ensure that no type definitions have been invalidly marked as "deferred future type definitions".

## Test Plan

`cargo test`


---

_Label `internal` added by @AlexWaygood on 2024-04-05 11:11_

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-04-05 11:11_

---

_Comment by @AlexWaygood on 2024-04-05 11:25_

> ## Test Plan
> 
> `cargo test`

I also ran this PR branch locally on typeshed, since we don't have as much test coverage for stub files, and there were no changes in the lints Ruff emitted

---

_Comment by @github-actions[bot] on 2024-04-05 11:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-04-06 16:11_

---

_Label `documentation` added by @charliermarsh on 2024-04-06 16:11_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/mod.rs`:939 on 2024-04-06 16:17_

```suggestion
            // `in_deferred_type_definition()` will only be `true` if we're now visiting the deferred nodes
```

---

_@AlexWaygood reviewed on 2024-04-06 16:20_

---

_Merged by @AlexWaygood on 2024-04-06 16:28_

---

_Closed by @AlexWaygood on 2024-04-06 16:28_

---

_Branch deleted on 2024-07-01 10:29_

---
