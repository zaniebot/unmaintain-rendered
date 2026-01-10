```yaml
number: 11718
title: Remove less used parser dependencies
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/parser-deps
created_at: 2024-06-03T09:15:12Z
updated_at: 2024-06-03T13:24:16Z
url: https://github.com/astral-sh/ruff/pull/11718
synced_at: 2026-01-10T21:56:00Z
```

# Remove less used parser dependencies

---

_Pull request opened by @dhruvmanila on 2024-06-03 09:15_

## Summary

This PR removes the following dependencies from the `ruff_python_parser` crate:
* `anyhow` (moved to dev dependencies)
* `is-macro`
* `itertools`

The main motivation is that they aren't used much.

Additionally, it updates the return type of `parse_type_annotation` to use a more specific `ParseError` instead of the generic `anyhow::Error`.

## Test Plan

`cargo insta test`


---

_Label `internal` added by @dhruvmanila on 2024-06-03 09:15_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-03 09:15_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/Cargo.toml`:24 on 2024-06-03 09:16_

This makes me happy :)

---

_@dhruvmanila reviewed on 2024-06-03 09:16_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:392 on 2024-06-03 09:16_

I think this is on me as I didn't realize that `find_position` was coming from `itertools` 😬

---

_@MichaReiser approved on 2024-06-03 09:18_

This doesn't just remove dependencies, I think it also improves the code/API (e.g. returning a specific `Error` over an `anyhow` is much easier to deal with. 

---

_Review requested from @carljm by @dhruvmanila on 2024-06-03 13:00_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-06-03 13:00_

---

_Review request for @carljm removed by @dhruvmanila on 2024-06-03 13:01_

---

_Review request for @AlexWaygood removed by @dhruvmanila on 2024-06-03 13:01_

---

_@dhruvmanila reviewed on 2024-06-03 13:05_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/Cargo.toml`:24 on 2024-06-03 13:05_

I knew it would :)

---

_Merged by @dhruvmanila on 2024-06-03 13:08_

---

_Closed by @dhruvmanila on 2024-06-03 13:08_

---

_Branch deleted on 2024-06-03 13:08_

---

_Comment by @github-actions[bot] on 2024-06-03 13:24_

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
