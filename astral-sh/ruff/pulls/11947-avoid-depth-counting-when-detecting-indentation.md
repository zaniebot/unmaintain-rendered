```yaml
number: 11947
title: Avoid depth counting when detecting indentation
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/stylist-depth
created_at: 2024-06-20T04:51:26Z
updated_at: 2024-06-20T05:12:36Z
url: https://github.com/astral-sh/ruff/pull/11947
synced_at: 2026-01-12T15:55:39Z
```

# Avoid depth counting when detecting indentation

---

_@dhruvmanila_

## Summary

This PR avoids the `depth` counter when detecting indentation from non-logical lines because it seems to never be used. It might have been a leftover when the logic was added originally in #11608.

## Test Plan

`cargo insta test`


---

_Label `internal` added by @dhruvmanila on 2024-06-20 04:51_

---

_Comment by @github-actions[bot] on 2024-06-20 05:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dhruvmanila on 2024-06-20 05:12_

---

_Closed by @dhruvmanila on 2024-06-20 05:12_

---

_Branch deleted on 2024-06-20 05:12_

---
