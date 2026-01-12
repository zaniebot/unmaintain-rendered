```yaml
number: 11199
title: Add basic docs for the parser crate
type: pull_request
state: merged
author: dhruvmanila
labels:
  - documentation
assignees: []
merged: true
base: main
head: dhruv/parser-docs
created_at: 2024-04-29T11:00:26Z
updated_at: 2024-04-29T17:18:58Z
url: https://github.com/astral-sh/ruff/pull/11199
synced_at: 2026-01-12T15:55:37Z
```

# Add basic docs for the parser crate

---

_@dhruvmanila_

## Summary

This PR adds a basic README for the `ruff_python_parser` crate and updates the CONTRIBUTING docs with the fuzzer and benchmark section.

Additionally, it also updates some inline documentation within the parser crate and splits the `parse_program` function into `parse_single_expression` and `parse_module` which will be called by matching against the `Mode`.

This PR doesn't go into too much internal detail around the parser logic due to the following reasons:
1. Where should the docs go? Should it be as a module docs in `lib.rs` or in README?
2. The parser is still evolving and could include a lot of refactors with the future work (feedback loop and improved error recovery and resilience)


---

_Label `documentation` added by @dhruvmanila on 2024-04-29 11:00_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-29 11:00_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/lib.rs`:162 on 2024-04-29 11:03_

```suggestion
/// Parse a full Python program into a [`Suite`].
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/parser/mod.rs`:151 on 2024-04-29 11:05_

```suggestion
    /// After parsing a single expression, an error is reported and all remaining tokens are
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/parser/mod.rs`:157 on 2024-04-29 11:05_

```suggestion
        // All remaining newlines are actually going to be non-logical newlines.
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/token.rs`:357 on 2024-04-29 11:06_

```suggestion
/// This is a lightweight representation of [`Tok`] which doesn't contain any information
```

---

_@AlexWaygood approved on 2024-04-29 11:06_

---

_Comment by @dhruvmanila on 2024-04-29 11:08_

Hmm, I think I can just use the following in `lib.rs`:
```rs
#![doc = include_str!("../README.md")]
```

---

_Comment by @github-actions[bot] on 2024-04-29 11:25_

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

_@MichaReiser approved on 2024-04-29 13:59_

---

_Merged by @dhruvmanila on 2024-04-29 17:08_

---

_Closed by @dhruvmanila on 2024-04-29 17:08_

---

_Branch deleted on 2024-04-29 17:08_

---
