```yaml
number: 15453
title: "[red-knot] Add boundness and declaredness tests"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/boundness-declaredness-tests
created_at: 2025-01-13T11:42:58Z
updated_at: 2025-01-14T12:07:18Z
url: https://github.com/astral-sh/ruff/pull/15453
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] Add boundness and declaredness tests

---

_@sharkdp_

## Summary

This changeset adds new tests for public types of symbols and diagnostics, considering all possible declaredness and boundness states. This was previously not the case as [discussed here].

Note that this is a mere documentation of the current behavior. There is still an [open ticket] questioning some of these choices (or unintential behaviors).

| **Public type**  | declared     | possibly-undeclared        | undeclared   |
| ---------------- | ------------ | -------------------------- | ------------ |
| bound            | `T_declared` | `T_declared \| T_inferred` | `T_inferred` |
| possibly-unbound | `T_declared` | `T_declared \| T_inferred` | `T_inferred` |
| unbound          | `T_declared` | `T_declared`               | `Unknown`    |

| **Diagnostic**   | declared | possibly-undeclared       | undeclared          |
| ---------------- | -------- | ------------------------- | ------------------- |
| bound            |          |                           |                     |
| possibly-unbound |          | `possibly-unbound-import` |                     |
| unbound          |          |                           | `unresolved-import` |

## Test plan

Made sure that the respective test fails if I add the questionable case again in `symbol_by_id`:

```rs
Symbol::Type(inferred_ty, Boundness::Bound) => {
    Symbol::Type(inferred_ty, Boundness::Bound)
}
```

[discussed here]: https://github.com/astral-sh/ruff/pull/15019#discussion_r1890970501
[open ticket]: https://github.com/astral-sh/ty/issues/229


---

_Label `red-knot` added by @sharkdp on 2025-01-13 11:42_

---

_Review requested from @carljm by @sharkdp on 2025-01-13 11:42_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-13 11:42_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-13 11:42_

---

_Comment by @github-actions[bot] on 2025-01-13 11:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness.md`:29 on 2025-01-13 13:08_

```suggestion
If a symbol has a declared type (`int`), we use that as the symbol's "public type" (the type of the symbol from the perspective of other scopes) even if there is a more precise local inferred type for the symbol
```

---

_@AlexWaygood approved on 2025-01-13 13:12_

Nice! This will make it much easier to see how our behaviour changes if we decide to tweak some of this in the future

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness.md`:17 on 2025-01-13 19:17_

While there may be other behaviors here that we might reconsider, I think the fact that `possibly-unbound-import` diagnostic occurs in the "possibly-undeclared and possibly-unbound" case but not in the "undeclared and possibly-unbound" or "unbound and possibly-undeclared" cases is clearly a bug, I don't see any justification for it. I'm tempted to call that out more clearly with a TODO here (and/or below where those specific cases are tested.) I don't feel strongly about it, though, the existing note below is sufficient and refers to an issue that clearly discusses the problem.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness.md`:29 on 2025-01-13 19:18_

This clarification applies to the entire document, not just this test. I do think it would be useful to outline more clearly what we mean by "public symbols" or "public types", but that should probably happen at the top of the document, not here?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness.md`:138 on 2025-01-13 19:19_

The "we also don't raise an error" here seems confusing, since in the prior case we _do_ raise an error (and it sure seems like either we should here, or we shouldn't there.)

---

_@carljm approved on 2025-01-13 19:20_

Awesome, thank you! Very clear exposition of a confusing area.

---

_@carljm reviewed on 2025-01-13 19:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness.md`:3 on 2025-01-13 19:22_

Maybe clarify scoping of this document. Here at top-level we mention "public symbols", but then there's a heading `## Public symbols` below, and the name of the file doesn't mention "public", so it seems a little unclear whether this document is intended to (eventually?) cover boundedness and declaredness for non-public cases as well.

(Personally I would be inclined to say this document is large enough already, and well-scoped as-is, so I'd probably clarify the headings/names to explicitly make this whole document about public symbols. But either way is fine.)

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness.md`:138 on 2025-01-14 11:16_

Oh, I moved subsections around too often. Will clarify.

---

_@sharkdp reviewed on 2025-01-14 11:16_

---

_@sharkdp reviewed on 2025-01-14 11:49_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness.md`:17 on 2025-01-14 11:49_

> but not in the "undeclared and possibly-unbound" or "unbound and possibly-undeclared"

I agree. In this table, when moving to the right or to the bottom, things only get worse. So if we raise a diagnostic in a particular cell, then everything in the lower-right quadrant of that cell (inclusive) should also raise a diagnostic. I added additional text, TODO-notes in the detailed examples, and question-marks in the corresponding cells in the table.

---

_Merged by @sharkdp on 2025-01-14 12:07_

---

_Closed by @sharkdp on 2025-01-14 12:07_

---

_Branch deleted on 2025-01-14 12:07_

---
