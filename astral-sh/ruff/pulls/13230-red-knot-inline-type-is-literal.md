```yaml
number: 13230
title: "[red-knot] Inline `Type::is_literal`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/is-literal
created_at: 2024-09-03T13:57:12Z
updated_at: 2024-09-03T14:11:18Z
url: https://github.com/astral-sh/ruff/pull/13230
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] Inline `Type::is_literal`

---

_@AlexWaygood_

The name of this method is somewhat confusing currently, as I would expect it to return `true` for `LiteralString`, but it doesn't (and `LiteralString` is displayed differently, so it wouldn't suit the method's current use case for it to include `LiteralString`). The method is only used in one location currently, so let's just inline it at that location.

---

_Label `red-knot` added by @AlexWaygood on 2024-09-03 13:57_

---

_Review requested from @carljm by @AlexWaygood on 2024-09-03 13:57_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-03 13:57_

---

_@MichaReiser approved on 2024-09-03 14:00_

Thanks

---

_Merged by @AlexWaygood on 2024-09-03 14:02_

---

_Closed by @AlexWaygood on 2024-09-03 14:02_

---

_Branch deleted on 2024-09-03 14:02_

---

_Comment by @codspeed-hq[bot] on 2024-09-03 14:02_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex/is-literal)

### Merging #13230 will **improve performances by 4.66%**

<sub>Comparing <code>alex/is-literal</code> (d9fd5d1) with <code>main</code> (50c8ee5)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `alex/is-literal` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 1.9 ms | 1.8 ms | +4.66% |


---

_Comment by @github-actions[bot] on 2024-09-03 14:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
