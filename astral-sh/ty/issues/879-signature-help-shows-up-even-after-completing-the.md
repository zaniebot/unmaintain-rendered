```yaml
number: 879
title: Signature help shows up even after completing the call
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - server
assignees: []
created_at: 2025-07-24T04:28:46Z
updated_at: 2025-07-25T04:04:24Z
url: https://github.com/astral-sh/ty/issues/879
synced_at: 2026-01-10T02:06:24Z
```

# Signature help shows up even after completing the call

---

_Issue opened by @dhruvmanila on 2025-07-24 04:28_

### Summary

Playground code:

```py
def foo(x: int, y: str) -> None: ...

foo(1, "")
```

The signature help shows up after _moving_ the cursor after the closing parentheses (`)`) that ends the call. The closing parenthesis was added automatically when I added the opening parenthesis but I explicitly typed <kbd>)</kbd> which _moved_ the cursor beyond the closing parenthesis. And, at this point, the signature help was shown which highlighted the first parameter.

Although, it's interesting that adding the closing parenthesis manually doesn't trigger the signature help. I haven't tried this out in an editor though.

https://github.com/user-attachments/assets/45ae4d17-64b5-43a7-b031-0470ce4806ab

### Version

e0149cd9f

---

_Label `bug` added by @dhruvmanila on 2025-07-24 04:28_

---

_Label `server` added by @dhruvmanila on 2025-07-24 04:28_

---

_Assigned to @UnboundVariable by @UnboundVariable on 2025-07-24 23:26_

---

_Comment by @UnboundVariable on 2025-07-24 23:27_

Thanks for reporting. I've checked in a fix.

---

_Closed by @dhruvmanila on 2025-07-25 04:04_

---
