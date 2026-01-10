```yaml
number: 16545
title: "[syntax-errors] Star annotations before Python 3.11"
type: pull_request
state: merged
author: ntBre
labels:
  - parser
  - preview
assignees: []
merged: true
base: main
head: brent/syn-star-annotation
created_at: 2025-03-06T23:38:51Z
updated_at: 2025-03-14T15:24:59Z
url: https://github.com/astral-sh/ruff/pull/16545
synced_at: 2026-01-10T19:49:01Z
```

# [syntax-errors] Star annotations before Python 3.11

---

_Pull request opened by @ntBre on 2025-03-06 23:38_

Summary
--

This is closely related to (and stacked on) https://github.com/astral-sh/ruff/pull/16544 and detects star annotations in function definitions.

I initially called the variant `StarExpressionInAnnotation` to mirror `StarExpressionInIndex`, but I realized it's not really a "star expression" in this position and renamed it. `StarAnnotation` seems in line with the PEP.

Test Plan
--

Two new inline tests. It looked like there was pretty good existing coverage of this syntax, so I just added simple examples to test the version cutoff.

---

_Label `parser` added by @ntBre on 2025-03-06 23:38_

---

_Label `preview` added by @ntBre on 2025-03-06 23:38_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-06 23:38_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-06 23:38_

---

_Comment by @github-actions[bot] on 2025-03-06 23:48_

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

_@MichaReiser reviewed on 2025-03-07 09:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:666 on 2025-03-07 09:13_

The link here is incorrect. I think it should point to `https://peps.python.org/pep-0646/#change-2-args-as-a-typevartuple`

---

_@MichaReiser approved on 2025-03-07 09:16_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/error.rs`:730 on 2025-03-07 10:07_

Technically, it's an "annotation expression" but as a user facing message I think either "star annotation" or "star annotation expression" is fine.

---

_@dhruvmanila approved on 2025-03-07 10:09_

---

_@ntBre reviewed on 2025-03-07 15:10_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:730 on 2025-03-07 15:10_

Oh good to know, thanks! I guess I'll leave it alone for now, but I'm happy to switch to `star annotation expression` if that would be more precise. It looks like pyright's message here is "Unpack operator not allowed in type expression," which also makes sense.

---

_Merged by @ntBre on 2025-03-14 15:20_

---

_Closed by @ntBre on 2025-03-14 15:20_

---

_Branch deleted on 2025-03-14 15:20_

---
