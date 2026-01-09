---
number: 17018
title: "[red-knot playground] \"Python syntax errors\" are reported on TOML files"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - playground
  - ty
assignees: []
created_at: 2025-03-27T18:25:50Z
updated_at: 2025-03-27T19:45:05Z
url: https://github.com/astral-sh/ruff/issues/17018
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot playground] "Python syntax errors" are reported on TOML files

---

_Issue opened by @AlexWaygood on 2025-03-27 18:25_

### Summary

I added a `knot.toml` file in the playground because I forgot that it was meant to be `knot.json` in the playground. Some confusing "syntax errors" were emitted that indicated that the playground was trying to parse the TOML file as JSON!

Link: https://playknot.ruff.rs/2d989b5d-ffd4-4128-a7c1-4c8149a48399

Screenshot:

---

![Image](https://github.com/user-attachments/assets/af87420f-15b8-4442-9e2c-07074670cd91)

---

---

_Label `bug` added by @AlexWaygood on 2025-03-27 18:25_

---

_Label `playground` added by @AlexWaygood on 2025-03-27 18:25_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-27 18:25_

---

_Assigned to @MichaReiser by @AlexWaygood on 2025-03-27 18:25_

---

_Referenced in [astral-sh/ruff#17021](../../astral-sh/ruff/pulls/17021.md) on 2025-03-27 19:38_

---

_Closed by @MichaReiser on 2025-03-27 19:45_

---

_Closed by @MichaReiser on 2025-03-27 19:45_

---
