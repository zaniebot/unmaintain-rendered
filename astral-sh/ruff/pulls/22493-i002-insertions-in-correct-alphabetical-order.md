```yaml
number: 22493
title: I002 insertions in correct alphabetical order
type: pull_request
state: open
author: 11happy
labels: []
assignees: []
base: main
head: I002
created_at: 2026-01-10T13:59:53Z
updated_at: 2026-01-10T14:17:31Z
url: https://github.com/astral-sh/ruff/pull/22493
synced_at: 2026-01-10T15:56:07Z
```

# I002 insertions in correct alphabetical order

---

_Pull request opened by @11happy on 2026-01-10 13:59_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR fixes #20811 , current approach reverses the order in `BtreeSet` however as pointed in https://github.com/astral-sh/ruff/issues/20811#issuecomment-3398958832 here we cannot use I`IndexSet` to preserve config order since Settings derives `CacheKey` which isn't implemented for `IndexSet`, another approach to preserve the original order might be to use `Vec` however lookup time complexity might get affected as a result.

<!-- How was it tested? -->
I have tested it locally its working as expected ,
<img width="2200" height="1071" alt="image" src="https://github.com/user-attachments/assets/7d97b488-1552-4a42-9d90-92acf55ec493" />



---

_Comment by @astral-sh-bot[bot] on 2026-01-10 14:07_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---
