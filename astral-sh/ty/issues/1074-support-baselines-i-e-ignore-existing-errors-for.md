```yaml
number: 1074
title: Support baselines, i.e., ignore existing errors for incremental adoption
type: issue
state: open
author: larroy
labels:
  - wish
assignees: []
created_at: 2025-08-21T05:15:35Z
updated_at: 2025-11-18T16:10:36Z
url: https://github.com/astral-sh/ty/issues/1074
synced_at: 2026-01-12T15:54:24Z
```

# Support baselines, i.e., ignore existing errors for incremental adoption

---

_@larroy_

### Question

To add type checking to existing projects is desirable to be able to ignore existing issues. TY should accept a file with current failed diagnostics to be able to add ty to existing projects.

### Version

_No response_

---

_Label `question` added by @larroy on 2025-08-21 05:15_

---

_Renamed from "Add ty ignoring existing issues" to "Add baseline support" by @MichaReiser on 2025-08-21 06:10_

---

_Label `question` removed by @MichaReiser on 2025-08-21 06:10_

---

_Label `wish` added by @MichaReiser on 2025-08-21 06:10_

---

_Comment by @MichaReiser on 2025-08-21 06:10_

Same as https://github.com/astral-sh/ruff/issues/1149 but for ty

---

_Renamed from "Add baseline support" to "Support baselines, i.e., ignore existing errors for incremental adoption" by @MichaReiser on 2025-08-21 06:10_

---

_Comment by @KotlinIsland on 2025-09-22 02:34_

basedpyright has had a lot of success with it's baseline functionality: https://docs.basedpyright.com/latest/benefits-over-pyright/baseline/ (basedmypy too)

for the lsp, baselined diagnostics are reported with the `hint` level:

<img width="571" height="126" alt="Image" src="https://github.com/user-attachments/assets/e941197a-8607-411c-acd1-2cab01069e84" />

this reduces visual noise while also providing an easy way to understand what is happening

and an action to write:

<img width="608" height="67" alt="Image" src="https://github.com/user-attachments/assets/4f648b55-a63f-494b-8c7f-b64a0db10688" />

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-15 01:58_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
