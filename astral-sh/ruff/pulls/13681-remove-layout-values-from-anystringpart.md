```yaml
number: 13681
title: "Remove layout values from `AnyStringPart`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: micha/refactor-any-string-part
created_at: 2024-10-08T15:06:23Z
updated_at: 2024-10-09T06:25:41Z
url: https://github.com/astral-sh/ruff/pull/13681
synced_at: 2026-01-10T20:59:36Z
```

# Remove layout values from `AnyStringPart`

---

_Pull request opened by @MichaReiser on 2024-10-08 15:06_

## Summary

This is a small refactor to simplify `AnyStringPart`. I always found it difficult to reason about how the `quoting` flows through the formatting
and this was part due to it being routed to `parts`, `AnyStringPart` and finally its `Format` implementation. 

This PR makes `AnyStringPart` a simple enum over the different parts as it used to be and moves the logic for 
formatting a part into the `FormatImplicitConcatenatedString`, the only place where it is used. 

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2024-10-08 15:06_

---

_Label `formatter` added by @MichaReiser on 2024-10-08 15:06_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/any.rs`:73 on 2024-10-08 15:09_

This is kind of what triggered the refactor. It felt off that it is necessary to pass an arbitrary `Quoating` to iterate over the parts.

---

_@MichaReiser reviewed on 2024-10-08 15:09_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-10-08 15:18_

---

_Comment by @github-actions[bot] on 2024-10-08 15:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "Remove layout values from `AnyStringPart" to "Remove layout values from `AnyStringPart`" by @MichaReiser on 2024-10-08 16:04_

---

_@AlexWaygood approved on 2024-10-08 16:16_

---

_@dhruvmanila approved on 2024-10-09 03:56_

Thanks!

---

_Merged by @MichaReiser on 2024-10-09 06:25_

---

_Closed by @MichaReiser on 2024-10-09 06:25_

---

_Branch deleted on 2024-10-09 06:25_

---
