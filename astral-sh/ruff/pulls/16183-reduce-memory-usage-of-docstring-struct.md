```yaml
number: 16183
title: "Reduce memory usage of `Docstring` struct"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - docstring
assignees: []
merged: true
base: main
head: alex/smaller-docstring
created_at: 2025-02-16T13:43:53Z
updated_at: 2025-02-16T15:51:53Z
url: https://github.com/astral-sh/ruff/pull/16183
synced_at: 2026-01-10T19:57:22Z
```

# Reduce memory usage of `Docstring` struct

---

_Pull request opened by @AlexWaygood on 2025-02-16 13:43_

## Summary

I noticed that our `Docstring` struct, that we use for querying information on docstrings in various linter rules, eagerly stores a lot of information in the struct that could probably be lazily computed in methods on the struct. This PR reworks the API to store less information on the struct itself, reducing memory usage.

In addition to this, it changes the struct to store an `ast::StringLiteral` node rather than an `ast::ExprStringLiteral` node. This:
- Allows us to enforce in the type system the existing invariant that we never consider an implicitly concatenated string to be a valid `Docstring` definition
- Allows us to use the `ast::StringLiteral::flags` attribute to query information directly from the underlying AST node, removing the need for quite a few `.unwrap()` calls (accompanied by `// SAFETY` comments) across our docstring-related rules

## Test Plan

- `cargo test`
- There should be 0 ecosystem hits, since this is a pure refactor


---

_Label `internal` added by @AlexWaygood on 2025-02-16 13:43_

---

_Label `docstring` added by @AlexWaygood on 2025-02-16 13:43_

---

_Comment by @github-actions[bot] on 2025-02-16 13:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @AlexWaygood on 2025-02-16 13:52_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/definitions.rs`:187 on 2025-02-16 14:03_

I found the old explicit implicit concatenated string helped with readability. I wonder if we should add a method for non-implicit concatenated strings to `string_literal`. 

Coming up with a good name is hard: `let Some(sole_string_part) = string_literal.as_non_implicit()` 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/docstrings/mod.rs`:59 on 2025-02-16 14:05_

Nit: `prefix_str` in case we ever want to expose `flags.prefix`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/docstrings/mod.rs`:99 on 2025-02-16 14:05_

Nit: It would be nice if we could avoid copying this computation. Consider adding a `content_range` to `StringLiteral` similar to https://github.com/astral-sh/ruff/blob/e1781485257783d4b714a9a33c6f8fed4f1477fb/crates/ruff_python_ast/src/expression.rs#L221-L228

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/blank_before_after_class.rs`:200 on 2025-02-16 14:07_

Nit: Consider a `indentation_len` method? Considering that most call sites aren't interested in the `indentation` itself (avoids going from range to string and back to range again.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs`:182 on 2025-02-16 14:08_

Nit: Consider storing the `indentation` (and maybe calling the method `to_indentation` because computing the line start isn't free. 

---

_@MichaReiser approved on 2025-02-16 14:09_

Nice, it helps clean up the code a lot. I only have a few nit comments.

---

_@AlexWaygood reviewed on 2025-02-16 14:47_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/docstrings/mod.rs`:99 on 2025-02-16 14:47_

Hehe. I was planning this for a followup, since there are a number of other places where this method would be useful ;) But I'll add it in this PR, and switch other places in the codebase to this new method as the followup!

---

_@AlexWaygood reviewed on 2025-02-16 15:18_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/analyze/definitions.rs`:187 on 2025-02-16 15:18_

Yeah, good point about readability. I will defer this to another PR, though, as I think almost every current usage of the `StringLiteralValue::as_slice()` method would probably switch to using this new API -- and I agree that what we would call it isn't necessarily obvious. It seems worthy of considering as its own thing.

For now I'll just add a comment here.

---

_Merged by @AlexWaygood on 2025-02-16 15:23_

---

_Closed by @AlexWaygood on 2025-02-16 15:23_

---

_Branch deleted on 2025-02-16 15:51_

---
