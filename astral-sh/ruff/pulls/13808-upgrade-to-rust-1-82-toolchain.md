```yaml
number: 13808
title: Upgrade to Rust 1.82 toolchain
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/rust-1.82
created_at: 2024-10-18T10:22:43Z
updated_at: 2024-10-19T09:45:41Z
url: https://github.com/astral-sh/ruff/pull/13808
synced_at: 2026-01-10T20:59:37Z
```

# Upgrade to Rust 1.82 toolchain

---

_Pull request opened by @MichaReiser on 2024-10-18 10:22_

## Summary
Upgrade the developer toolchain. This does not bump the minimal MSRV.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-10-18 10:22_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-18 10:22_

---

_Label `internal` added by @MichaReiser on 2024-10-18 10:22_

---

_@AlexWaygood approved on 2024-10-18 11:20_

LGTM, but you have a merge conflict which is preventing CI from running :-)

---

_@dhruvmanila approved on 2024-10-18 11:41_

---

_Review comment by @AlexWaygood on `crates/red_knot_server/src/server.rs`:4 on 2024-10-18 12:06_

```suggestion
#[allow(deprecated)]  // Our MSRV doesn't allow us to use the new name
```

---

_Review comment by @AlexWaygood on `crates/red_knot_server/src/server.rs`:123 on 2024-10-18 12:06_

```suggestion
        #[allow(deprecated)]  // Our MSRV doesn't allow us to use the new name
```

---

_Review comment by @AlexWaygood on `crates/ruff_server/src/server.rs`:9 on 2024-10-18 12:06_

```suggestion
#[allow(deprecated)]  // Our MSRV doesn't allow us to use the new name
```

---

_Review comment by @AlexWaygood on `crates/ruff_server/src/server.rs`:129 on 2024-10-18 12:06_

```suggestion
        #[allow(deprecated)]  // Our MSRV doesn't allow us to use the new name
```

---

_@AlexWaygood reviewed on 2024-10-18 12:06_

I'd maybe leave some comments explaining why we're ignoring the deprecation warnings:

---

_Comment by @codspeed-hq[bot] on 2024-10-18 12:07_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha/rust-1.82)

### Merging #13808 will **degrade performances by 14.01%**

<sub>Comparing <code>micha/rust-1.82</code> (3f57a25) with <code>main</code> (4ecfe95)</sub>



### Summary

`❌ 6` regressions
`✅ 26` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha/rust-1.82)._

### Benchmarks breakdown

|     | Benchmark | `main` | `micha/rust-1.82` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `lexer[large/dataset.py]` | 1.1 ms | 1.2 ms | -14.01% |
| ❌ | `lexer[numpy/ctypeslib.py]` | 216.6 µs | 244.8 µs | -11.54% |
| ❌ | `lexer[numpy/globals.py]` | 30.1 µs | 32.6 µs | -7.6% |
| ❌ | `lexer[pydantic/types.py]` | 481.9 µs | 541.1 µs | -10.94% |
| ❌ | `lexer[unicode/pypinyin.py]` | 74.1 µs | 82.4 µs | -10.08% |
| ❌ | `parser[large/dataset.py]` | 4.8 ms | 5.1 ms | -5.14% |


---

_Merged by @MichaReiser on 2024-10-18 12:08_

---

_Closed by @MichaReiser on 2024-10-18 12:08_

---

_Branch deleted on 2024-10-18 12:08_

---

_Comment by @AlexWaygood on 2024-10-18 12:08_

> Merging #13808 will **degrade performances by 14.01%**

that doesn't look good...

---

_Comment by @MichaReiser on 2024-10-18 12:13_

Hmm nope

---

_Comment by @github-actions[bot] on 2024-10-18 12:22_

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

_Branch restored on 2024-10-19 09:45_

---
