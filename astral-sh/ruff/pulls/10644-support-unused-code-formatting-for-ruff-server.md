```yaml
number: 10644
title: "Support unused code formatting for `ruff server`"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/unused-code
created_at: 2024-03-28T06:35:37Z
updated_at: 2024-03-28T11:31:57Z
url: https://github.com/astral-sh/ruff/pull/10644
synced_at: 2026-01-10T22:47:02Z
```

# Support unused code formatting for `ruff server`

---

_Pull request opened by @snowsignal on 2024-03-28 06:35_

## Summary

Fixes #10589.

Code that violates `F401` or `F841` (in other words, unused variables or imports) should now appear greyed out or 'unused' in an editor.

## Test Plan

Put the following test code in a new file within the extension development host window:

```python
import math
def func():
   if False:
      unused = "<- this should be greyed out"
```

The following test code should have greyed out/unused import and variable names, like so:
<img width="294" alt="Screenshot 2024-03-28 at 4 23 18 AM" src="https://github.com/astral-sh/ruff/assets/19577865/e84a6e7a-49e2-4fed-9624-f8f9559e0837">


---

_Label `server` added by @snowsignal on 2024-03-28 06:35_

---

_Comment by @snowsignal on 2024-03-28 06:36_

(Note: this builds on top of the work in #10643 to prevent merge conflicts)

---

_Comment by @github-actions[bot] on 2024-03-28 06:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-03-28 08:01_

Looks good

As a Suggestion: consider pasting a screenshot as test plan. It allows me to see the change in action without starting the Editor myself

---

_@dhruvmanila approved on 2024-03-28 08:05_

---

_Comment by @snowsignal on 2024-03-28 11:24_

@MichaReiser I added a screenshot to the test plan 👍 

---

_Merged by @snowsignal on 2024-03-28 11:30_

---

_Closed by @snowsignal on 2024-03-28 11:30_

---

_Branch deleted on 2024-03-28 11:30_

---
