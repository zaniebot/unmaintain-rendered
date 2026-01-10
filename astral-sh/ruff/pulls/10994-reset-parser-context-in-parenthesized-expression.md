```yaml
number: 10994
title: Reset parser context in parenthesized expression
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/reset-ctx-in-parenthesized
created_at: 2024-04-17T11:38:07Z
updated_at: 2024-04-17T13:04:01Z
url: https://github.com/astral-sh/ruff/pull/10994
synced_at: 2026-01-10T22:37:01Z
```

# Reset parser context in parenthesized expression

---

_Pull request opened by @dhruvmanila on 2024-04-17 11:38_

## Summary

This PR fixes the bug where the parser would fail to parse the following code:

```python
for (x in y)[0] in z: ...
```

This is because when parsing the target of a `for` statement, the context flag for the parser is set to `FOR_TARGET` and this is used by binary expression parsing to stop the expression parsing when it sees the `in` keyword. But, in parenthesized context, this is allowed.

The solution implemented in this PR is to reset the parser context when parsing a parenthesized expression and set it back to the original context later.

An alternative solution would be to introduce [`ExpressionContext`](https://github.com/biomejs/biome/blob/838ccb442370e72197e625a7d0e4456e6e77e498/crates/biome_js_parser/src/syntax/expr.rs#L42-L43) but that would require a lot of updates as almost all of expression parsing function would need to be updated to take this context. A benefit of this would be that both `AllowStarredExpression` and `AllowNamedExpression` can be merged in the context and everything around `ParserCtxFlags` can be removed. It could possibly also simplify certain parsing functions.

## Test Plan

Add test cases and verified the snapshots.


---

_Label `bug` added by @dhruvmanila on 2024-04-17 11:38_

---

_Label `parser` added by @dhruvmanila on 2024-04-17 11:38_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-17 11:38_

---

_Comment by @codspeed-hq[bot] on 2024-04-17 11:43_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/reset-ctx-in-parenthesized)

### Merging #10994 will **not alter performance**

<sub>Comparing <code>dhruv/reset-ctx-in-parenthesized</code> (c4a2752) with <code>dhruv/parser</code> (c30057a)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1761 on 2024-04-17 12:00_

Nit: I think I would explicitly unset the `FOR_TARGET` flag to ensure the code doesn't reset other flags when we add new flags to the context.

---

_@MichaReiser approved on 2024-04-17 12:01_

---

_Comment by @github-actions[bot] on 2024-04-17 12:10_

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

_Merged by @dhruvmanila on 2024-04-17 12:45_

---

_Closed by @dhruvmanila on 2024-04-17 12:45_

---

_Branch deleted on 2024-04-17 12:45_

---
