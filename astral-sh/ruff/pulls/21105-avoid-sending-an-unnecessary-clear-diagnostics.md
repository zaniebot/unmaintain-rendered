```yaml
number: 21105
title: "Avoid sending an unnecessary \"clear diagnostics\" message for clients supporting pull diagnostics"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - server
assignees: []
merged: true
base: main
head: capabilities-check-for-clear_diagnostics_for_document
created_at: 2025-10-28T05:40:36Z
updated_at: 2025-10-31T07:20:24Z
url: https://github.com/astral-sh/ruff/pull/21105
synced_at: 2026-01-12T15:57:16Z
```

# Avoid sending an unnecessary "clear diagnostics" message for clients supporting pull diagnostics

---

_@TaKO8Ki_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #20784

Move the capability check into `publish_diagnostics_for_document` and `clear_diagnostics_for_document`.

## Test Plan

<!-- How was it tested? -->

I have tested it with VSCode.

![output](https://github.com/user-attachments/assets/bba4815b-d714-4feb-ac3b-eea4cdb089c9)



---

_Comment by @github-actions[bot] on 2025-10-28 05:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `server` added by @MichaReiser on 2025-10-28 18:10_

---

_@MichaReiser approved on 2025-10-28 18:22_

Thank you

---

_Renamed from "[`ruff`] Add capabilities check for `clear_diagnostics_for_document`" to "Avoid sending an unnecessary "clear diagnostics" message for clients supporting pull diagnostics" by @MichaReiser on 2025-10-28 18:22_

---

_@MichaReiser approved on 2025-10-28 18:23_

Thank you

---

_Comment by @MichaReiser on 2025-10-28 18:24_

Ahh... airplane wifi at its best.

---

_Merged by @MichaReiser on 2025-10-28 18:24_

---

_Closed by @MichaReiser on 2025-10-28 18:24_

---

_Branch deleted on 2025-10-28 18:31_

---

_Comment by @MichaReiser on 2025-10-31 01:12_

This breaks notebook support. Notebooks only support publish diagnostics. I'll revert it for now

---

_Comment by @TaKO8Ki on 2025-10-31 07:20_

@MichaReiser Ok. I will create a pull request with necessary fixes.

---
