```yaml
number: 11405
title: "Rename `AnyStringKind` -> `AnyStringFlags`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/any-string-flags
created_at: 2024-05-13T12:40:49Z
updated_at: 2024-05-13T13:28:35Z
url: https://github.com/astral-sh/ruff/pull/11405
synced_at: 2026-01-10T22:05:26Z
```

# Rename `AnyStringKind` -> `AnyStringFlags`

---

_Pull request opened by @dhruvmanila on 2024-05-13 12:40_

## Summary

This PR renames `AnyStringKind` to `AnyStringFlags` and `AnyStringFlags` to `AnyStringFlagsInner`.

The main motivation is to have consistent usage of "kind" and "flags". For each string kind, it's "flags" like `StringLiteralFlags`, `BytesLiteralFlags`, and `FStringFlags` but it was `AnyStringKind` for the "any" variant.


---

_Label `internal` added by @dhruvmanila on 2024-05-13 12:40_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-13 12:40_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-05-13 12:40_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/string.rs`:47 on 2024-05-13 12:50_

```suggestion
    /// Flags that can be used to query information about the string.
```

---

_@AlexWaygood approved on 2024-05-13 12:53_

I guess "flags" isn't an ideal name for any of these structs, since they all use newtypes to abstract away the fact that the underlying data is stored as a bitflag. But this definitely makes things more consistent — thanks!

---

_Comment by @AlexWaygood on 2024-05-13 12:53_

(You have a lot of snapshots to update ;)

---

_@dhruvmanila reviewed on 2024-05-13 13:05_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__lexer__tests__empty_fstrings.snap`:25 on 2024-05-13 13:05_

@AlexWaygood Was this suppose to be "AnyStringKind"? If so, I'll rename it to "AnyStringFlags" otherwise I'll rename it to "StringFlags".

---

_@dhruvmanila reviewed on 2024-05-13 13:09_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__lexer__tests__empty_fstrings.snap`:25 on 2024-05-13 13:09_

I've renamed it to "AnyStringFlags".

---

_@AlexWaygood reviewed on 2024-05-13 13:11_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__lexer__tests__empty_fstrings.snap`:25 on 2024-05-13 13:11_

Thanks!

---

_Comment by @dhruvmanila on 2024-05-13 13:11_

> I guess "flags" isn't an ideal name for any of these structs, since they all use newtypes to abstract away the fact that the underlying data is stored as a bitflag. But this definitely makes things more consistent — thanks!

I think "flags" is fine, but maybe not from an encapsulation point of view. Pyright also calls it "flags".

---

_Merged by @dhruvmanila on 2024-05-13 13:18_

---

_Closed by @dhruvmanila on 2024-05-13 13:18_

---

_Branch deleted on 2024-05-13 13:18_

---

_Comment by @github-actions[bot] on 2024-05-13 13:28_

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
