```yaml
number: 10385
title: Use string flags to mark it as invalid
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/invalid-string
created_at: 2024-03-13T14:57:23Z
updated_at: 2024-03-14T03:28:01Z
url: https://github.com/astral-sh/ruff/pull/10385
synced_at: 2026-01-10T22:47:01Z
```

# Use string flags to mark it as invalid

---

_Pull request opened by @dhruvmanila on 2024-03-13 14:57_

## Summary

This PR updates the string flags to include an `Invalid` variant for any invalid string nodes as deemed by the parser. This is to avoid dropping the nodes and instead just mark it as invalid. The nodes will be empty for now but we can discuss on whether to keep the raw source text or not. It's not really required because the range can be used to get the same.

It also adds a new `handle_implicitly_concatenated_strings` method which is similar to existing `concatenated_strings` function. The reason to have a separate method is to avoid dropping all strings if there's an error. The error being that it's concatenating bytes and non-bytes literal. Now, we need to decide which strings to retain. Currently, I've kept it simple to retain bytes literal _only_ if all of them are bytes otherwise we'll have string / f-string with invalid nodes instead of bytes literal.

This removes the need for having a `StringType::Invalid` variant.

## Test Plan

- [x] Existing test cases pass
- [x] No performance regression


---

_Label `parser` added by @dhruvmanila on 2024-03-13 14:57_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:974 on 2024-03-13 15:01_

```suggestion
/// An `FStringLiteralElement` with an empty `value` is an invalid f-string element.
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1527 on 2024-03-13 15:02_

To avoid this sounding strangely judgemental:

```suggestion
        /// The string was deemed invalid by the parser.
```

---

_Comment by @github-actions[bot] on 2024-03-13 15:16_

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

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1878 on 2024-03-13 15:17_

```suggestion
        /// The bytestring was deemed invalid by the parser.
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1561 on 2024-03-13 15:19_

Could possibly call this method `marked_as_invalid`? (Same for the one on `BytesLiteralFlags`)

---

_@AlexWaygood reviewed on 2024-03-13 15:20_

Nice — a few suggestions on docs and naming

(sorry for reviewing a draft PR!)

---

_@dhruvmanila reviewed on 2024-03-13 16:32_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:974 on 2024-03-13 16:32_

Oops, thanks!

---

_@dhruvmanila reviewed on 2024-03-13 16:36_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:927 on 2024-03-13 16:36_

Somewhat torn on whether we should keep the raw source text in the node as it's not a valid information and any downstream tool can get the same through the range.

---

_Marked ready for review by @dhruvmanila on 2024-03-14 02:56_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-14 02:56_

---

_@dhruvmanila reviewed on 2024-03-14 02:57_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:974 on 2024-03-14 02:57_

This is fine because there can never be an empty `FStringLiteralElement` for any valid f-string including the ones which only contains expressions. This is because the lexer doesn't emit any `FStringMiddle` token according to the PEP.

---

_Merged by @dhruvmanila on 2024-03-14 03:28_

---

_Closed by @dhruvmanila on 2024-03-14 03:28_

---

_Branch deleted on 2024-03-14 03:28_

---
