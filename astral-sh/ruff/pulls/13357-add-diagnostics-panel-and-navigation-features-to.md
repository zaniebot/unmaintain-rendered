```yaml
number: 13357
title: Add diagnostics panel and navigation features to playground
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
assignees: []
merged: true
base: main
head: micha/playground-diagnostics
created_at: 2024-09-15T13:28:01Z
updated_at: 2024-09-16T07:34:47Z
url: https://github.com/astral-sh/ruff/pull/13357
synced_at: 2026-01-10T21:30:32Z
```

# Add diagnostics panel and navigation features to playground

---

_Pull request opened by @MichaReiser on 2024-09-15 13:28_

## Summary

This PR makes a few UX improvements:

1. Add a diagnostic panel. Syntax errors can sometimes be difficult to spot. A dedicated diagnostics panel makes reviewing all diagnostics in a file easier.
2. Add a `range` to `code` navigation. Clicking the range in the tokens or AST panel reveals the relevant source code.
3. Add a "reveal node" option that reveals the enclosing node or token

The range identification could be better. It uses a Regex to find the `x..y` ranges
without excluding occurrences in strings. I think that's fine. A range in a string
would be clickable, but it doesn't crash the playground. It either navigates to a bogus 
position or just does nothing.

## Test Plan


https://github.com/user-attachments/assets/4f16c0f9-02ed-4807-96f4-9afa07b113df



---

_Review requested from @charliermarsh by @MichaReiser on 2024-09-15 13:29_

---

_Label `playground` added by @MichaReiser on 2024-09-15 13:29_

---

_Review comment by @charliermarsh on `playground/src/Editor/SourceEditor.tsx`:5 on 2024-09-15 17:22_

Intentional?

---

_@charliermarsh reviewed on 2024-09-15 17:22_

---

_@charliermarsh approved on 2024-09-15 17:23_

---

_Merged by @MichaReiser on 2024-09-16 07:34_

---

_Closed by @MichaReiser on 2024-09-16 07:34_

---

_Branch deleted on 2024-09-16 07:34_

---
