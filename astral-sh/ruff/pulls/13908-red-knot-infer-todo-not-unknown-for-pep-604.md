```yaml
number: 13908
title: "[red-knot] Infer `Todo`, not `Unknown`, for PEP-604 unions in annotations"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/pep604-todo
created_at: 2024-10-24T12:38:05Z
updated_at: 2024-10-28T12:44:04Z
url: https://github.com/astral-sh/ruff/pull/13908
synced_at: 2026-01-12T15:55:46Z
```

# [red-knot] Infer `Todo`, not `Unknown`, for PEP-604 unions in annotations

---

_@AlexWaygood_

## Summary

We don't support PEP-604 annotations yet, but we _should_, so this should be `Type::Todo`, not `Type::Unknown`.

Making this change on its own results in several new spurious errors in the `tomllib` benchmark:

```
    "/src/tomllib/_parser.py:108:17: Conflicting declared types for `second_char`: Unknown, @Todo",
    "/src/tomllib/_parser.py:267:9: Conflicting declared types for `char`: Unknown, @Todo",
    "/src/tomllib/_parser.py:364:9: Conflicting declared types for `char`: Unknown, @Todo",
    "/src/tomllib/_parser.py:381:13: Conflicting declared types for `char`: Unknown, @Todo",
    "/src/tomllib/_parser.py:395:9: Conflicting declared types for `char`: Unknown, @Todo",
    "/src/tomllib/_parser.py:590:9: Conflicting declared types for `char`: Unknown, @Todo",
```

But it doesn't seem correct to me to emit these errors (since `Todo` is only ever inferred in situations where we acknowledge our inference is currently lacking), so I've added some logic to filter out `Todo` from the `conflicting` vector in `red_knot_python_semantic::types::declarations_ty`.

This unblocks #13904: without this change, that PR leads to many false-positive or counter-intuitive errors, since the attributes on `types.ModuleType` use PEP-604 unions in their type annotations.

## Test Plan

- `cargo test -p red_knot_python_semantic`
- `cargo bench -p ruff_benchmark -- red_knot`


---

_Label `red-knot` added by @AlexWaygood on 2024-10-24 12:38_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-24 12:38_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-24 12:38_

---

_Comment by @github-actions[bot] on 2024-10-24 12:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3477 on 2024-10-24 18:10_

I think actually supporting this would take only like three lines here, plus some tests 😆 maybe a good task for @charliermarsh 

---

_@carljm approved on 2024-10-24 18:10_

Looks great, thank you!

---

_@MichaReiser reviewed on 2024-10-25 07:30_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:224 on 2024-10-25 07:30_

Hmm, from just reading this comment, it isn't clear to me why we are only filtering `Todo` types? Why aren't we filtering out `Unknown` types? I thought the two are equivalent except that they indicate a different source

Would it be possible to remove the `retain` call and instead avoid adding the types to `conflicting` on line 214

---

_@AlexWaygood reviewed on 2024-10-25 17:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:224 on 2024-10-25 17:21_

That's a fair point. I think we should never infer a variable that has a declaration as being of type `Unknown` unless they had an invalid type annotation of some kind, in which case we should have already emitted a diagnostic. So probably we should also filter out `Unknown` types here, to avoid cascading errors from one mistake.

> Would it be possible to remove the `retain` call and instead avoid adding the types to `conflicting` on line 214

makes sense

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:224 on 2024-10-25 17:41_

Ah, no. We do need to keep `Unknown` in `conflicting`, or we won't complain about this case:

```py
if flag:
    x: int
x = 1  # error: [conflicting-declarations] "Conflicting declared types for `x`: Unknown, int"
```

Currently every undeclared symbol in our model is marked as being "implicitly declared as `Unknown`"; this is how we detect that a symbol is only annotated in one branch.

I also don't think it's _so_ awful if a user gets two errors rather than one when they've got an invalid annotation in their code

---

_@AlexWaygood reviewed on 2024-10-25 17:41_

---

_Merged by @AlexWaygood on 2024-10-25 18:21_

---

_Closed by @AlexWaygood on 2024-10-25 18:21_

---

_Branch deleted on 2024-10-25 18:21_

---

_@charliermarsh reviewed on 2024-10-28 12:44_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:3477 on 2024-10-28 12:44_

I can do it!

---
