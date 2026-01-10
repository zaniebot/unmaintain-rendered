```yaml
number: 10691
title: "Implement `as_str` & `Display` for all operator enums"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/operator-as-str
created_at: 2024-04-01T05:50:52Z
updated_at: 2024-04-02T10:45:34Z
url: https://github.com/astral-sh/ruff/pull/10691
synced_at: 2026-01-10T22:47:02Z
```

# Implement `as_str` & `Display` for all operator enums

---

_Pull request opened by @dhruvmanila on 2024-04-01 05:50_

## Summary

This PR adds the `as_str` implementation for all the operator methods. It already exists for `CmpOp` which is being [used in the linter](https://github.com/astral-sh/ruff/blob/ffcd77860c316bfe5709551389eb52e5feb702f6/crates/ruff_linter/src/rules/flake8_simplify/rules/key_in_dict.rs#L117) and it makes sense to implement it for the rest as well. This will also be utilized in error messages for the new parser.

**Question:** Is it ok to implement the `Display` trait for these enums? I think it's fine. Edit: I've implemented them.


---

_Label `internal` added by @dhruvmanila on 2024-04-01 05:50_

---

_Renamed from "Add `as_str` to all operator enums" to "Implement `as_str` & `Display` for all operator enums" by @dhruvmanila on 2024-04-01 05:52_

---

_Comment by @github-actions[bot] on 2024-04-01 06:44_

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

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:2739 on 2024-04-01 11:22_

Nit: any reason not to make this a `const fn`? (Same nit for `Operator::as_str`, `UnaryOp::as_str`, and `CmpOp::as_str`)

```suggestion
    pub const fn as_str(&self) -> &'static str {
```

---

_@AlexWaygood approved on 2024-04-01 11:22_

---

_@MichaReiser approved on 2024-04-02 07:39_

---

_@dhruvmanila reviewed on 2024-04-02 10:24_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:2739 on 2024-04-02 10:24_

No reason but thanks for pointing it out.

---

_Merged by @dhruvmanila on 2024-04-02 10:34_

---

_Closed by @dhruvmanila on 2024-04-02 10:34_

---

_Branch deleted on 2024-04-02 10:34_

---
