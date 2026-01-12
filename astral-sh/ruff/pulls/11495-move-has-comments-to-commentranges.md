```yaml
number: 11495
title: "Move `has_comments` to `CommentRanges`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/has-comments
created_at: 2024-05-22T13:31:29Z
updated_at: 2024-05-28T06:37:05Z
url: https://github.com/astral-sh/ruff/pull/11495
synced_at: 2026-01-12T15:55:38Z
```

# Move `has_comments` to `CommentRanges`

---

_@dhruvmanila_

## Summary

This PR moves the `has_comments` function from `Indexer` to `CommentRanges`. The main motivation is that the `CommentRanges` will now be built by the parser which is shared between the linter and the formatter. Thus, the `CommentRanges` will be removed from the `Indexer`.

## Test Plan

`cargo test`


---

_Label `internal` added by @dhruvmanila on 2024-05-22 13:31_

---

_@charliermarsh approved on 2024-05-22 13:32_

---

_Merged by @dhruvmanila on 2024-05-22 13:35_

---

_Closed by @dhruvmanila on 2024-05-22 13:35_

---

_Branch deleted on 2024-05-22 13:35_

---

_Comment by @github-actions[bot] on 2024-05-22 13:50_

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

_@MichaReiser reviewed on 2024-05-27 10:42_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/assertion.rs`:387 on 2024-05-27 10:42_

Nit: You could have kept the `has_comments` method on `Indexer` and simply forwarded the call to `CommentRanges` to reduce the diff size (It also increases encapsulation because callers don't need to know how the Indexer represents the data internally)

---

_@dhruvmanila reviewed on 2024-05-28 06:37_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/assertion.rs`:387 on 2024-05-28 06:37_

I'm not sure if that makes any difference in terms of encapsulation because `CommentRanges` is already exposed via `indexer.comment_ranges` and calls to the other methods on `CommentRanges` are done directly.

---
