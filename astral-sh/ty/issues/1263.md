```yaml
number: 1263
title: Pressing esc in the playground reverts file contents
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - playground
assignees: []
created_at: 2025-09-26T17:53:47Z
updated_at: 2025-11-12T07:54:16Z
url: https://github.com/astral-sh/ty/issues/1263
synced_at: 2026-01-10T02:06:25Z
```

# Pressing esc in the playground reverts file contents

---

_Issue opened by @MeGaGiGaGon on 2025-09-26 17:53_

### Summary

In the playground, if you edit one file, then make a new file, edit it, and press escape, the contents will revert to those of the second-to-last edited file. This is very annoying for me since my muscle memory is to use esc to close autocomplete popups I don't need, so I run into this very often. Demonstration video using on-screen keyboard attached.

https://github.com/user-attachments/assets/71449ded-5c8c-4c0d-86e0-4f1f7995179d

### Version

playground (6b3c493cf)

---

_Comment by @MichaReiser on 2025-09-26 18:01_

Huh, that's funny. I'm not sure why Monaco thinks it should go back to the previously selected file when clicking Esc.

---

_Label `bug` added by @MichaReiser on 2025-09-26 18:02_

---

_Label `playground` added by @MichaReiser on 2025-09-26 18:02_

---

_Closed by @MichaReiser on 2025-11-12 07:54_

---
