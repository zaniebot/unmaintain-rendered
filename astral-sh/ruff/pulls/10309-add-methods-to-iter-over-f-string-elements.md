```yaml
number: 10309
title: Add methods to iter over f-string elements
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/f-string-iters
created_at: 2024-03-09T11:12:49Z
updated_at: 2024-03-18T11:14:26Z
url: https://github.com/astral-sh/ruff/pull/10309
synced_at: 2026-01-12T15:55:31Z
```

# Add methods to iter over f-string elements

---

_@dhruvmanila_

## Summary

This PR adds methods on `FString` to iterate over the two different kind of elements it can have - literals and expressions. This is similar to the methods we have on `ExprFString`.




---

_Label `internal` added by @dhruvmanila on 2024-03-09 11:12_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-09 11:12_

---

_@AlexWaygood approved on 2024-03-09 11:26_

---

_@AlexWaygood reviewed on 2024-03-09 11:27_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1259 on 2024-03-09 11:27_

could possibly improve the grammar here slightly:

```suggestion
    /// Returns an iterator over all the [`FStringLiteralElement`] nodes contained in this f-string.
    pub fn literals(&self) -> impl Iterator<Item = &FStringLiteralElement> {
        self.elements
            .iter()
            .filter_map(|element| element.as_literal())
    }

    /// Returns an iterator over all the [`FStringExpressionElement`] nodes contained in this f-string.
```

---

_Comment by @github-actions[bot] on 2024-03-09 11:31_

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

_Merged by @dhruvmanila on 2024-03-13 08:46_

---

_Closed by @dhruvmanila on 2024-03-13 08:46_

---

_Branch deleted on 2024-03-13 08:46_

---

_@MichaReiser reviewed on 2024-03-18 11:14_

That's nice :)

---
