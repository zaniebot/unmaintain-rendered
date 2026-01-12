```yaml
number: 9437
title: Use Rust 1.75 toolchain
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: rust-1.75
created_at: 2024-01-08T11:11:05Z
updated_at: 2024-01-08T17:03:17Z
url: https://github.com/astral-sh/ruff/pull/9437
synced_at: 2026-01-12T15:55:28Z
```

# Use Rust 1.75 toolchain

---

_@MichaReiser_

## Summary

Upgrade the rust toolchain from version 1.74 to 1.75. 

This does not change the minimal required Rust version to compile Ruff. It only upgrades Clippy, Rustfmt, Rustc etc used locally and in CI

## Test Plan

cargo test


---

_Label `internal` added by @MichaReiser on 2024-01-08 11:11_

---

_Comment by @codspeed-hq[bot] on 2024-01-08 11:20_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/rust-1.75)

### Merging #9437 will **degrade performances by 5.36%**

<sub>Comparing <code>rust-1.75</code> (721f3d5) with <code>main</code> (ba71772)</sub>



### Summary

`⚡ 12` improvements
`❌ 4 (👁 4)` regressions
`✅ 14` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `rust-1.75` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[numpy/ctypeslib.py]` | 12.1 ms | 11.6 ms | +4.4% |
| ⚡ | `parser[large/dataset.py]` | 70.5 ms | 67.4 ms | +4.58% |
| 👁 | `lexer[unicode/pypinyin.py]` | 545.7 µs | 574.7 µs | -5.04% |
| 👁 | `lexer[pydantic/types.py]` | 3.7 ms | 3.9 ms | -5.28% |
| ⚡ | `parser[pydantic/types.py]` | 26.9 ms | 25.7 ms | +4.65% |
| 👁 | `lexer[numpy/ctypeslib.py]` | 1.8 ms | 1.9 ms | -4.62% |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 23.5 ms | 22.3 ms | +5.28% |
| ⚡ | `linter/all-with-preview-rules[numpy/ctypeslib.py]` | 26.9 ms | 25.6 ms | +5.1% |
| 👁 | `lexer[large/dataset.py]` | 8.4 ms | 8.9 ms | -5.36% |
| ⚡ | `linter/all-with-preview-rules[pydantic/types.py]` | 56.9 ms | 53.9 ms | +5.74% |
| ⚡ | `linter/all-with-preview-rules[large/dataset.py]` | 124.6 ms | 118.1 ms | +5.49% |
| ⚡ | `parser[unicode/pypinyin.py]` | 4.2 ms | 4 ms | +4.39% |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 3 ms | 2.9 ms | +4.84% |
| ⚡ | `linter/all-rules[pydantic/types.py]` | 47.8 ms | 45 ms | +6.27% |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 3.4 ms | 3.2 ms | +4.66% |
| ⚡ | `linter/all-rules[large/dataset.py]` | 102.8 ms | 96.6 ms | +6.43% |


---

_Comment by @github-actions[bot] on 2024-01-08 11:32_

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

_@konstin approved on 2024-01-08 13:50_

The code looks good, the perf diff is massive though

---

_Comment by @charliermarsh on 2024-01-08 14:27_

Interesting. Note that the parser benchmark is a superset of the lexer benchmark... So if the parser is improving, it seems like a net win?

---

_Comment by @konstin on 2024-01-08 14:46_

Looking at the flamegraphs, it seems that lexer became slower because `MultiPeek` isn't inlined anymore, but the parser speedup is due to `check_tokens` which also has new functions in the flamegraph, so maybe it's both the same inlining changes? Anyway i'm happy if the parser improves.

---

_Comment by @MichaReiser on 2024-01-08 16:00_

Agree, I think it's both because of the changed heuristic of when to inline functions. I can try to fix some of the regression by adding `inline`s but I'm not sure if it is worth it, especially considering that the parser performance improves

---

_Merged by @MichaReiser on 2024-01-08 17:03_

---

_Closed by @MichaReiser on 2024-01-08 17:03_

---

_Branch deleted on 2024-01-08 17:03_

---
