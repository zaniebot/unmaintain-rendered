```yaml
number: 13133
title: " [`FastAPI`] Avoid introducing invalid syntax in fix for `fast-api-non-annotated-dependency` (`FAST002`)"
type: pull_request
state: merged
author: arkuhn
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: akuhn/FAST002_autofix
created_at: 2024-08-28T05:07:49Z
updated_at: 2024-08-28T15:38:34Z
url: https://github.com/astral-sh/ruff/pull/13133
synced_at: 2026-01-12T15:55:43Z
```

#  [`FastAPI`] Avoid introducing invalid syntax in fix for `fast-api-non-annotated-dependency` (`FAST002`)

---

_@arkuhn_


<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

  - Context:[ this rule](https://github.com/astral-sh/ruff/pull/11579) swaps out default function argument values for type annotations
  - Problem: When this swap occurs _after_ a default argument value is already present, the function signature is no longer valid python code
  - Fix: Track default arguments and do not recommend a fix if we hit this condition, making this fix only sometimes possible.

Fixes https://github.com/astral-sh/ruff/issues/12982

Addtl:
- Breakout helper functions

## Test Plan

```
# 3 new test cases demonstrating default value scenarios
cargo run -p ruff -- check crates/ruff_linter/resources/test/fixtures/fastapi/FAST002.py  --no-cache --preview --select FAST002

cargo test
```


---

_Comment by @github-actions[bot] on 2024-08-28 05:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-08-28 14:11_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:101 on 2024-08-28 14:17_

```suggestion
            // - if we've encountered a non-updatable parameter with a default value, it's no longer
            //   safe. (https://github.com/astral-sh/ruff/issues/12982)
```

---

_@AlexWaygood approved on 2024-08-28 15:23_

Thanks! This seems like a reasonable fix to me

---

_Renamed from " [FAST002] fix - do not generate invalid args" to " [`FastAPI`] Avoid introducing invalid syntax in fix for `fast-api-non-annotated-dependency` (`FAST002`)" by @AlexWaygood on 2024-08-28 15:26_

---

_Label `bug` added by @AlexWaygood on 2024-08-28 15:26_

---

_Label `fixes` added by @AlexWaygood on 2024-08-28 15:26_

---

_Merged by @AlexWaygood on 2024-08-28 15:29_

---

_Closed by @AlexWaygood on 2024-08-28 15:29_

---

_Comment by @codspeed-hq[bot] on 2024-08-28 15:30_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/arkuhn:akuhn/FAST002_autofix)

### Merging #13133 will **improve performances by 7.61%**

<sub>Comparing <code>arkuhn:akuhn/FAST002_autofix</code> (8e2aaf3) with <code>main</code> (2e75cfb)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `arkuhn:akuhn/FAST002_autofix` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 784.7 µs | 729.2 µs | +7.61% |


---
