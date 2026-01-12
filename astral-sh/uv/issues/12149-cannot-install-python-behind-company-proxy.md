```yaml
number: 12149
title: Cannot install python behind company proxy
type: issue
state: open
author: gaiazaff
labels:
  - question
assignees: []
created_at: 2025-03-13T11:00:14Z
updated_at: 2025-03-19T10:55:22Z
url: https://github.com/astral-sh/uv/issues/12149
synced_at: 2026-01-12T16:00:56Z
```

# Cannot install python behind company proxy

---

_@gaiazaff_

### Question

I have seen this mentioned in #8525 and #9747, but I still have issues with installing any python behind company proxy: uv retries multiple times, but then fails (I get "Failed to extract archive").

Should I assume that it is a company proxy issue, or is there anything I can do on my side?

### Platform

Windows

### Version

uv 0.5.25 (9c07c3fc5 2025-01-28)

---

_Label `question` added by @gaiazaff on 2025-03-13 11:00_

---

_Comment by @charliermarsh on 2025-03-13 12:59_

I don't think there's much we can do here unfortunately if you're continuing to see issues with the GitHub downloads.

---

_Comment by @notatallshaw on 2025-03-13 14:23_

Try downloading manually and see if it is blocked: https://github.com/astral-sh/python-build-standalone/releases/download/20250311/cpython-3.11.11+20250311-x86_64-pc-windows-msvc-install_only.tar.gz

The link redirects to a https://objects.githubusercontent.com/ link which may be blocked in your corporation. If it is blocked you need to decide whether it is worth raising this to your IT depeartment.

---

_Comment by @Chasarr on 2025-03-19 10:48_

It works fine on my Linux workstation here at my company, but not on Windows. I get the error:

```
Failed to download https://github.com/astral-sh/python-build-standalone/releases/downloa/20250317/cpython-3.10.16%2b20250317-x86_64-windows-msvc-install_only_stripped.tar.gz
```
It works fine to download it directly from PowerShell though.
```
Invoke-WebRequest https://github.com/astral-sh/python-build-standalone/releases/downloa/20250317/cpython-3.10.16%2b20250317-x86_64-windows-msvc-install_only_stripped.tar.gz
```

So the issue must lie with uv or any of its dependencies.

I have tried on uv version 0.6.7

---

_Comment by @gaiazaff on 2025-03-19 10:55_

thanks @charliermarsh  and @notatallshaw for your reply. 
The manual download worked fine in my case, so it was not a matter of  https://objects.githubusercontent.com/  being blocked in my case.

I have updated uv (to 0.6.6 and again to 0.6.8) and everything works, very confusing.

---
