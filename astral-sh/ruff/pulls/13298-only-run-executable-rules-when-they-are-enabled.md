```yaml
number: 13298
title: Only run executable rules when they are enabled
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/only-run-executable-rules-when-enabled
created_at: 2024-09-10T00:39:27Z
updated_at: 2024-09-10T00:53:02Z
url: https://github.com/astral-sh/ruff/pull/13298
synced_at: 2026-01-12T15:55:43Z
```

# Only run executable rules when they are enabled

---

_@MichaReiser_

## Summary

I noticed that the `flake8_executable::*ExecutableFile` rules run as part of the benchmark when I reviewed a [diff](https://codspeed.io/astral-sh/ruff/branches/alex/f821-error-msg) from another PR. 

That surprised me because IO rules should be disabled in codspeed because they are more flaky because they're syscalls. 

This PR disables the rules for good in benchmarks by adding the missing checks in `shebang_not_executable` to only call
the rules when they're enabled unlike before where we called the rules when any `flake8_executable` rule was enabled.  

## Test Plan

I added panis to the executable file rules and verified that running the benchmarks with the panics present doesn't panic.


---

_Label `internal` added by @MichaReiser on 2024-09-10 00:39_

---

_Comment by @codspeed-hq[bot] on 2024-09-10 00:45_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha/only-run-executable-rules-when-enabled)

### Merging #13298 will **improve performances by 7.12%**

<sub>Comparing <code>micha/only-run-executable-rules-when-enabled</code> (ec9f07f) with <code>main</code> (5ef6979)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `micha/only-run-executable-rules-when-enabled` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 871.6 µs | 813.6 µs | +7.12% |


---

_Merged by @MichaReiser on 2024-09-10 00:46_

---

_Closed by @MichaReiser on 2024-09-10 00:46_

---

_Branch deleted on 2024-09-10 00:46_

---

_Comment by @github-actions[bot] on 2024-09-10 00:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
