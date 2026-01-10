```yaml
number: 11741
title: "Disallow access to `Parsed` output, use the API instead"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/checker-comment-ranges
created_at: 2024-06-04T16:56:10Z
updated_at: 2024-06-05T08:34:19Z
url: https://github.com/astral-sh/ruff/pull/11741
synced_at: 2026-01-10T21:56:00Z
```

# Disallow access to `Parsed` output, use the API instead

---

_Pull request opened by @dhruvmanila on 2024-06-04 16:56_

## Summary

This PR is a follow-up to #11740 to restrict access to the `Parsed` output by replacing the `parsed` API function with a more specific one. Currently, that is `comment_ranges` but the linked PR exposes a `tokens` method.

The main motivation is so that there's no way to get an incorrect information from the checker. And, it also encapsulates the source of the comment ranges and the tokens itself. This way it would become easier to just update the checker if the source for these information changes in the future. 

## Test Plan

`cargo insta test`


---

_Label `internal` added by @dhruvmanila on 2024-06-04 16:56_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-06-04 16:56_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-04 16:56_

---

_Comment by @github-actions[bot] on 2024-06-04 17:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@zanieb approved on 2024-06-04 18:28_

---

_@MichaReiser approved on 2024-06-05 07:08_

---

_Merged by @dhruvmanila on 2024-06-05 08:24_

---

_Closed by @dhruvmanila on 2024-06-05 08:24_

---

_Branch deleted on 2024-06-05 08:24_

---
