```yaml
number: 12708
title: "[`refurb`] - fix unused autofix for `implicit-cwd` (`FURB177`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - fixes
assignees: []
merged: true
base: main
head: fix-unused-autofix
created_at: 2024-08-06T05:21:54Z
updated_at: 2024-08-06T06:09:35Z
url: https://github.com/astral-sh/ruff/pull/12708
synced_at: 2026-01-10T21:47:02Z
```

# [`refurb`] - fix unused autofix for `implicit-cwd` (`FURB177`)

---

_Pull request opened by @diceroll123 on 2024-08-06 05:21_

## Summary

The autofix for this already existed, it just was not pushed to the checker.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2024-08-06 05:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-08-06 06:09_

Nice find! 

I wondered if there was a reason for not pushing the fix, but nope. Has been like this since the rule was added. 

Thanks for PRing the fix to add the right diagnostic ;)

---

_Label `fixes` added by @MichaReiser on 2024-08-06 06:09_

---

_Merged by @MichaReiser on 2024-08-06 06:09_

---

_Closed by @MichaReiser on 2024-08-06 06:09_

---
