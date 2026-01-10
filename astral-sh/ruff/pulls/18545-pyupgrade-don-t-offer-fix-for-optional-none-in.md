```yaml
number: 18545
title: "[`pyupgrade`] Don't offer fix for `Optional[None]` in non-pep604-annotation-optional (`UP045)` or non-pep604-annotation-union (`UP007`)"
type: pull_request
state: merged
author: robsdedude
labels:
  - fixes
assignees: []
merged: true
base: main
head: fix/18508-up045-ignore-optional-none
created_at: 2025-06-08T08:04:19Z
updated_at: 2025-06-11T06:30:24Z
url: https://github.com/astral-sh/ruff/pull/18545
synced_at: 2026-01-10T18:45:04Z
```

# [`pyupgrade`] Don't offer fix for `Optional[None]` in non-pep604-annotation-optional (`UP045)` or non-pep604-annotation-union (`UP007`)

---

_Pull request opened by @robsdedude on 2025-06-08 08:04_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

As suggested here https://github.com/astral-sh/ruff/issues/18508#issuecomment-2950452307, this PR makes `UP045`/`UP007` not offer a fix for `Optional[None]`. Because
 1. rewriting it to `None | None` is not helpful or desired
 2. the user's intention is not clear

## Related Issue
https://github.com/astral-sh/ruff/issues/18508


---

_Converted to draft by @robsdedude on 2025-06-08 08:05_

---

_Renamed from "[`pyupgrade`] Ignore `Optional[None]` in non-pep604-annotation-optional (`UP045)` and non-pep604-annotation-union (`UP007`)" to "[`pyupgrade`] Don't offer fix for `Optional[None]` in non-pep604-annotation-optional (`UP045)` or non-pep604-annotation-union (`UP007`)" by @robsdedude on 2025-06-08 08:09_

---

_Comment by @github-actions[bot] on 2025-06-08 08:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @robsdedude on 2025-06-08 08:33_

---

_Label `fixes` added by @MichaReiser on 2025-06-11 06:17_

---

_Comment by @MichaReiser on 2025-06-11 06:18_

Thank you

---

_Merged by @MichaReiser on 2025-06-11 06:19_

---

_Closed by @MichaReiser on 2025-06-11 06:19_

---

_Branch deleted on 2025-06-11 06:30_

---
