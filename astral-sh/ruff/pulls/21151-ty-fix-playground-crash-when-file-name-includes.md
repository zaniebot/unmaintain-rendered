```yaml
number: 21151
title: "[ty] Fix playground crash when file name includes path separator"
type: pull_request
state: merged
author: sharkdp
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: david/fix-playground-crash
created_at: 2025-10-30T21:32:14Z
updated_at: 2025-11-04T17:36:39Z
url: https://github.com/astral-sh/ruff/pull/21151
synced_at: 2026-01-12T15:57:17Z
```

# [ty] Fix playground crash when file name includes path separator

---

_@sharkdp_

_No description provided._

---

_Label `playground` added by @sharkdp on 2025-10-30 21:32_

---

_Label `ty` added by @sharkdp on 2025-10-30 21:32_

---

_Marked ready for review by @sharkdp on 2025-11-04 14:04_

---

_Review comment by @MichaReiser on `playground/ty/src/Editor/SecondaryPanel.tsx`:106 on 2025-11-04 14:28_

To be less dependent on pyodide:

We could set the home path when loading pyodide, see https://pyodide.org/en/stable/usage/api/js-api.html#exports.PyodideConfig.env

---

_@MichaReiser approved on 2025-11-04 14:28_

Thank you

---

_@sharkdp reviewed on 2025-11-04 15:36_

---

_Review comment by @sharkdp on `playground/ty/src/Editor/SecondaryPanel.tsx`:106 on 2025-11-04 15:36_

Like this?

---

_@MichaReiser reviewed on 2025-11-04 16:48_

---

_Review comment by @MichaReiser on `playground/ty/src/Editor/SecondaryPanel.tsx`:106 on 2025-11-04 16:48_

yes

---

_Merged by @sharkdp on 2025-11-04 17:36_

---

_Closed by @sharkdp on 2025-11-04 17:36_

---

_Branch deleted on 2025-11-04 17:36_

---
