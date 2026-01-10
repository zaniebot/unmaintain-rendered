```yaml
number: 12479
title: Refactor NPY201
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/refactor-numpy
created_at: 2024-07-23T17:17:50Z
updated_at: 2024-07-23T17:31:46Z
url: https://github.com/astral-sh/ruff/pull/12479
synced_at: 2026-01-10T21:47:02Z
```

# Refactor NPY201

---

_Pull request opened by @AlexWaygood on 2024-07-23 17:17_

## Summary

This PR refactors the NPY201 rule to reduce nesting and indentation in the code. It's just a pure refactor; there's no substantive changes here. I'm opening this because it makes it easier to rework the code slightly in order to address #12476. It's a separate PR because it changes the indentation for a very large range of lines, creating a pretty big diff.

## Test Plan

`cargo test -p ruff_linter --lib`

---

_Label `internal` added by @AlexWaygood on 2024-07-23 17:17_

---

_Comment by @MichaReiser on 2024-07-23 17:21_

Lol. Exactly the same amount of added and removed lines. I'll take a look tomorrow unless someone else beats me to it

---

_@charliermarsh approved on 2024-07-23 17:22_

---

_Comment by @codspeed-hq[bot] on 2024-07-23 17:23_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex/refactor-numpy)

### Merging #12479 will **improve performances by 4.26%**

<sub>Comparing <code>alex/refactor-numpy</code> (ef0c7b4) with <code>main</code> (3af6ccb)</sub>



### Summary

`⚡ 1` improvements
`✅ 32` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `alex/refactor-numpy` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `lexer[numpy/globals.py]` | 30.4 µs | 29.2 µs | +4.26% |


---

_Merged by @AlexWaygood on 2024-07-23 17:24_

---

_Closed by @AlexWaygood on 2024-07-23 17:24_

---

_Branch deleted on 2024-07-23 17:24_

---

_Comment by @github-actions[bot] on 2024-07-23 17:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
