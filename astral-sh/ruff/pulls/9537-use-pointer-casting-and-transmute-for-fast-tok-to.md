```yaml
number: 9537
title: "Use pointer casting and transmute for fast `Tok` to `TokenKind` conversion"
type: pull_request
state: closed
author: MichaReiser
labels: []
assignees: []
draft: true
base: main
head: transmute-between-token-and-token-kind
created_at: 2024-01-15T19:53:48Z
updated_at: 2024-02-29T13:19:00Z
url: https://github.com/astral-sh/ruff/pull/9537
synced_at: 2026-01-12T15:55:29Z
```

# Use pointer casting and transmute for fast `Tok` to `TokenKind` conversion

---

_@MichaReiser_

## Summary

This PR changes the implementation of `TokenKind::from_token` to use the `Tok` discriminant 
and `transmute` instead of an explicit match statement. 

The reason is that a fast conversion between the two is desired. It sometimes shows up in the 
logical lines benchmarks (and blank lines soon?) and the new parser calls the conversion for every token.

This has the unfortunate downside that it increases the `Tok` size from 32 bytes to 40 bytes, which kind of makes sense. 

## Test Plan

**LLVM Lines**

* This PR: 8
* Main: 216

That's a TODO. I need to write a test that tests the conversion for every possible token.  


---

_Comment by @codspeed-hq[bot] on 2024-01-15 20:06_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/transmute-between-token-and-token-kind)

### Merging #9537 will **degrade performances by 6.73%**

<sub>Comparing <code>transmute-between-token-and-token-kind</code> (4ffcf25) with <code>main</code> (b983ab1)</sub>



### Summary

`❌ 2` regressions
`✅ 28` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/transmute-between-token-and-token-kind)._

### Benchmarks breakdown

|     | Benchmark | `main` | `transmute-between-token-and-token-kind` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 12.2 ms | 13.1 ms | -6.73% |
| ❌ | `parser[large/dataset.py]` | 70.8 ms | 73.8 ms | -4.02% |


---

_Comment by @github-actions[bot] on 2024-01-15 20:17_

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

_@BurntSushi reviewed on 2024-01-15 20:34_

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/src/token.rs`:903 on 2024-01-15 20:34_

How sure are we that this was hurting us perf-wise? Another thing to check is whether this was actually doing branching or if the compiler was smart enough to optimize this down without needing `unsafe`.

---

_@MichaReiser reviewed on 2024-01-15 20:36_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token.rs`:903 on 2024-01-15 20:36_

That's what I tried to figure out with this PR, but our benchmarks aren't stable enough right now to give much signal. Maybe something to wait and re-test with the new parser.

I was hoping that Rust is clever enough but the `match` version generates 200 LLVM lines in release where this version only generates 8 (and likely without any branching which would be beneficial in hot loops where each token has a different kind). 



---

_@BurntSushi reviewed on 2024-01-15 21:39_

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/src/token.rs`:903 on 2024-01-15 21:39_

Not sure about the LLVM IR, but it looks like the actual code generated should be pretty good: https://godbolt.org/z/8a4Ks3e19

(I've run into this issue before and have tried the same sort of optimization you're trying here, and I don't recall ever being successful. Probably because the compiler is smart enough to optimize this.)

---

_@MichaReiser reviewed on 2024-01-15 21:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token.rs`:903 on 2024-01-15 21:52_

Thx. I take a closer look tomorrow and plan to do some profiling on the new parser before perusing this more

---

_Closed by @MichaReiser on 2024-02-02 10:27_

---

_Branch deleted on 2024-02-29 13:19_

---
