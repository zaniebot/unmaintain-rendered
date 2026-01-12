```yaml
number: 1776
title: "[completions] Rank symbols with class-literal types higher inside class declarations"
type: issue
state: open
author: AlexWaygood
labels:
  - server
  - completions
assignees: []
created_at: 2025-12-05T14:49:50Z
updated_at: 2025-12-05T14:49:50Z
url: https://github.com/astral-sh/ty/issues/1776
synced_at: 2026-01-12T15:54:25Z
```

# [completions] Rank symbols with class-literal types higher inside class declarations

---

_@AlexWaygood_

`NotImplementedError` and `NotADirectoryError` should be ranked higher than `NotImplemented` here, because it is invalid to inherit from `NotImplemented` -- `NotImplemented` is not a class

<img width="1358" height="768" alt="Image" src="https://github.com/user-attachments/assets/1b90f5d0-cc8f-4cea-b06a-886c1a38f33b" />

---

_Label `server` added by @AlexWaygood on 2025-12-05 14:49_

---

_Label `completions` added by @AlexWaygood on 2025-12-05 14:49_

---
