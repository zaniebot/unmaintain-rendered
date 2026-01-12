```yaml
number: 17355
title: "Flag `--native-tls` still shows UnknownIssuer on windows."
type: issue
state: open
author: Tennyleaz
labels:
  - bug
assignees: []
created_at: 2026-01-08T01:00:24Z
updated_at: 2026-01-09T13:23:00Z
url: https://github.com/astral-sh/uv/issues/17355
synced_at: 2026-01-12T16:02:49Z
```

# Flag `--native-tls` still shows UnknownIssuer on windows.

---

_@Tennyleaz_

### Summary

In a cooperation network, my IT installed company certificate as trusted root CA on the windows PC.
Even using `--native-tls` flag, uv still shows UnknownIssuer error, unless I use `--allow-insecure-host pypi.org`.
The error output:
```
PS C:\Users\tenny_lu> uv tool install pycowsay
⠇ Resolving dependencies...                                                                                               × Request failed after 3 retries
  ├─▶ Failed to fetch: `https://pypi.org/simple/pycowsay/`
  ├─▶ error sending request for url (https://pypi.org/simple/pycowsay/)
  ├─▶ client error (Connect)
  ╰─▶ invalid peer certificate: UnknownIssuer
  help: Consider enabling use of system TLS certificates with the `--native-tls` command-line flag
PS C:\Users\tenny_lu> uv tool install pycowsay --native-tls
⠧ Resolving dependencies...                                                                                             error: Request failed after 3 retries
  Caused by: Failed to fetch: `https://pypi.org/simple/pycowsay/`
  Caused by: error sending request for url (https://pypi.org/simple/pycowsay/)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
```

But plain pip still works out of the box:
```
PS C:\Users\tenny_lu> python -m pip install pycowsay
Defaulting to user installation because normal site-packages is not writeable
Collecting pycowsay
  Downloading pycowsay-0.0.0.2-py3-none-any.whl.metadata (965 bytes)
Downloading pycowsay-0.0.0.2-py3-none-any.whl (4.0 kB)
Installing collected packages: pycowsay
Successfully installed pycowsay-0.0.0.2
```

On the same network, a linux PC does not have this problem. The output is like:
```
tenny@ubuntu-tenny01:~$ uv tool install pycowsay
Resolved 1 package in 589ms
Prepared 1 package in 121ms
Installed 1 package in 9ms
 + pycowsay==0.0.0.2
Installed 1 executable: pycowsay
```

### Platform

Windows 11 x64

### Version

uv 0.9.22

### Python version

Python 3.13.11 (Installed from python website)

---

_Label `bug` added by @Tennyleaz on 2026-01-08 01:00_

---

_Comment by @LoukasPap on 2026-01-08 01:18_

Have you tried setting it as an environment variable `UV_NATIVE_TLS=true`and then running it?

---

_Comment by @NMertsch on 2026-01-09 13:23_

Possible duplicate of #9243. I hope it will be resolved soon, we also use `allow-insecure-host` because of this.

---
