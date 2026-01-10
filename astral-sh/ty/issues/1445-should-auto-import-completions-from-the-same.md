```yaml
number: 1445
title: Should auto-import completions from the same module be omitted?
type: issue
state: closed
author: sharkdp
labels:
  - server
  - completions
assignees: []
created_at: 2025-10-27T12:29:30Z
updated_at: 2025-10-29T13:13:50Z
url: https://github.com/astral-sh/ty/issues/1445
synced_at: 2026-01-10T02:06:25Z
```

# Should auto-import completions from the same module be omitted?

---

_Issue opened by @sharkdp on 2025-10-27 12:29_

<img width="604" height="291" alt="Image" src="https://github.com/user-attachments/assets/26e6edf3-f979-45d0-8e4d-cba527645ba6" />

The resulting code is not *wrong*, but it seems unlikely that someone would want that?

`main.py`:
```py
import main
class MyClass: ...

main.MyClass
```


---

_Label `completions` added by @sharkdp on 2025-10-27 12:29_

---

_Label `server` added by @AlexWaygood on 2025-10-27 12:31_

---

_Comment by @BurntSushi on 2025-10-27 12:50_

Yes they definitely should be. (I mean that they ought to be. The current code is not handling this case.)

---

_Assigned to @BurntSushi by @BurntSushi on 2025-10-27 12:50_

---

_Closed by @BurntSushi on 2025-10-29 13:13_

---
