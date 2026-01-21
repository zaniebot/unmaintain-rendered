```yaml
number: 22773
title: "[`pyupgrade`] Make fix unsafe if it deletes comments (`UP041`)"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
assignees: []
merged: true
base: main
head: UP041
created_at: 2026-01-20T18:19:54Z
updated_at: 2026-01-21T14:12:11Z
url: https://github.com/astral-sh/ruff/pull/22773
synced_at: 2026-01-21T15:05:46Z
```

# [`pyupgrade`] Make fix unsafe if it deletes comments (`UP041`)

---

_@chirizxc_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @astral-sh-bot[bot] on 2026-01-20 18:30_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `fixes` added by @ntBre on 2026-01-21 14:04_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/timeout_error_alias.rs`:44 on 2026-01-21 14:05_

```suggestion
/// within the exception expression range.
```

I'd probably err on the slightly shorter side like in your other PRs. I think it's clear enough why deleting comments is bad.

---

_@ntBre approved on 2026-01-21 14:07_

Thanks! I just had one nit about the message that I'll apply.

---

_Merged by @ntBre on 2026-01-21 14:12_

---

_Closed by @ntBre on 2026-01-21 14:12_

---
