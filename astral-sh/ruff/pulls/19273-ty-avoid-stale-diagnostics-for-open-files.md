```yaml
number: 19273
title: "[ty] Avoid stale diagnostics for open files diagnostic mode"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - server
  - ty
assignees: []
merged: true
base: main
head: dhruv/stale-diagnostic-open-files-mode
created_at: 2025-07-11T03:27:41Z
updated_at: 2025-07-11T10:59:18Z
url: https://github.com/astral-sh/ruff/pull/19273
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Avoid stale diagnostics for open files diagnostic mode

---

_Pull request opened by @dhruvmanila on 2025-07-11 03:27_

## Summary

This PR fixes a bug where in `openFilesOnly` diagnostic mode, VS Code wouldn't clean up the diagnostics even though the server asked it to by sending an empty publish diagnostics.

This is not the long-term solution but a quick fix. Ideally, the server would dynamically register for workspace diagnostics but that requires listening for `didChangeConfiguration` notification which I'm going to be working on with https://github.com/astral-sh/ty/issues/82.

## Test Plan

### Before

This uses the latest stable version of ty.

https://github.com/user-attachments/assets/0cc6c513-ccad-4955-a1b6-a0ee242119d6

### After

This uses the debug build of ty from this PR.

https://github.com/user-attachments/assets/e539d569-d852-46a9-bbfc-d54375127c62




---

_Review requested from @carljm by @dhruvmanila on 2025-07-11 03:27_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-07-11 03:27_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-07-11 03:27_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-07-11 03:27_

---

_Label `bug` added by @dhruvmanila on 2025-07-11 03:27_

---

_Review requested from @dcreager by @dhruvmanila on 2025-07-11 03:27_

---

_Label `server` added by @dhruvmanila on 2025-07-11 03:27_

---

_Label `ty` added by @dhruvmanila on 2025-07-11 03:27_

---

_Comment by @github-actions[bot] on 2025-07-11 03:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review request for @sharkdp removed by @sharkdp on 2025-07-11 06:07_

---

_@MichaReiser approved on 2025-07-11 07:13_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-11 08:19_

---

_Merged by @dhruvmanila on 2025-07-11 10:59_

---

_Closed by @dhruvmanila on 2025-07-11 10:59_

---

_Branch deleted on 2025-07-11 10:59_

---
