```yaml
number: 22492
title: "[ty] Handle playground file updates in the monaco editor onChange"
type: pull_request
state: open
author: RasmusNygren
labels:
  - playground
  - ty
assignees: []
base: main
head: fix-playground-completions
created_at: 2026-01-10T13:37:09Z
updated_at: 2026-01-12T08:26:25Z
url: https://github.com/astral-sh/ruff/pull/22492
synced_at: 2026-01-12T08:53:00Z
```

# [ty] Handle playground file updates in the monaco editor onChange

---

_Pull request opened by @RasmusNygren on 2026-01-10 13:37_

## Summary

Fixes https://github.com/astral-sh/ty/issues/2415 I think?

Previously, completions could be computed on stale file content because they were requested before the file objects were fully updated.

This became apparent when we configured `incomplete = True` as this causes completions to be re-fetched as more text is typed.

I don't really know if this is the best fix but it seems to work 🤷 


## Test Plan
Verifying that https://github.com/astral-sh/ty/issues/2415 does not reproduce and that the playground seems fine in general.


---

_Label `playground` added by @AlexWaygood on 2026-01-10 13:55_

---

_Marked ready for review by @RasmusNygren on 2026-01-10 13:55_

---

_Review requested from @MichaReiser by @AlexWaygood on 2026-01-10 13:56_

---

_Comment by @RasmusNygren on 2026-01-10 13:56_

cc @MichaReiser You probably have some input here :D

---

_Comment by @MichaReiser on 2026-01-10 14:13_

@AlexWaygood would you mind testing if this fixes the completions bug that you observed?

---

_Comment by @AlexWaygood on 2026-01-10 14:15_

sure!

---

_Comment by @AlexWaygood on 2026-01-10 14:36_

Yup, this fixes the bug for me! I can't review the typescript though 😆

---

_Label `ty` added by @AlexWaygood on 2026-01-12 08:26_

---
