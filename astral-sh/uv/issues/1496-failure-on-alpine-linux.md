```yaml
number: 1496
title: Failure on Alpine Linux
type: issue
state: closed
author: ilanschnell
labels:
  - duplicate
assignees: []
created_at: 2024-02-16T14:51:33Z
updated_at: 2024-02-16T14:54:49Z
url: https://github.com/astral-sh/uv/issues/1496
synced_at: 2026-01-12T15:58:28Z
```

# Failure on Alpine Linux

---

_@ilanschnell_

On the latest Alpine 3.19.1, I tried out `uv` with apk installed Python.  I get the following error:
```
# uv pip install bitarray
error: Failed to detect the operating system version: Couldn't detect either glibc version nor musl libc version, at least one of which is required
```
Here is some system information:
```
# cat /etc/os-release 
NAME="Alpine Linux"
ID=alpine
VERSION_ID=3.19.1
PRETTY_NAME="Alpine Linux v3.19"
HOME_URL="https://alpinelinux.org/"
BUG_REPORT_URL="https://gitlab.alpinelinux.org/alpine/aports/-/issues"
# ldd
musl libc (x86_64)
Version 1.2.4_git20230717
Dynamic Program Loader
Usage: /lib/ld-musl-x86_64.so.1 [options] [--] pathname
```


---

_Comment by @charliermarsh on 2024-02-16 14:52_

Thanks! I believe this is a duplicate of https://github.com/astral-sh/uv/issues/1427.

---

_Closed by @charliermarsh on 2024-02-16 14:52_

---

_Label `duplicate` added by @zanieb on 2024-02-16 14:54_

---
