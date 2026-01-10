```yaml
number: 832
title: Inconsistent syntax highlighting for keyword-only argument types
type: issue
state: closed
author: twoertwein
labels:
  - server
assignees: []
created_at: 2025-07-16T16:35:39Z
updated_at: 2025-07-18T07:02:25Z
url: https://github.com/astral-sh/ty/issues/832
synced_at: 2026-01-10T02:07:36Z
```

# Inconsistent syntax highlighting for keyword-only argument types

---

_Issue opened by @twoertwein on 2025-07-16 16:35_

### Summary

Types of keyword-only arguments are given a different color compared to types of "normal" arguments.

[playground example](https://play.ty.dev/ef9f6703-3fc6-476e-a5c3-a330be5161a4) same happens also in neovim.

### Version

0.0.1-alpha.14

---

_Label `server` added by @AlexWaygood on 2025-07-16 16:36_

---

_Comment by @MichaReiser on 2025-07-16 16:38_

Thanks. Are you seeing this in VS code too or only the playground?

---

_Comment by @carljm on 2025-07-16 16:56_

> Thanks. Are you seeing this in VS code too or only the playground?

OP mentioned seeing it in neovim, so it's not playground-only.

---

_Comment by @twoertwein on 2025-07-17 13:14_

in neovim, the names of keyword-only arguments are also colored differently (not just the types):

<img width="402" height="263" alt="Image" src="https://github.com/user-attachments/assets/167f066a-eb6b-49d8-accc-cd42b6054cf9" />

---

_Assigned to @UnboundVariable by @UnboundVariable on 2025-07-17 20:45_

---

_Closed by @UnboundVariable on 2025-07-18 07:02_

---
