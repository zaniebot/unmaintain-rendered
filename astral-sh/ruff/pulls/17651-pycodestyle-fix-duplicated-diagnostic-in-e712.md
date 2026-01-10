```yaml
number: 17651
title: "[`pycodestyle`] Fix duplicated diagnostic in `E712`"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-E712
created_at: 2025-04-26T23:09:42Z
updated_at: 2025-04-30T20:55:30Z
url: https://github.com/astral-sh/ruff/pull/17651
synced_at: 2026-01-10T19:03:00Z
```

# [`pycodestyle`] Fix duplicated diagnostic in `E712`

---

_Pull request opened by @LaBatata101 on 2025-04-26 23:09_



<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes a duplicated diagnostic when both sides of the operator are `True`.
Example:
```python
if True == True:
    ...
```

Let me know if there is another case where this could happen, I couldn't think of any.

Fixes #17582.

## Test Plan

Snapshot tests.


---

_Comment by @LaBatata101 on 2025-04-27 00:02_

Hmm, weird tests didn't fail locally.

---

_Comment by @Daverball on 2025-04-27 05:33_

@LaBatata101 You probably changed the comment in `E712.py` and forgot to rerun `cargo insta test` afterwards or you forgot to include your updated snapshot in your initial commit.

It used to read `# No duplicated error`, but you changed it to `# No duplicated diagnostic`, but the snapshot still contains the old comment. The snapshot needs to match exactly, so every time you change the source code, you also need to update the snapshot again.

---

_Label `rule` added by @MichaReiser on 2025-04-27 09:52_

---

_Comment by @LaBatata101 on 2025-04-27 14:01_

> @LaBatata101 You probably changed the comment in `E712.py` and forgot to rerun `cargo insta test` afterwards or you forgot to include your updated snapshot in your initial commit.
> 
> It used to read `# No duplicated error`, but you changed it to `# No duplicated diagnostic`, but the snapshot still contains the old comment. The snapshot needs to match exactly, so every time you change the source code, you also need to update the snapshot again.

Thanks!

---

_Comment by @github-actions[bot] on 2025-04-27 14:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-04-28 07:31_

---

_Merged by @MichaReiser on 2025-04-28 07:31_

---

_Closed by @MichaReiser on 2025-04-28 07:31_

---

_Label `bug` added by @MichaReiser on 2025-04-28 07:31_

---

_Branch deleted on 2025-04-30 20:55_

---
