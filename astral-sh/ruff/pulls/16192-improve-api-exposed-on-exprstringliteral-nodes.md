```yaml
number: 16192
title: "Improve API exposed on `ExprStringLiteral` nodes"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/contents-range
created_at: 2025-02-16T18:19:32Z
updated_at: 2025-02-17T08:00:34Z
url: https://github.com/astral-sh/ruff/pull/16192
synced_at: 2026-01-10T19:57:22Z
```

# Improve API exposed on `ExprStringLiteral` nodes

---

_Pull request opened by @AlexWaygood on 2025-02-16 18:19_

## Summary

This PR makes the following changes:
- It adjusts various callsites to use the new `ast::StringLiteral::contents_range()` method that was introduced in https://github.com/astral-sh/ruff/pull/16183. This is less verbose and more type-safe than using the `ast::str::raw_contents()` helper function.
- It adds a new `ast::ExprStringLiteral::as_unconcatenated_literal()` helper method, and adjusts various callsites to use it. This addresses @MichaReiser's review comment at https://github.com/astral-sh/ruff/pull/16183#discussion_r1957334365. There is no functional change here, but it helps readability to make it clearer that we're differentiating between implicitly concatenated strings and unconcatenated strings at various points.
- It renames the `StringLiteralValue::flags()` method to `StringLiteralFlags::first_literal_flags()`. If you're dealing with an implicitly concatenated string `string_node`, `string_node.value.flags().closer_len()` could give an incorrect result; this renaming makes it clearer that the `StringLiteralFlags` instance returned by the method is only guaranteed to give accurate information for the first `StringLiteral` contained in the `ExprStringLiteral` node.
- It deletes the unused `BytesLiteralValue::flags()` method. This seems prone to misuse in the same way as `StringLiteralValue::flags()`: if it's an implicitly concatenated bytestring, the `BytesLiteralFlags` instance returned by the method would only give accurate information for the first `BytesLiteral` in the bytestring.

## Test Plan

`cargo test`


---

_Label `internal` added by @AlexWaygood on 2025-02-16 18:19_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-16 18:19_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-16 18:19_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-16 18:19_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-02-16 18:19_

---

_Comment by @github-actions[bot] on 2025-02-16 18:34_

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

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1293 on 2025-02-17 07:43_

Nit: I wonder if it should be `as_only_literal` to align with `first_literal` but I'm fine with either.

---

_@MichaReiser approved on 2025-02-17 07:43_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1293 on 2025-02-17 07:55_

I think I'd prefer to use `as_single_literal` / `as_only_literal` instead as reading `unconcatenated` and matching against `Single` confused me at the first glance

---

_@dhruvmanila approved on 2025-02-17 07:55_

---

_@AlexWaygood reviewed on 2025-02-17 07:58_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1293 on 2025-02-17 07:58_

I _weakly_ prefer the current name as I think it makes it clearer that the method is used to differentiate between strings that are implicitly concatenated and ones that aren't

---

_Merged by @AlexWaygood on 2025-02-17 07:58_

---

_Closed by @AlexWaygood on 2025-02-17 07:58_

---

_Branch deleted on 2025-02-17 07:58_

---

_@AlexWaygood reviewed on 2025-02-17 08:00_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1293 on 2025-02-17 08:00_

Oh shoot, I didn't see Dhruv's message until after I merged — sorry. I'll rename it as a followup, since you both dislike the current name!

---
