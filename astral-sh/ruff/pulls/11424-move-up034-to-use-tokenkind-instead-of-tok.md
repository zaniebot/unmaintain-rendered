```yaml
number: 11424
title: "Move `UP034` to use `TokenKind` instead of `Tok`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/extraneous-parens
created_at: 2024-05-14T12:28:09Z
updated_at: 2024-05-14T17:32:40Z
url: https://github.com/astral-sh/ruff/pull/11424
synced_at: 2026-01-12T15:55:38Z
```

# Move `UP034` to use `TokenKind` instead of `Tok`

---

_@dhruvmanila_

## Summary

This PR follows up from #11420 to move `UP034` to use `TokenKind` instead of `Tok`.

The main reason to have a separate PR is so that the reviewing is easy. This required a lot more updates because the rule used an index (`i`) to keep track of the current position in the token vector. Now, as it's just an iterator, we just use `next` to move the iterator forward and extract the relevant information.

This is part of https://github.com/astral-sh/ruff/issues/11401

## Test Plan

`cargo test`


---

_Label `internal` added by @dhruvmanila on 2024-05-14 12:28_

---

_Marked ready for review by @dhruvmanila on 2024-05-14 12:31_

---

_Renamed from "Use tokenize for linter benchmark" to "Move `UP034` to use `TokenKind` instead of `Tok`" by @dhruvmanila on 2024-05-14 12:31_

---

_Comment by @github-actions[bot] on 2024-05-14 12:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-05-14 14:49_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-14 17:08_

---

_Merged by @dhruvmanila on 2024-05-14 17:28_

---

_Closed by @dhruvmanila on 2024-05-14 17:28_

---

_Branch deleted on 2024-05-14 17:28_

---
