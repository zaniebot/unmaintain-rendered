```yaml
number: 14101
title: "Remove `unpack` field from `SemanticIndexBuilder`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/unpack-cleanup
created_at: 2024-11-05T04:54:16Z
updated_at: 2024-11-06T03:13:00Z
url: https://github.com/astral-sh/ruff/pull/14101
synced_at: 2026-01-10T20:50:57Z
```

# Remove `unpack` field from `SemanticIndexBuilder`

---

_Pull request opened by @dhruvmanila on 2024-11-05 04:54_

## Summary

Related to https://github.com/astral-sh/ruff/pull/13979#discussion_r1828305790, this PR removes the `current_unpack` state field from `SemanticIndexBuilder` and passes the `Unpack` ingredient via the `CurrentAssignment` -> `DefinitionNodeRef` conversion to finally store it on `DefintionNodeKind`.

This involves updating the lifetime of `AnyParameterRef` (parameter to `declare_parameter`) to use the `'db` lifetime. Currently, all AST nodes stored on various enums are marked with `'a` lifetime but they're always utilized using the `'db` lifetime.

This also removes the dedicated `'a` lifetime parameter on `add_definition` which is currently being used in `DefinitionNodeRef`. As mentioned, all AST nodes live through the `'db` lifetime so we can remove the `'a` lifetime parameter from that method and use the `'db` lifetime instead.

---

_Label `red-knot` added by @dhruvmanila on 2024-11-05 04:54_

---

_Comment by @github-actions[bot] on 2024-11-05 05:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dhruvmanila on 2024-11-05 09:35_

---

_Review requested from @carljm by @dhruvmanila on 2024-11-05 09:35_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-11-05 09:35_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-11-05 09:35_

---

_Review requested from @sharkdp by @dhruvmanila on 2024-11-05 09:35_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:214 on 2024-11-05 10:21_

Nit: I would use `'a` here to be consistent with the `DefinitionNodeRef` definition and we don't use db here

---

_@MichaReiser approved on 2024-11-05 10:21_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:214 on 2024-11-05 11:58_

I think the main reason to use `'db` here is because I've used that in `DefinitionKind` as well which is the return type of `DefinitionNodeRef::into_owned` function.

---

_@dhruvmanila reviewed on 2024-11-05 11:58_

---

_@carljm approved on 2024-11-05 18:01_

LGTM, thank you!

---

_Merged by @dhruvmanila on 2024-11-06 03:12_

---

_Closed by @dhruvmanila on 2024-11-06 03:12_

---

_Branch deleted on 2024-11-06 03:13_

---
