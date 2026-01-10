```yaml
number: 1370
title: No completions available on typevars
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - server
  - completions
assignees: []
created_at: 2025-10-16T15:30:11Z
updated_at: 2025-10-17T18:05:21Z
url: https://github.com/astral-sh/ty/issues/1370
synced_at: 2026-01-10T02:06:25Z
```

# No completions available on typevars

---

_Issue opened by @sharkdp on 2025-10-16 15:30_

### Summary

Consider

```py
def f[T: str](msg: T):
    msg.<CURSOR>
```

We don't show any completions (except for keyword completions which are not valid in this context):

<img width="585" height="154" alt="Image" src="https://github.com/user-attachments/assets/3c3a74eb-1a44-4686-9833-a4fa14a8d0a5" />

### Version

_No response_

---

_Label `server` added by @sharkdp on 2025-10-16 15:30_

---

_Label `completions` added by @sharkdp on 2025-10-16 15:30_

---

_Label `bug` added by @sharkdp on 2025-10-16 15:30_

---

_Comment by @sharkdp on 2025-10-16 15:32_

This is especially relevant for completions on `self`, whose type is a non-inferable typevar bound to the class, i.e. we don't get completions on `self.<CURSOR>`:

<img width="693" height="233" alt="Image" src="https://github.com/user-attachments/assets/0910f89b-fdf2-4f1a-b8e3-63ecdb5c7f3c" />

---

_Assigned to @sharkdp by @sharkdp on 2025-10-16 15:32_

---

_Closed by @sharkdp on 2025-10-17 18:05_

---
