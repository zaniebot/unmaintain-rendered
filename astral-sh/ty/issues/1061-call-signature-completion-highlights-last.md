```yaml
number: 1061
title: "Call signature completion highlights last argument after `)`"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - server
assignees: []
created_at: 2025-08-20T13:19:52Z
updated_at: 2025-08-22T14:09:23Z
url: https://github.com/astral-sh/ty/issues/1061
synced_at: 2026-01-12T15:54:24Z
```

# Call signature completion highlights last argument after `)`

---

_@MichaReiser_

### Summary

Using `print("hello"` as an example. I would expect the signature completion to close once a type `)` but, instead, ty highlights the separator parameter as the active parameter:

https://github.com/user-attachments/assets/b475f8cb-c5bd-4439-b1ec-271e2a41f31e


In comparison, pyright does close the signature help when I type the `)`.

https://github.com/user-attachments/assets/79a9cff0-1247-4377-8d2c-02e199a77da7
 




### Version

_No response_

---

_Label `bug` added by @MichaReiser on 2025-08-20 13:19_

---

_Label `server` added by @MichaReiser on 2025-08-20 13:19_

---

_Closed by @MichaReiser on 2025-08-22 14:09_

---
