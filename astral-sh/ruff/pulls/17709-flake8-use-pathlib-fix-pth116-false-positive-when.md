```yaml
number: 17709
title: "[`flake8-use-pathlib`] Fix `PTH116` false positive when `stat` is passed a file descriptor"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-PTH116
created_at: 2025-04-29T14:17:04Z
updated_at: 2025-05-08T21:29:09Z
url: https://github.com/astral-sh/ruff/pull/17709
synced_at: 2026-01-10T18:57:03Z
```

# [`flake8-use-pathlib`] Fix `PTH116` false positive when `stat` is passed a file descriptor

---

_Pull request opened by @LaBatata101 on 2025-04-29 14:17_



<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Contains the same changes to the semantic type inference as #17705.
<!-- What's the purpose of the change? What does it do, and why? -->
Fixes #17693.
## Test Plan

<!-- How was it tested? -->
Snapshot tests.

---

_Comment by @github-actions[bot] on 2025-04-29 14:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-04-29 20:17_

I'll review this, but let's merge #17705 first to get the shared changes and then update this PR :)

Same with #17712.

---

_Comment by @LaBatata101 on 2025-04-29 20:53_

Fixing the merge conflicts

---

_Label `bug` added by @MichaReiser on 2025-04-30 06:46_

---

_Label `rule` added by @MichaReiser on 2025-04-30 06:46_

---

_@MichaReiser approved on 2025-04-30 06:47_

Thanks. We should probably exclude bytes string from this change as discussed on the issue, Sorry about that

---

_Comment by @LaBatata101 on 2025-04-30 14:04_

> Thanks. We should probably exclude bytes string from this change as discussed on the issue, Sorry about that

Alright, no problem. So, should I remove the bytes string changes from the other PTH* rules as well?

---

_Comment by @dhruvmanila on 2025-04-30 14:39_

I realized that the refactor that I did in https://github.com/astral-sh/ruff/pull/17715 will cause merge conflicts in your PRs, I'll resolve them.

---

_Renamed from "[`flake8-use-pathlib`] Fix `PTH116` false positive when `stat` is passed a file descriptor or bytes string" to "[`flake8-use-pathlib`] Fix `PTH116` false positive when `stat` is passed a file descriptor" by @LaBatata101 on 2025-04-30 14:46_

---

_Comment by @MichaReiser on 2025-05-01 06:16_

> Alright, no problem. So, should I remove the bytes string changes from the other PTH* rules as well?

That would be great, yes

---

_Merged by @MichaReiser on 2025-05-01 06:16_

---

_Closed by @MichaReiser on 2025-05-01 06:16_

---

_Branch deleted on 2025-05-08 21:29_

---
