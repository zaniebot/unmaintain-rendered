```yaml
number: 10636
title: Add tests for dictionary comprehension
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/test-dict-comp
created_at: 2024-03-27T19:54:37Z
updated_at: 2024-03-29T08:40:32Z
url: https://github.com/astral-sh/ruff/pull/10636
synced_at: 2026-01-12T15:55:32Z
```

# Add tests for dictionary comprehension

---

_@dhruvmanila_

## Summary

This PR adds test cases for dictionary comprehension.


---

_Label `parser` added by @dhruvmanila on 2024-03-27 19:54_

---

_Label `testing` added by @dhruvmanila on 2024-03-27 19:54_

---

_Comment by @codspeed-hq[bot] on 2024-03-28 02:32_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/test-dict-comp)

### Merging #10636 will **not alter performance**

<sub>:warning: No base runs were found</sub>

<sub>Falling back to comparing <code>dhruv/test-dict-comp</code> (00c36b1) with <code>dhruv/parser</code> (80275ac)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_@dhruvmanila reviewed on 2024-03-28 06:59_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/resources/invalid/expressions/dict/double_star_comprehension.py`:9 on 2024-03-28 06:59_

It's more like there's no way to represent `**y` if it's used in a key-value pair. A standalone double star expression is represented by adding `None` to keys vector and the expression to the `value` vector. The `None` in the corresponding position of the key vector signals that it's a double star expression.

---

_Marked ready for review by @dhruvmanila on 2024-03-28 07:00_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-28 07:00_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/resources/invalid/expressions/dict/double_star_comprehension.py`:9 on 2024-03-28 08:53_

I think we could recover here by parsing this as a power op without a left hand side:

```py
{ x: <missing>**y for x, y in data}
```

See https://github.com/microsoft/TypeScript/blob/02bb3108ad625cddcec228846b9ff56cc5398976/src/compiler/parser.ts#L5506-L5569

---

_@MichaReiser approved on 2024-03-28 08:55_

---

_@dhruvmanila reviewed on 2024-03-28 09:33_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/resources/invalid/expressions/dict/double_star_comprehension.py`:9 on 2024-03-28 09:33_

Thanks for the reference. I'll make the change.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/resources/invalid/expressions/dict/double_star_comprehension.py`:9 on 2024-03-28 10:06_

I should have been more explicit. I think that's an improvement we can make as a follow up PR or in the future. I don't see it as a priority (for as long as we track this somewhere)

---

_@MichaReiser reviewed on 2024-03-28 10:06_

---

_Merged by @dhruvmanila on 2024-03-29 08:22_

---

_Closed by @dhruvmanila on 2024-03-29 08:22_

---

_Branch deleted on 2024-03-29 08:22_

---

_Comment by @github-actions[bot] on 2024-03-29 08:40_

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
