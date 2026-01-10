```yaml
number: 10327
title: "Remove `FStringElement::Invalid`, improve parsing logic"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/f-string-parsing
created_at: 2024-03-11T06:20:57Z
updated_at: 2024-03-12T08:23:37Z
url: https://github.com/astral-sh/ruff/pull/10327
synced_at: 2026-01-10T22:47:01Z
```

# Remove `FStringElement::Invalid`, improve parsing logic

---

_Pull request opened by @dhruvmanila on 2024-03-11 06:20_

## Summary

This PR does the following around f-string parsing:
1. Removes the `FStringElement::Invalid` variant
2. Move the parsing of f-string elements to use list parsing logic
3. Add error recovery for f-string elements

## Test plan

- [x] All tests pass
- [x] No performance regression 

---

_Label `parser` added by @dhruvmanila on 2024-03-11 06:20_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-11 06:20_

---

_@dhruvmanila reviewed on 2024-03-12 08:18_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string.rs`:236 on 2024-03-12 08:18_

To be explicit that this only parses the `FStringMiddle` token which gives us the `FStringLiteralElement` AST node.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1037 on 2024-03-12 08:22_

Conversion flag is derived from the `Name` token as per the grammar:

```
fstring_conversion:
    | "!" NAME 
```

This is also what our old parser used. We will still raise the correct error (`InvalidConversionFlag`) even if it isn't a `NAME` token.

---

_@dhruvmanila reviewed on 2024-03-12 08:22_

---

_Merged by @dhruvmanila on 2024-03-12 08:23_

---

_Closed by @dhruvmanila on 2024-03-12 08:23_

---

_Branch deleted on 2024-03-12 08:23_

---
