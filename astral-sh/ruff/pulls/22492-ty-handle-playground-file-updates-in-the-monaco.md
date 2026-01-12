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
updated_at: 2026-01-12T17:54:59Z
url: https://github.com/astral-sh/ruff/pull/22492
synced_at: 2026-01-12T18:23:35Z
```

# [ty] Handle playground file updates in the monaco editor onChange

---

_@RasmusNygren_

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

_Comment by @MichaReiser on 2026-01-12 17:26_

I hope to get to reviewing this tomorrow. One question from skimming the code: Could we use the same mechanism for setting changes or what's the reason that settings are handled differently?

---

_Comment by @RasmusNygren on 2026-01-12 17:54_

> I hope to get to reviewing this tomorrow. One question from skimming the code: Could we use the same mechanism for setting changes or what's the reason that settings are handled differently?

We could, but we update the options in a few different scenarios in Playground.tsx and I'm not sure it's an improvement to specifically handle the settings changes when a file is modified in the editor while the rest still has to remain in the playground.

---
