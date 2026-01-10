```yaml
number: 13091
title: "Optimize some `SemanticModel` methods"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: optimize-current-exprs
created_at: 2024-08-25T16:46:05Z
updated_at: 2024-09-02T09:03:54Z
url: https://github.com/astral-sh/ruff/pull/13091
synced_at: 2026-01-10T21:38:32Z
```

# Optimize some `SemanticModel` methods

---

_Pull request opened by @AlexWaygood on 2024-08-25 16:46_

_No description provided._

---

_Comment by @github-actions[bot] on 2024-08-25 17:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-08-25 19:37_

The premise of this PR is that in Python, a statement can contain a statement, an expression can contain an expression, and a statement can contain an expression -- but an expression _cannot_ contain a statement. As such, for various methods that iterate through all "parent expression nodes" in the AST, we can stop as soon as we hit a statement node, since we know that there can be no parent expression nodes of that statement node.

It seems like this has no impact on the Codspeed microbenchmarks, but it still feels to me like the "right" thing to do regardless, if you consider Python's semantics... anyway, I'll let others judge whether it's worth it!

---

_Marked ready for review by @AlexWaygood on 2024-08-25 19:37_

---

_Comment by @codspeed-hq[bot] on 2024-08-26 00:51_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/optimize-current-exprs)

### Merging #13091 will **improve performances by 5.32%**

<sub>Comparing <code>optimize-current-exprs</code> (2da0cb6) with <code>main</code> (f50f873)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `optimize-current-exprs` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 767.8 µs | 729 µs | +5.32% |


---

_Label `internal` added by @MichaReiser on 2024-09-02 08:58_

---

_@MichaReiser approved on 2024-09-02 09:01_

---

_Merged by @AlexWaygood on 2024-09-02 09:03_

---

_Closed by @AlexWaygood on 2024-09-02 09:03_

---

_Branch deleted on 2024-09-02 09:03_

---
