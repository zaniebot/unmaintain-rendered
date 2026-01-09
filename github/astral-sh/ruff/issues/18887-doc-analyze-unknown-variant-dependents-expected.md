---
number: 18887
title: "doc(analyze): unknown variant `dependents`, expected `Dependencies` or `Dependents`"
type: issue
state: closed
author: chirizxc
labels:
  - documentation
  - analyze
assignees: []
created_at: 2025-06-23T10:12:32Z
updated_at: 2025-06-23T12:30:31Z
url: https://github.com/astral-sh/ruff/issues/18887
synced_at: 2026-01-07T13:12:16-06:00
---

# doc(analyze): unknown variant `dependents`, expected `Dependencies` or `Dependents`

---

_Issue opened by @chirizxc on 2025-06-23 10:12_

### Summary
doc - https://docs.astral.sh/ruff/settings/#analyze_direction

![Image](https://github.com/user-attachments/assets/939aa59b-ee67-481a-a48d-f51720f6b750)

.ruff.toml
```toml
[analyze]
direction = "dependents"
```
ruff check . => unknown variant `dependents`, expected `Dependencies` or `Dependents`


### Version

ruff 0.12.0 (87f0feb21 2025-06-17

---

_Label `documentation` added by @AlexWaygood on 2025-06-23 10:41_

---

_Label `analyze` added by @AlexWaygood on 2025-06-23 10:41_

---

_Comment by @MichaReiser on 2025-06-23 10:58_

Thank you. It's actually the documentation that's correct and the setting that uses the wrong cases. 

---

_Referenced in [astral-sh/ruff#18892](../../astral-sh/ruff/pulls/18892.md) on 2025-06-23 11:02_

---

_Closed by @MichaReiser on 2025-06-23 12:30_

---
