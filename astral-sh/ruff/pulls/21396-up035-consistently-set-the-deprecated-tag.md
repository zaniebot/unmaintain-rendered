```yaml
number: 21396
title: "`UP035`: Consistently set the deprecated tag"
type: pull_request
state: merged
author: 11happy
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: add_diagnostic_tag
created_at: 2025-11-12T05:58:53Z
updated_at: 2025-11-12T07:17:29Z
url: https://github.com/astral-sh/ruff/pull/21396
synced_at: 2026-01-12T15:57:23Z
```

# `UP035`: Consistently set the deprecated tag

---

_@11happy_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR fixes #21333 

## Test Plan

<!-- How was it tested? -->
Before:
<img width="613" height="194" alt="Screenshot from 2025-11-12 11-23-41" src="https://github.com/user-attachments/assets/45e68870-229d-48c3-ae68-dbd3998c1fd1" />
After:
<img width="613" height="194" alt="Screenshot from 2025-11-12 11-24-16" src="https://github.com/user-attachments/assets/0808405b-9ac1-4c43-a44d-3edd79ac6f7b" />

Settings.json configured with local release binary:
<img width="613" height="194" alt="image" src="https://github.com/user-attachments/assets/aaca00a1-0ed3-4a6a-a017-2602ac913acc" />



---

_Comment by @astral-sh-bot[bot] on 2025-11-12 06:09_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `bug` added by @MichaReiser on 2025-11-12 07:16_

---

_Label `server` added by @MichaReiser on 2025-11-12 07:16_

---

_Renamed from "fix: add deprecated_tag for fix with renames" to "`UP035`: Consistently set the deprecated tag" by @MichaReiser on 2025-11-12 07:17_

---

_@MichaReiser approved on 2025-11-12 07:17_

Thank you

---

_Merged by @MichaReiser on 2025-11-12 07:17_

---

_Closed by @MichaReiser on 2025-11-12 07:17_

---
