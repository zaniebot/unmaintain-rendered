```yaml
number: 10855
title: "Remove `is_current_token_postfix`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/remove-is-current-postfix
created_at: 2024-04-10T04:17:52Z
updated_at: 2024-04-10T04:36:33Z
url: https://github.com/astral-sh/ruff/pull/10855
synced_at: 2026-01-12T15:55:33Z
```

# Remove `is_current_token_postfix`

---

_@dhruvmanila_

## Summary

This PR removes the `is_current_token_postifx` method and instead directly calls the `parse_postifx_expression`. The condition was required because postfix expression parsing would reset the `is_parenthesized` field on the `ParsedExpr`. Now, we'll make that explicit.


---

_Label `parser` added by @dhruvmanila on 2024-04-10 04:17_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-10 04:17_

---

_Merged by @dhruvmanila on 2024-04-10 04:18_

---

_Closed by @dhruvmanila on 2024-04-10 04:18_

---

_Branch deleted on 2024-04-10 04:18_

---

_Comment by @codspeed-hq[bot] on 2024-04-10 04:23_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/remove-is-current-postfix)

### Merging #10855 will **degrade performances by 5.78%**

<sub>Comparing <code>dhruv/remove-is-current-postfix</code> (a0dc22a) with <code>dhruv/parser</code> (38d649c)</sub>



### Summary

`⚡ 2` improvements
`❌ 1` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/remove-is-current-postfix)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/remove-is-current-postfix` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[large/dataset.py]` | 28.7 ms | 26.7 ms | +7.66% |
| ⚡ | `parser[pydantic/types.py]` | 11.8 ms | 10.9 ms | +8.19% |
| ❌ | `parser[unicode/pypinyin.py]` | 1.6 ms | 1.7 ms | -5.78% |


---

_Comment by @github-actions[bot] on 2024-04-10 04:36_

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
