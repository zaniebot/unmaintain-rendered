```yaml
number: 13285
title: Fix configuration inheritance for configurations specified in the LSP settings
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: micha/fix-server-configuration-extend
created_at: 2024-09-08T19:26:09Z
updated_at: 2024-09-09T18:46:41Z
url: https://github.com/astral-sh/ruff/pull/13285
synced_at: 2026-01-10T21:38:32Z
```

# Fix configuration inheritance for configurations specified in the LSP settings

---

_Pull request opened by @MichaReiser on 2024-09-08 19:26_

## Summary
This PR addresses a bug where the ruff LSP didn't follow a configuration's `extend` when the configuration is specified in the LSP settings.

Fixes https://github.com/astral-sh/ruff/issues/13261

## Test Plan

I created a workspace layout as described in https://github.com/astral-sh/ruff/issues/13261 and verified that the diagnostic isn't shown on main but is shown with this patch. 


---

_Review requested from @dhruvmanila by @MichaReiser on 2024-09-08 19:26_

---

_Label `bug` added by @MichaReiser on 2024-09-08 19:26_

---

_Label `server` added by @MichaReiser on 2024-09-08 19:26_

---

_Comment by @github-actions[bot] on 2024-09-08 19:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila approved on 2024-09-09 14:41_

Thanks!

---

_Merged by @MichaReiser on 2024-09-09 18:46_

---

_Closed by @MichaReiser on 2024-09-09 18:46_

---

_Branch deleted on 2024-09-09 18:46_

---
