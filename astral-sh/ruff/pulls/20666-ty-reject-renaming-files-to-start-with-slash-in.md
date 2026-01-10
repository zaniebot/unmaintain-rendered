```yaml
number: 20666
title: "[`ty`] Reject renaming files to start with slash in Playground"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - playground
assignees: []
merged: true
base: main
head: playground-leading-slash-filenames
created_at: 2025-10-01T10:07:08Z
updated_at: 2025-10-01T13:54:28Z
url: https://github.com/astral-sh/ruff/pull/20666
synced_at: 2026-01-10T17:40:28Z
```

# [`ty`] Reject renaming files to start with slash in Playground

---

_Pull request opened by @TaKO8Ki on 2025-10-01 10:07_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes https://github.com/astral-sh/ty/issues/1282

Block renaming to names starting with `/`, surfacing a clear error instead of crashing the playground.

## Test Plan

<!-- How was it tested? -->

https://github.com/user-attachments/assets/94848f31-9fcf-46b7-895e-f468e5030104

---

_Review comment by @MichaReiser on `playground/ty/src/Playground.tsx`:100 on 2025-10-01 11:06_

Does the error message get cleared at some point? 

---

_@MichaReiser reviewed on 2025-10-01 11:06_

---

_@TaKO8Ki reviewed on 2025-10-01 11:21_

---

_Review comment by @TaKO8Ki on `playground/ty/src/Playground.tsx`:100 on 2025-10-01 11:21_

As shown in the video, the error message disappears as soon as you type anything. Actions like creating a file don’t dismiss it, and this behavior is the same for other error messages as well.

https://github.com/user-attachments/assets/adad7437-bf7b-40fb-ac75-aecfed51318d



---

_Label `playground` added by @MichaReiser on 2025-10-01 13:54_

---

_@MichaReiser approved on 2025-10-01 13:54_

---

_Merged by @MichaReiser on 2025-10-01 13:54_

---

_Closed by @MichaReiser on 2025-10-01 13:54_

---
