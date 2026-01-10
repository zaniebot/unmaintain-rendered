```yaml
number: 9953
title: "Use non-parenthesized range for `DebugText`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: dhruv/fix-debug-text
created_at: 2024-02-12T15:15:35Z
updated_at: 2024-02-12T17:30:04Z
url: https://github.com/astral-sh/ruff/pull/9953
synced_at: 2026-01-10T22:57:09Z
```

# Use non-parenthesized range for `DebugText`

---

_Pull request opened by @dhruvmanila on 2024-02-12 15:15_

## Summary

This PR fixes the `DebugText` implementation to use the expression range instead of the parenthesized range.

Taking the following code snippet as an example:
```python
x = 1
print(f"{  ( x  ) = }")
```

The output of running it would be:
```
  ( x  ) = 1
```

Notice that the whitespace between the parentheses and the expression is preserved as is.

Currently, we don't preserve this information in the AST which defeats the purpose of `DebugText` as the main purpose of the struct is to preserve whitespaces _around_ the expression.

This is also problematic when generating the code from the AST node as then the generator has no information about the parentheses the whitespaces between them and the expression which would lead to the removal of the parentheses in the generated code.

I noticed this while working on the f-string formatting where the debug text would be used to preserve the text surrounding the expression in the presence of debug expression. The parentheses were being dropped then which made me realize that the problem is instead in the parser.

## Test Plan

1. Add a test case for the parser
2. Add a test case for the generator


---

_Label `bug` added by @dhruvmanila on 2024-02-12 15:15_

---

_Label `parser` added by @dhruvmanila on 2024-02-12 15:15_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-02-12 15:15_

---

_Comment by @MichaReiser on 2024-02-12 15:20_

Can you add a short explanation why it should use the expression range instead of the parenthesized range?

---

_Comment by @github-actions[bot] on 2024-02-12 15:33_

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

_@MichaReiser approved on 2024-02-12 16:48_

Thanks for the explanation. It helped me understand the change and makes this a very easy review :) 

---

_Renamed from "Update `DebugText` with non-parenthesized range" to "Use non-parenthesized range for `DebugText`" by @dhruvmanila on 2024-02-12 17:28_

---

_Merged by @dhruvmanila on 2024-02-12 17:30_

---

_Closed by @dhruvmanila on 2024-02-12 17:30_

---

_Branch deleted on 2024-02-12 17:30_

---
