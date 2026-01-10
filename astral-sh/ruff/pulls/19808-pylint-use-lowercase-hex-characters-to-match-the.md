```yaml
number: 19808
title: "[`pylint`] Use lowercase hex characters to match the formatter (`PLE2513`)"
type: pull_request
state: merged
author: ember91
labels:
  - fixes
assignees: []
merged: true
base: main
head: emibe/esc_hex
created_at: 2025-08-07T14:49:56Z
updated_at: 2025-08-08T12:32:03Z
url: https://github.com/astral-sh/ruff/pull/19808
synced_at: 2026-01-10T17:52:17Z
```

# [`pylint`] Use lowercase hex characters to match the formatter (`PLE2513`)

---

_Pull request opened by @ember91 on 2025-08-07 14:49_

PLE2513 --fix changes ESC and SUB to uppercase hexadecimal values such as \x1B while the formatter changes them to lowercase \x1b

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

_Review requested from @carljm by @ember91 on 2025-08-07 14:49_

---

_Review requested from @MichaReiser by @ember91 on 2025-08-07 14:49_

---

_Review requested from @sharkdp by @ember91 on 2025-08-07 14:49_

---

_Review requested from @dcreager by @ember91 on 2025-08-07 14:49_

---

_Label `fixes` added by @ntBre on 2025-08-07 15:24_

---

_@ntBre approved on 2025-08-07 15:28_

Looks good, thank you! And nice catch on the docs typo too. You just need to update a few more snapshots, and then this is good to go.

I'm also happy to push a commit for that if you prefer.

---

_Renamed from "[PLE2513] Make hex characters lowercase" to "[`pylint`] Use lowercase hex characters to match the formatter (`PLE2513`)" by @ntBre on 2025-08-07 15:29_

---

_Comment by @ember91 on 2025-08-07 17:54_

It's fine if you do. Thanks!

---

_Merged by @ntBre on 2025-08-08 12:25_

---

_Closed by @ntBre on 2025-08-08 12:25_

---

_Comment by @github-actions[bot] on 2025-08-08 12:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
