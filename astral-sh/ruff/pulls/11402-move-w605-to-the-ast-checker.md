```yaml
number: 11402
title: "Move `W605` to the AST checker"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/move-w605-to-ast
created_at: 2024-05-13T09:29:19Z
updated_at: 2024-05-13T16:24:10Z
url: https://github.com/astral-sh/ruff/pull/11402
synced_at: 2026-01-12T15:55:37Z
```

# Move `W605` to the AST checker

---

_@dhruvmanila_

## Summary

This PR moves the `W605` rule to the AST checker.

This is part of #11401

## Test Plan

`cargo test`


---

_Label `internal` added by @dhruvmanila on 2024-05-13 09:29_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-13 10:17_

---

_Comment by @github-actions[bot] on 2024-05-13 12:57_

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

_@AlexWaygood reviewed on 2024-05-13 13:08_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/expression.rs`:457 on 2024-05-13 13:08_

Hmm... Wouldn't it be more extensible if we had a kind() (or `flags()`, if we do the rename you proposed in your other PR) method that returned `AnyStringKind`? Then linter rules could use that method to query whether the string part is raw, but they could also use it for querying other information about the string as well. Something like

```rs
impl StringLikePart<'_> {
    pub fn kind(self) -> AnyStringKind {
        match self {
            StringLikePart::String(string) => AnyStringKind::from(string.flags),
            StringLikePart::Bytes(bytes) => AnyStringKind::from(bytes.flags),
            StringLikePart::FString(f_string) => AnyStringKind::from(f_string.flags),
        }
    }
}
```

---

_@dhruvmanila reviewed on 2024-05-13 13:15_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/expression.rs`:457 on 2024-05-13 13:15_

Yeah, good point. It makes sense to expose the `AnyStringKind` instead. I'll update it.

---

_@AlexWaygood approved on 2024-05-13 13:59_

---

_Merged by @dhruvmanila on 2024-05-13 16:13_

---

_Closed by @dhruvmanila on 2024-05-13 16:13_

---

_Branch deleted on 2024-05-13 16:13_

---
