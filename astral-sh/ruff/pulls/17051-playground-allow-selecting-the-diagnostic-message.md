```yaml
number: 17051
title: "[playground] Allow selecting the diagnostic message"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
assignees: []
merged: true
base: main
head: micha/diagnostic-selection
created_at: 2025-03-28T20:52:54Z
updated_at: 2025-03-28T20:58:07Z
url: https://github.com/astral-sh/ruff/pull/17051
synced_at: 2026-01-10T19:40:36Z
```

# [playground] Allow selecting the diagnostic message

---

_Pull request opened by @MichaReiser on 2025-03-28 20:52_

## Summary

Allow selecting the diagnostic message so that the message can be copied (e.g. into an issue)

## Test Plan

<img width="1679" alt="Screenshot 2025-03-28 at 16 52 45" src="https://github.com/user-attachments/assets/06674d87-6c88-45d4-b46c-0bcb3e151996" />



---

_Label `playground` added by @MichaReiser on 2025-03-28 20:53_

---

_Review requested from @Copilot by @MichaReiser on 2025-03-28 20:53_

---

_@copilot-pull-request-reviewer[bot] reviewed on 2025-03-28 20:53_

## Pull Request Overview

This PR enables the selection of the diagnostic message so that it can be easily copied (such as when reporting an issue).  
- Updated the class name to include "select-text" in playground/ruff/src/Editor/Diagnostics.tsx.  
- Updated the class name to include "select-text" in playground/knot/src/Editor/Diagnostics.tsx.

### Reviewed Changes

Copilot reviewed 2 out of 2 changed files in this pull request and generated no comments.

| File                                                     | Description                                             |
| -------------------------------------------------------- | ------------------------------------------------------- |
| playground/ruff/src/Editor/Diagnostics.tsx               | Added "cursor-pointer select-text" to allow text copying |
| playground/knot/src/Editor/Diagnostics.tsx               | Added "cursor-pointer select-text" to allow text copying |





---

_Merged by @MichaReiser on 2025-03-28 20:58_

---

_Closed by @MichaReiser on 2025-03-28 20:58_

---

_Branch deleted on 2025-03-28 20:58_

---
