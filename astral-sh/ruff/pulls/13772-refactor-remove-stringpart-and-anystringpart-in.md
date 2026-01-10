```yaml
number: 13772
title: "Refactor: Remove `StringPart` and `AnyStringPart` in favor of `StringLikePart`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: micha/remove-string-part
created_at: 2024-10-16T09:51:03Z
updated_at: 2024-10-16T10:52:07Z
url: https://github.com/astral-sh/ruff/pull/13772
synced_at: 2026-01-10T20:59:37Z
```

# Refactor: Remove `StringPart` and `AnyStringPart` in favor of `StringLikePart`

---

_Pull request opened by @MichaReiser on 2024-10-16 09:51_

## Summary

I don't think having both types still makes sense. 
The way I remember it is that we had `StringPart` first, a generic representation of any string part. 
We later introduced `AnyStringPart` without realizing that we now have two generic representations of a string part. We then also added `StringLike` when we needed a similar construct in the linter. And there were three representations :)  

This PR removes `StringPart`  and `AnyStringPart` and replaces them with `StringLikePart`, to only have one generic string part representation.

Split out from https://github.com/astral-sh/ruff/pull/13663

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2024-10-16 09:51_

---

_Label `formatter` added by @MichaReiser on 2024-10-16 09:51_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-10-16 09:52_

---

_@dhruvmanila reviewed on 2024-10-16 09:56_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/string/any.rs`:172 on 2024-10-16 09:56_

I think we could possibly remove this as well and use the one in the `ruff_python_ast` crate:

https://github.com/astral-sh/ruff/blob/61fc39bdb6dd0da7d594a9d2755a353eceb3a771/crates/ruff_python_ast/src/expression.rs#L446-L452

---

_Comment by @github-actions[bot] on 2024-10-16 10:06_

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

_@MichaReiser reviewed on 2024-10-16 10:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/any.rs`:172 on 2024-10-16 10:28_

Oh good point. It does make it slightly more annoying to add formatter specific methods but I think it's fine. 

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-10-16 10:28_

---

_Renamed from "Refactor: Remove `StringPart` and use `AnyStringPart` instead" to "Refactor: Remove `StringPart` and `AnyStringPart` in favor of `StringLike`" by @MichaReiser on 2024-10-16 10:29_

---

_Renamed from "Refactor: Remove `StringPart` and `AnyStringPart` in favor of `StringLike`" to "Refactor: Remove `StringPart` and `AnyStringPart` in favor of `StringLikePart`" by @MichaReiser on 2024-10-16 10:30_

---

_@dhruvmanila approved on 2024-10-16 10:34_

This is great, lots of code removed. Thanks for doing this!

I agree that it's a bit annoying to add formatter specific methods but the `StringLikeExtensions` trait is a good idea.

---

_Merged by @MichaReiser on 2024-10-16 10:52_

---

_Closed by @MichaReiser on 2024-10-16 10:52_

---

_Branch deleted on 2024-10-16 10:52_

---
