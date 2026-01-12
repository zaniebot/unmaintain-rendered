```yaml
number: 8675
title: "F-strings doesn't contain bytes literal for `PLW0129`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/PLW0129
created_at: 2023-11-14T11:40:17Z
updated_at: 2023-11-14T13:26:19Z
url: https://github.com/astral-sh/ruff/pull/8675
synced_at: 2026-01-10T23:40:55Z
```

# F-strings doesn't contain bytes literal for `PLW0129`

---

_Pull request opened by @dhruvmanila on 2023-11-14 11:40_

For the `PLW0129` rule, the f-string case shouldn't match against bytes literal as f-strings cannot contain them. F-strings are made up of either string literals or formatted expressions.

---

_Comment by @dhruvmanila on 2023-11-14 11:40_

Current dependencies on/for this PR:
* main
  * **PR #8675** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8675?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8675?utm_source=stack-comment).

---

_Comment by @github-actions[bot] on 2023-11-14 11:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @dhruvmanila on 2023-11-14 13:25_

---

_Renamed from "F-strings doesn't contain bytes literal" to "F-strings doesn't contain bytes literal in `PLW0129`" by @dhruvmanila on 2023-11-14 13:25_

---

_Renamed from "F-strings doesn't contain bytes literal in `PLW0129`" to "F-strings doesn't contain bytes literal for `PLW0129`" by @dhruvmanila on 2023-11-14 13:26_

---

_Merged by @dhruvmanila on 2023-11-14 13:26_

---

_Closed by @dhruvmanila on 2023-11-14 13:26_

---

_Branch deleted on 2023-11-14 13:26_

---
