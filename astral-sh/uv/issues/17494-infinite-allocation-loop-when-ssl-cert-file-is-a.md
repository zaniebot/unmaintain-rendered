```yaml
number: 17494
title: Infinite allocation loop when SSL_CERT_FILE is a directory
type: issue
state: closed
author: pieterdavid
labels:
  - bug
assignees: []
created_at: 2026-01-15T18:59:50Z
updated_at: 2026-01-16T13:12:56Z
url: https://github.com/astral-sh/uv/issues/17494
synced_at: 2026-01-16T13:57:15Z
```

# Infinite allocation loop when SSL_CERT_FILE is a directory

---

_@pieterdavid_

### Summary

To reproduce (note: kill the process before it freezes your machine):
```bash
% uv init --bare
% mkdir empty-dir
% SSL_CERT_FILE=./empty-dir uv venv --verbose
```
After a few lines of output the process starts allocating more and more memory, until the RAM and swap file limits of WSL are reached and it is killed.
The last lines of output are these:
```
[...]
DEBUG Using Python request `>=3.13` from `requires-python` metadata
DEBUG Using request timeout of 30s
```
Putting a directory for SSL_CERT_FILE is obviously a misconfiguration, but since there is a check to ignore the value in case the path doesn't exist, it would be nice if it could do the same in case it's a directory instead of a file.

### Platform

Archlinux on WSL2

### Version

uv 0.9.25 (38fcac0f3 2026-01-13)

### Python version

Python 3.13.11

---

_Label `bug` added by @pieterdavid on 2026-01-15 18:59_

---

_Comment by @zanieb on 2026-01-15 20:56_

Thank you!

---

_Comment by @zanieb on 2026-01-15 22:13_

It looks like the root cause is at https://github.com/rustls/pki-types/blob/85522565adf28a76909ea17dd2759b6046eb2bed/src/pem.rs#L50-L56

---

_Closed by @zanieb on 2026-01-16 13:12_

---
