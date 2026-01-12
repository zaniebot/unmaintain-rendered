```yaml
number: 10383
title: Unify enums used for internal representation of quoting style
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: simplify-quote-enums
created_at: 2024-03-13T14:21:50Z
updated_at: 2024-03-13T23:56:53Z
url: https://github.com/astral-sh/ruff/pull/10383
synced_at: 2026-01-12T15:55:32Z
```

# Unify enums used for internal representation of quoting style

---

_@AlexWaygood_

## Summary

Currently we have four enums across the codebase for representing the fact that Python strings can either use single quotes (`'`) or double quotes (`"`):

https://github.com/astral-sh/ruff/blob/c269c1a7063bb27e75dc81c2a598eab13aa8b754/crates/ruff_python_ast/src/str.rs#L6-L13

https://github.com/astral-sh/ruff/blob/c269c1a7063bb27e75dc81c2a598eab13aa8b754/crates/ruff_python_formatter/src/string/mod.rs#L234-L242

https://github.com/astral-sh/ruff/blob/c269c1a7063bb27e75dc81c2a598eab13aa8b754/crates/ruff_python_codegen/src/stylist.rs#L97-L103

https://github.com/astral-sh/ruff/blob/c269c1a7063bb27e75dc81c2a598eab13aa8b754/crates/ruff_python_literal/src/escape.rs#L1-L5

This means we have a lot of `impl From<QuoteEnum> for IdenticalOtherQuoteEnum` methods, and it's generally somewhat confusing to keep track of the subtle differences between them. This PR unfies them into one enum in `ruff_python_ast::str::QuoteStyle`.

Using `ruff_python_ast::str::QuoteStyle` in the formatter would have created an awkward name clash with `ruff_python_formatter::options::QuoteStyle`, so I renamed `ruff_python_formatter::options::QuoteStyle` to `ruff_python_formatter::options::QuotePreference`. This seemed to me to more accurately reflect what that enum was for, but an alternative here would have been to rename `ruff_python_ast::str::QuoteStyle`. (Yet another alternative would be to always use the fully qualified name of `ruff_python_ast::str:QuoteStyle` in the formatter crate, but that felt too cumbersome to me.)

## Test Plan

`cargo test`

---

_Label `internal` added by @AlexWaygood on 2024-03-13 14:21_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-03-13 14:21_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-03-13 14:40_

---

_Comment by @github-actions[bot] on 2024-03-13 14:50_

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

_Comment by @dhruvmanila on 2024-03-13 16:27_

> Using `ruff_python_ast::str::QuoteStyle` in the formatter would have created an awkward name clash with `ruff_python_ast::options::QuoteStyle`, so I renamed `ruff_python_ast::options::QuoteStyle` to `ruff_python_ast::options::QuotePreference`.

Another option would be to use `ruff_python_ast::str::Quote` and keep the `ruff_python_formatter::options::QuoteStyle`. There's a chance that I might get confused with `QuoteStyle`'s value coming from the `quote-style` config option 😅. This would mean that the enum usage would be `Quote::Single`, `Quote::Double`, `quote.is_single`, `quote.is_double`, etc. What do you think? @AlexWaygood 

---

_Comment by @AlexWaygood on 2024-03-13 16:30_

> here's a chance that I might get confused with `QuoteStyle`'s value coming from the `quote-style` config option 😅. This would mean that the enum usage would be `Quote::Single`, `Quote::Double`, `quote.is_single`, `quote.is_double`, etc. What do you think? @AlexWaygood

Ahh, that makes sense... I should have checked what the config option was called, sorry!

I'll change the formatter enum's name back, and change the `ruff_python_ast` enum to `Quote` 👍

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/mod.rs`:235 on 2024-03-13 16:58_

Oh nice, I like this change as the `current_expressions` start with _innermost_ f-string in case of nested f-strings.

---

_Review comment by @dhruvmanila on `crates/ruff_python_literal/Cargo.toml`:26 on 2024-03-13 17:02_

nit: we tend to have local dependencies first, then third party dependencies ;)

---

_@dhruvmanila approved on 2024-03-13 17:03_

This is great! So many lines red lines :)

---

_Merged by @AlexWaygood on 2024-03-13 17:19_

---

_Closed by @AlexWaygood on 2024-03-13 17:19_

---

_Branch deleted on 2024-03-13 17:30_

---
