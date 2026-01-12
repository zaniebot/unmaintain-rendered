```yaml
number: 11335
title: "Improve `ruff_python_semantic::all::extract_all_names()`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - linter
assignees: []
merged: true
base: main
head: improve-all
created_at: 2024-05-08T10:40:05Z
updated_at: 2024-05-08T16:11:43Z
url: https://github.com/astral-sh/ruff/pull/11335
synced_at: 2026-01-12T15:55:37Z
```

# Improve `ruff_python_semantic::all::extract_all_names()`

---

_@AlexWaygood_

## Summary

Following https://github.com/astral-sh/ruff/commit/22639c5a2aabb21b97b1f3b6366e1fe5d300d572, this function now lives in the `ruff_python_semantic` crate. That means we can simplify the function's signature: where previously it required a closure to be passed in, we can now simply pass in a reference to a `SemanticModel` instance. As well as simplifying the signature of the function, this also means we can make the function more accurate: it now recognises `__all__ = builtins.list(["foo", "bar"])` as a valid `__all__` definition, as well as `__all__ = list(["foo", "bar"])`.

## Test Plan

I added a new pyflakes fixture. `cargo test` / `cargo insta review`.


---

_Label `linter` added by @AlexWaygood on 2024-05-08 10:48_

---

_Comment by @github-actions[bot] on 2024-05-08 10:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/all.rs`:74 on 2024-05-08 12:20_

Nit: We could also consider making it a method on Semantic model (I think that's what you recommended initially)

---

_@MichaReiser approved on 2024-05-08 12:20_

---

_Label `linter` removed by @MichaReiser on 2024-05-08 12:21_

---

_Label `internal` added by @MichaReiser on 2024-05-08 12:21_

---

_Comment by @AlexWaygood on 2024-05-08 12:32_

> [MichaReiser](https://github.com/MichaReiser) added internal and removed linter labels

FWIW this does have a _small_ user-visible impact (`__all__ = builtins.list(["foo"])` is now recognised as a valid `__all__` assignment, when it previously was not). But I'm also fine with skipping the changelog.

---

_Label `internal` removed by @MichaReiser on 2024-05-08 12:37_

---

_Label `linter` added by @MichaReiser on 2024-05-08 12:37_

---

_@AlexWaygood reviewed on 2024-05-08 12:37_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/all.rs`:73 on 2024-05-08 12:37_

@MichaReiser how idiomatic/unidiomatic is it to have this `impl` block here rather than in `model.rs`, would you say? I think it _is_ quite nice to have all the functionality related to `__all__` in a separate module, but I'm not sure how common this kind of thing is

---

_@MichaReiser reviewed on 2024-05-08 13:49_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/all.rs`:73 on 2024-05-08 13:49_

It's not unidiomatic to split impl Blocks across multiple files. However, I do try to keep the impl blocks in the same module (e.g. `semantic`, `semantic/all`, `semantic/functions`). 

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-05-08 16:06_

---

_@MichaReiser approved on 2024-05-08 16:08_

---

_Merged by @AlexWaygood on 2024-05-08 16:09_

---

_Closed by @AlexWaygood on 2024-05-08 16:09_

---

_Branch deleted on 2024-05-08 16:09_

---
