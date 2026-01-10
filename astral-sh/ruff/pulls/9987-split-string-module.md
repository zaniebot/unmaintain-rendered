```yaml
number: 9987
title: split string module
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: split-string-module
created_at: 2024-02-14T16:24:40Z
updated_at: 2024-02-14T17:54:56Z
url: https://github.com/astral-sh/ruff/pull/9987
synced_at: 2026-01-10T22:57:09Z
```

# split string module

---

_Pull request opened by @MichaReiser on 2024-02-14 16:24_

## Summary

I started to find it difficult to navigate the `string/mod.rs` and decided to split it into `any` containing the `AnyString*` types
and `normalize` containing the normalization logic. 

This has the benefit that `mod.rs` now mainly contains the code related to formating continuations, quotes, and prefixes. 

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2024-02-14 16:24_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-02-14 16:24_

---

_Comment by @github-actions[bot] on 2024-02-14 16:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@dhruvmanila approved on 2024-02-14 17:54_

Makes sense 👍 

---

_Merged by @MichaReiser on 2024-02-14 17:54_

---

_Closed by @MichaReiser on 2024-02-14 17:54_

---

_Branch deleted on 2024-02-14 17:54_

---
