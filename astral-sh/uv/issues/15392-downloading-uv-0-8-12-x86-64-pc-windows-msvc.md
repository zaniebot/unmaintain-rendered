```yaml
number: 15392
title: downloading uv 0.8.12 x86_64-pc-windows-msvc failed
type: issue
state: closed
author: fusioncid
labels:
  - bug
assignees: []
created_at: 2025-08-20T13:44:05Z
updated_at: 2025-08-20T14:02:36Z
url: https://github.com/astral-sh/uv/issues/15392
synced_at: 2026-01-12T16:02:09Z
```

# downloading uv 0.8.12 x86_64-pc-windows-msvc failed

---

_@fusioncid_

### Summary

Maybe wrong URL:  “https://github.com/astral-sh/uv/releases/download/0.8.12/**uv-x86_64-pc-windows-msvc.zip**”

Which should be:
                         https://github.com/astral-sh/uv/releases/download/0.8.12/**uv-i686-pc-windows-msvc.zip**
---------------------------------------------------------------
downloading uv 0.8.12 x86_64-pc-windows-msvc
curl: (35) schannel: next InitializeSecurityContext failed: CRYPT_E_NO_REVOCATION_CHECK (0x80092012) - failed to download https://github.com/astral-sh/uv/releases/download/0.8.12/uv-x86_64-pc-windows-msvc.zip
this may be a standard network error, but it may also indicate
that uv's release process is not working. When in doubt
please feel free to open an issue!

<img width="786" height="388" alt="Image" src="https://github.com/user-attachments/assets/e3d71e80-1388-44f0-80f5-555ce5a3d461" />

### Platform

win11 x86-64

### Version

uv 0.8.12

### Python version

_No response_

---

_Label `bug` added by @fusioncid on 2025-08-20 13:44_

---

_Comment by @zanieb on 2025-08-20 13:49_

I don't think you want the 32-bit Windows so I think the URL is right.

Does this fail on retry?

---

_Comment by @zanieb on 2025-08-20 13:49_

It looks like this usually happens due to security software? https://stackoverflow.com/questions/79443441/windows-11-curl-35-schannel-next-initializesecuritycontext-failed-crypt-e-n

---

_Comment by @fusioncid on 2025-08-20 14:02_

Thank you for your prompt guidance. It has been resolved @zanieb 

---

_Closed by @fusioncid on 2025-08-20 14:02_

---
