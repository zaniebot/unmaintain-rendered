```yaml
number: 12273
title: "Change `File::touch_path` to only take a `SystemPath`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: files-touch-path
created_at: 2024-07-10T12:09:14Z
updated_at: 2024-07-10T12:24:50Z
url: https://github.com/astral-sh/ruff/pull/12273
synced_at: 2026-01-12T15:55:40Z
```

# Change `File::touch_path` to only take a `SystemPath`

---

_@MichaReiser_

## Summary

This PR changes the signature of `File::touch_path` to take a `SystemPath` instead of a `FilePath`, because `VendoredPath` can never change. 

## Test Plan

`cargo test`

<!-- How was it tested? -->


---

_Review requested from @carljm by @MichaReiser on 2024-07-10 12:09_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-10 12:09_

---

_Label `internal` added by @MichaReiser on 2024-07-10 12:09_

---

_@AlexWaygood approved on 2024-07-10 12:11_

---

_Merged by @MichaReiser on 2024-07-10 12:15_

---

_Closed by @MichaReiser on 2024-07-10 12:15_

---

_Branch deleted on 2024-07-10 12:15_

---

_Comment by @codspeed-hq[bot] on 2024-07-10 12:16_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/files-touch-path)

### Merging #12273 will **degrade performances by 4.88%**

<sub>Comparing <code>files-touch-path</code> (6484af4) with <code>main</code> (e8b5341)</sub>



### Summary

`❌ 2` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/files-touch-path)._

### Benchmarks breakdown

|     | Benchmark | `main` | `files-touch-path` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `lexer[numpy/globals.py]` | 29.1 µs | 30.6 µs | -4.88% |
| ❌ | `linter/default-rules[pydantic/types.py]` | 1.7 ms | 1.8 ms | -4.69% |


---

_Comment by @github-actions[bot] on 2024-07-10 12:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
