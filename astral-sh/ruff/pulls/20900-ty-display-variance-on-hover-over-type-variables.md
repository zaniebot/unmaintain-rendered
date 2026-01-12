```yaml
number: 20900
title: "[ty] display variance on hover over type variables"
type: pull_request
state: merged
author: mtshiba
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: hover-variance
created_at: 2025-10-15T17:11:10Z
updated_at: 2025-10-20T17:28:59Z
url: https://github.com/astral-sh/ruff/pull/20900
synced_at: 2026-01-12T15:57:12Z
```

# [ty] display variance on hover over type variables

---

_@mtshiba_

## Summary

Edit: please merge #20924 first

Closes astral-sh/ty#1348

The variance of a type variable is displayed (next to the type) when hovered over.
They are also displayed in the type parameters list.

This is an enhancement to the language server, but also includes some internal additions to `ty_python_semantic` (such as calculating variances for type aliases).

## Test Plan

New tests in `ty_ide/hover/tests`.


---

_Comment by @github-actions[bot] on 2025-10-15 17:13_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-15 17:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @mtshiba on 2025-10-15 17:33_

---

_Review requested from @carljm by @mtshiba on 2025-10-15 17:33_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-10-15 17:33_

---

_Review requested from @sharkdp by @mtshiba on 2025-10-15 17:33_

---

_Review requested from @dcreager by @mtshiba on 2025-10-15 17:33_

---

_Review requested from @MichaReiser by @mtshiba on 2025-10-15 17:33_

---

_Label `server` added by @AlexWaygood on 2025-10-15 17:35_

---

_Label `ty` added by @AlexWaygood on 2025-10-15 17:35_

---

_@MichaReiser reviewed on 2025-10-16 08:49_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:10814 on 2025-10-16 08:49_

Is this a salsa query to avoid cross-module dependencies? I'm asking because I don't think it's worth caching otherwise, given that all functions within are mostly cached.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/hover.rs`:37 on 2025-10-16 08:54_

Nit, I think I'd move the mapping to `HoverContent` to line 52. 

```suggestion
    let ty = goto_target.inferred_type(&model);
    
    ...
    
    let mut contents = Vec::new();
    contents.extend(match ty {
        Type::KnownInstance(KnownInstanceType::TypeVar(typevar)) => typevar
            .bind_pep695(db)
            .map_or(HoverContent::Type(ty, None), |typevar| {
                HoverContent::Type(
                    Type::NonInferableTypeVar(typevar),
                    Some(typevar.variance(db)),
                )
            }),
        Type::TypeVar(typevar) | Type::NonInferableTypeVar(typevar) => {
            HoverContent::Type(ty, Some(typevar.variance(db)))
        }
        _ => HoverContent::Type(ty, None),
    });
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:10762 on 2025-10-16 08:57_

Should `value_type` call into `raw_value_type` to avoid repeating the chunk up to where we apply the specialization?

---

_@MichaReiser approved on 2025-10-16 08:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7509 on 2025-10-16 10:36_

Can you add some tests for this to `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`? This even seems like it could be separated out as a standalone semantics-only PR, with the on-hover improvements in a separate PR based on top of that

---

_@AlexWaygood reviewed on 2025-10-16 10:36_

Thanks!

---

_Review request for @sharkdp removed by @sharkdp on 2025-10-17 07:21_

---

_Comment by @MichaReiser on 2025-10-17 10:23_

@mtshiba, unfortunately, there are now some merge conflicts.

---

_Converted to draft by @mtshiba on 2025-10-18 04:05_

---

_Comment by @mtshiba on 2025-10-19 13:21_

Divergence suppression for recursive aliases is addressed in #20969. Please check it out.

---

_Marked ready for review by @mtshiba on 2025-10-19 13:35_

---

_Comment by @MichaReiser on 2025-10-20 07:50_

The hover changes look good to me but this PR also includes the changes of https://github.com/astral-sh/ruff/pull/20969

Are those changes necessary for this PR? If not, I suggest backing them out, so that we can merge this PR without having to wait for https://github.com/astral-sh/ruff/pull/20969

---

_Comment by @mtshiba on 2025-10-20 17:15_

Changes that overlap with #20969 have been completely moved to #20969.

---

_@MichaReiser approved on 2025-10-20 17:25_

---

_Merged by @MichaReiser on 2025-10-20 17:28_

---

_Closed by @MichaReiser on 2025-10-20 17:28_

---

_Branch deleted on 2025-10-20 17:28_

---
