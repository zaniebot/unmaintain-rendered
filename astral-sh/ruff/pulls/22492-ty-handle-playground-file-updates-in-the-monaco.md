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
updated_at: 2026-01-13T08:50:58Z
url: https://github.com/astral-sh/ruff/pull/22492
synced_at: 2026-01-13T09:21:15Z
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

_@MichaReiser reviewed on 2026-01-13 08:48_

---

_Review comment by @MichaReiser on `playground/ty/src/Editor/Editor.tsx`:257 on 2026-01-13 08:48_

What's a bit surprising is that react-monaco wrapper does exactly this

https://github.com/react-monaco-editor/react-monaco-editor/blob/3062386185b0fee4fb2aa5002e5ef5fdedc05dbb/src/editor.tsx#L63-L67

I've to take a closer look why doing this here would work when doing the same in `onChange` does not

I suspect it has to do with `__prevent_trigger_change_event` but that would also mean that the issue really just is that we update the content out of order?

---

_@MichaReiser reviewed on 2026-01-13 08:50_

---

_Review comment by @MichaReiser on `playground/ty/src/Playground.tsx`:88 on 2026-01-13 08:50_

I wonder if the fix is to move the lines above the `dispatchFiles` action. 

---
