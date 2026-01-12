```yaml
number: 13810
title: "Revert \"Upgrade to Rust 1.82 toolchain\""
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: revert-13808-micha/rust-1.82
created_at: 2024-10-18T12:13:41Z
updated_at: 2024-10-18T12:37:31Z
url: https://github.com/astral-sh/ruff/pull/13810
synced_at: 2026-01-12T15:55:45Z
```

# Revert "Upgrade to Rust 1.82 toolchain"

---

_@MichaReiser_

Reverts astral-sh/ruff#13808

There's a huge perf regression. Let's revert this for now

---

_Review requested from @carljm by @MichaReiser on 2024-10-18 12:13_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-18 12:13_

---

_Label `internal` added by @MichaReiser on 2024-10-18 12:13_

---

_@AlexWaygood approved on 2024-10-18 12:16_

We should try to see if we can minimize a repro of the perf regression to "just the toolchain upgrade", without any of the linter-suggested changes. (But best to do that separately; a clean revert seems like a good course of action for now.)

---

_Merged by @MichaReiser on 2024-10-18 12:18_

---

_Closed by @MichaReiser on 2024-10-18 12:18_

---

_Branch deleted on 2024-10-18 12:18_

---

_Comment by @codspeed-hq[bot] on 2024-10-18 12:19_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/revert-13808-micha/rust-1.82)

### Merging #13810 will **improve performances by 16.31%**

<sub>Comparing <code>revert-13808-micha/rust-1.82</code> (bfa74ba) with <code>main</code> (ff72055)</sub>



### Summary

`⚡ 7` improvements
`✅ 25` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `revert-13808-micha/rust-1.82` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `lexer[large/dataset.py]` | 1.2 ms | 1.1 ms | +16.31% |
| ⚡ | `lexer[numpy/ctypeslib.py]` | 244.8 µs | 216.5 µs | +13.07% |
| ⚡ | `lexer[numpy/globals.py]` | 32.5 µs | 30.1 µs | +8.12% |
| ⚡ | `lexer[pydantic/types.py]` | 541.1 µs | 481.9 µs | +12.3% |
| ⚡ | `lexer[unicode/pypinyin.py]` | 82.4 µs | 74.1 µs | +11.21% |
| ⚡ | `parser[large/dataset.py]` | 5.1 ms | 4.8 ms | +5.41% |
| ⚡ | `parser[numpy/ctypeslib.py]` | 973.1 µs | 935.2 µs | +4.05% |


---

_Comment by @github-actions[bot] on 2024-10-18 12:33_

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

_Comment by @MichaReiser on 2024-10-18 12:34_

> We should try to see if we can minimize a repro of the perf regression to "just the toolchain upgrade", without any of the linter-suggested changes. (But best to do that separately; a clean revert seems like a good course of action for now.)

There were no linter changes in the parser or lexer. So we can consider them to be isolated.

---

_Comment by @AlexWaygood on 2024-10-18 12:37_

I was wondering if any of the macros that changed were used by the parser or lexer. But looking again, I see that all of them except `cache_key` seem to be linter-specific macros, not ones that would be used by the parser or lexer. And even `cache_key` does not itself appear to be used in the parser or lexer.

---
