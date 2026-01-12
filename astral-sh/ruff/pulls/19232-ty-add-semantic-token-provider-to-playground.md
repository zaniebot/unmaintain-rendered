```yaml
number: 19232
title: "[ty] Add semantic token provider to playground"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/semantic-tokens-playground
created_at: 2025-07-09T12:19:28Z
updated_at: 2025-07-10T05:55:21Z
url: https://github.com/astral-sh/ruff/pull/19232
synced_at: 2026-01-12T15:56:34Z
```

# [ty] Add semantic token provider to playground

---

_@MichaReiser_

## Summary

Integrate the semantic token provider into the playground. 

I did remap some colors but I didn't assign colors to every supported token yet

## Test Plan

<img width="994" alt="Screenshot 2025-07-09 at 14 19 44" src="https://github.com/user-attachments/assets/0d31ffd6-7465-456b-86a1-8be60afe0c6d" />
<img width="994" alt="Screenshot 2025-07-09 at 14 19 40" src="https://github.com/user-attachments/assets/168ef5bc-b23d-49eb-8c96-9c5fd7590629" />



---

_Label `playground` added by @MichaReiser on 2025-07-09 12:19_

---

_Label `ty` added by @MichaReiser on 2025-07-09 12:19_

---

_Review requested from @UnboundVariable by @MichaReiser on 2025-07-09 12:20_

---

_Comment by @github-actions[bot] on 2025-07-09 12:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @UnboundVariable on `playground/shared/src/setupMonaco.tsx`:302 on 2025-07-10 05:47_

It looks like a lot of work to maintain the theme colors manually. IIRC, there are theme packs for monaco that work well on dark and light background.

---

_@UnboundVariable approved on 2025-07-10 05:48_

Thanks for doing this. LGTM.

---

_Marked ready for review by @MichaReiser on 2025-07-10 05:49_

---

_Review requested from @carljm by @MichaReiser on 2025-07-10 05:49_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-10 05:49_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-10 05:49_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-10 05:49_

---

_@MichaReiser reviewed on 2025-07-10 05:50_

---

_Review comment by @MichaReiser on `playground/shared/src/setupMonaco.tsx`:302 on 2025-07-10 05:50_

Yeah, I considered doing the same. It's a bit unfortunate that it's not possible to use the VS code themes directly

---

_Merged by @MichaReiser on 2025-07-10 05:50_

---

_Closed by @MichaReiser on 2025-07-10 05:50_

---

_Branch deleted on 2025-07-10 05:50_

---

_@UnboundVariable reviewed on 2025-07-10 05:55_

---

_Review comment by @UnboundVariable on `playground/shared/src/setupMonaco.tsx`:302 on 2025-07-10 05:55_

I think monaco offers some reasonable default themes, but maybe you're looking for something that's more customizable.

Here's [the code in the "pyright playground" code](https://github.com/erictraut/pyright-playground/blob/6bbe0cc1ea6825d7682cb25d144f503178473c80/client/MonacoEditor.tsx#L139) that specifies the default VS Code theme.

---
