```yaml
number: 13034
title: "Add `hyperfine` installation instructions; update `hyperfine` code samples"
type: pull_request
state: merged
author: olp-cs
labels:
  - documentation
assignees: []
merged: true
base: main
head: hyperfine
created_at: 2024-08-21T14:01:00Z
updated_at: 2024-08-22T04:57:31Z
url: https://github.com/astral-sh/ruff/pull/13034
synced_at: 2026-01-12T15:55:42Z
```

# Add `hyperfine` installation instructions; update `hyperfine` code samples

---

_@olp-cs_

## Summary

When following the step-by-step instructions to run the benchmarks in `CONTRIBUTING.md`, I encountered two errors: 

**Error 1:**
`bash: hyperfine: command not found`
    
**Solution**: I updated the instructions to include the step of installing the benchmark tool. 

**Error 2:**
```shell
$ ./target/release/ruff ./crates/ruff_linter/resources/test/cpython/ 
error: `ruff <path>` has been removed. Use `ruff check <path>` instead.
```
**Solution**: I added `check`.

## Test Plan

I tested it by running the benchmark-related commands in a new workspace within GitHub Codespaces.

---

_@charliermarsh approved on 2024-08-21 14:05_

---

_Label `documentation` added by @charliermarsh on 2024-08-21 14:05_

---

_Comment by @charliermarsh on 2024-08-21 14:06_

I'm a little mixed on adding `hyperfine` to the dev container since most contributors won't need to run benchmarks, but not strongly opposed at all.

---

_Comment by @olp-cs on 2024-08-21 14:11_

> most contributors won't need to run benchmarks

I agree, that makes sense! I will revert this change.

---

_Renamed from "Add `hyperfine` installation instructions to docs and .devcontainer; update `hyperfine` code samples" to "Add `hyperfine` installation instructions; update `hyperfine` code samples" by @olp-cs on 2024-08-21 14:14_

---

_Comment by @codspeed-hq[bot] on 2024-08-21 14:14_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/olp-cs:hyperfine)

### Merging #13034 will **degrade performances by 7.41%**

<sub>Comparing <code>olp-cs:hyperfine</code> (c2f0d1f) with <code>main</code> (ecd9e6a)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/olp-cs:hyperfine)._

### Benchmarks breakdown

|     | Benchmark | `main` | `olp-cs:hyperfine` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[numpy/globals.py]` | 170.5 µs | 184.2 µs | -7.41% |


---

_Merged by @dhruvmanila on 2024-08-22 03:35_

---

_Closed by @dhruvmanila on 2024-08-22 03:35_

---

_Branch deleted on 2024-08-22 04:57_

---
