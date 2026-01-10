```yaml
number: 11725
title: "red-knot: Use `parse_unchecked` to get all parse errors"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: red-knot-use-parse-unchecked
created_at: 2024-06-03T15:37:13Z
updated_at: 2024-06-04T06:20:34Z
url: https://github.com/astral-sh/ruff/pull/11725
synced_at: 2026-01-10T21:56:00Z
```

# red-knot: Use `parse_unchecked` to get all parse errors

---

_Pull request opened by @MichaReiser on 2024-06-03 15:37_

## Summary

The parser now supports to return all parse errors instead of just the first. Let's use that and also reuse the ast's `Parsed` module instead of defining our own.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-06-03 15:37_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-06-03 15:37_

---

_Label `red-knot` added by @MichaReiser on 2024-06-03 15:37_

---

_Comment by @github-actions[bot] on 2024-06-03 15:56_

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

_Review comment by @carljm on `crates/red_knot/src/lint.rs`:90 on 2024-06-03 19:34_

Why do we need the reborrow here?

---

_@carljm approved on 2024-06-03 19:36_

Looks good! Love a PR that removes more lines of code than it adds.

---

_Review comment by @dhruvmanila on `crates/red_knot/src/parse.rs`:23 on 2024-06-04 05:11_

I think `parse_unchecked_source` (also used in the linter) would be more useful here if I'm not mistaken. That API takes in the `PySourceType` instead of `Mode` and always returns `ModModule`. This also means that it'll be easier to support `.pyi` and `.ipynb` filetypes in the future.

---

_@dhruvmanila approved on 2024-06-04 05:11_

---

_@MichaReiser reviewed on 2024-06-04 05:56_

---

_Review comment by @MichaReiser on `crates/red_knot/src/lint.rs`:90 on 2024-06-04 05:56_

`parsed` returns an `Arc<Parsed<...>>`. We could either pass in the owned Arc, or just the reference as I do here because we never need to clone (and the context already has lifetimes anyway)

---

_Merged by @MichaReiser on 2024-06-04 06:04_

---

_Closed by @MichaReiser on 2024-06-04 06:04_

---

_Branch deleted on 2024-06-04 06:04_

---
