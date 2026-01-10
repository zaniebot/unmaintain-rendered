```yaml
number: 13682
title: Harmonise methods for distinguishing different Python source types
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/remove-redundant-routines
created_at: 2024-10-08T17:42:37Z
updated_at: 2024-10-09T13:33:10Z
url: https://github.com/astral-sh/ruff/pull/13682
synced_at: 2026-01-10T20:59:36Z
```

# Harmonise methods for distinguishing different Python source types

---

_Pull request opened by @AlexWaygood on 2024-10-08 17:42_

## Summary

I was looking into #13246 to see if we could land the proposed change as part of Ruff 0.7, and noticed that we're currently pretty inconsistent in how we try to determine whether something is a Python source file or not (and, if so, what kind of source file it is). This PR tries to make it so that we only ever use `ruff_python_ast::PySourceType` for that purpose. This removes duplicated logic and the number of magic `"py"` or `"pyi"` strings across the codebase, and it should make it easier to make changes such as #13246 in the future.

## Test Plan

`cargo test`

---

_Label `internal` added by @AlexWaygood on 2024-10-08 17:42_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-08 17:42_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-08 17:42_

---

_@AlexWaygood reviewed on 2024-10-08 17:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_server/src/session/index.rs`:76 on 2024-10-08 17:58_

this compares case-sensitively where the existing code compares case-insensitively. I don't actually know if that's correct for Jupyter notebooks, but I do know that casing _does_ matter for other Python files. `import foo` succeeds if the `foo` module is found at `foo.py`, but fails if the `foo` module is found at `foo.PY`

---

_Comment by @github-actions[bot] on 2024-10-08 18:04_

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

_@zanieb reviewed on 2024-10-08 18:24_

---

_Review comment by @zanieb on `crates/red_knot_server/src/session/index.rs`:76 on 2024-10-08 18:24_

Hard to get data on this...

<img width="1775" alt="Screenshot 2024-10-08 at 1 24 01 PM" src="https://github.com/user-attachments/assets/a5bb8b27-edc6-4a40-b06a-6a463760d275">


---

_@zanieb reviewed on 2024-10-08 18:25_

---

_Review comment by @zanieb on `crates/red_knot_server/src/session/index.rs`:76 on 2024-10-08 18:25_

Seems overkill to make the breaking change for `ipynb`, I think the `.PY` import logic does make sense though? 

---

_@MichaReiser reviewed on 2024-10-08 20:41_

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/session/index.rs`:76 on 2024-10-08 20:41_

> this compares case-sensitively where the existing code compares case-insensitively. I don't actually know if that's correct for Jupyter notebooks, but I do know that casing _does_ matter for other Python files. `import foo` succeeds if the `foo` module is found at `foo.py`, but fails if the `foo` module is found at `foo.PY`

Is this also true on case insensitive file systems? I do think that import resolver is different from what we consider a python file

---

_@MichaReiser reviewed on 2024-10-08 20:54_

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/session/index.rs`:76 on 2024-10-08 20:54_

My experience is that most applications don't consider file extensions to be case sensitive. Naming a file test.txt or test.TXT doesn't change the application in which my desktop environment opens the file. That's why I think we shouldn't treat file extensions as case sensitive but I can see how this is beyond the scope of this pr

It does make sense that the module resolver only tests for the existence of ’<module>.py’ and whether ’<module>.PY’ is considered to match depends on the file system's case sensitivity 

---

_@AlexWaygood reviewed on 2024-10-09 10:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_server/src/session/index.rs`:76 on 2024-10-09 10:48_

For now I will revert these changes so that this PR is a "pure refactor" that does not change functionality. I think we may need to review this, though. In most places, we currently do case-sensitive matching for file extensions. Maybe that's correct or maybe it's incorrect, but it seems like we're pretty inconsistent right now. (And if that inconsistency is correct, because we need to apply logic in different situations, then we could at least add some comments to explain it more clearly 😄)

---

_Review requested from @zanieb by @AlexWaygood on 2024-10-09 10:49_

---

_Review request for @carljm removed by @AlexWaygood on 2024-10-09 10:49_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_builtins/rules/builtin_module_shadowing.rs`:48 on 2024-10-09 12:34_

Nit: This seems somewhat common. We could consider introducing a method for it: `is_python_or_stub`

---

_@MichaReiser approved on 2024-10-09 12:34_

---

_Comment by @MichaReiser on 2024-10-09 12:35_

Could you create an issue to re-visit the case-insensitive extension matching and assign it to the next milestone. We should re visit this soon.

---

_@AlexWaygood reviewed on 2024-10-09 13:00_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_builtins/rules/builtin_module_shadowing.rs`:48 on 2024-10-09 13:00_

makes sense

---

_Merged by @AlexWaygood on 2024-10-09 13:18_

---

_Closed by @AlexWaygood on 2024-10-09 13:18_

---

_Branch deleted on 2024-10-09 13:18_

---

_Comment by @codspeed-hq[bot] on 2024-10-09 13:19_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex/remove-redundant-routines)

### Merging #13682 will **degrade performances by 4.13%**

<sub>Comparing <code>alex/remove-redundant-routines</code> (ac777ae) with <code>main</code> (b9827a4)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex/remove-redundant-routines)._

### Benchmarks breakdown

|     | Benchmark | `main` | `alex/remove-redundant-routines` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `lexer[numpy/globals.py]` | 28.9 µs | 30.1 µs | -4.13% |


---

_Comment by @AlexWaygood on 2024-10-09 13:24_

the codspeed flamegraph for this PR is very noisy so it's hard to tell, but I _think_ that's just a false positive. I don't think I can see anything that would be plausibly related to this PR.

---

_Comment by @AlexWaygood on 2024-10-09 13:29_

> Could you create an issue to re-visit the case-insensitive extension matching and assign it to the next milestone. We should re visit this soon.

I opened https://github.com/astral-sh/ruff/issues/13691

---
