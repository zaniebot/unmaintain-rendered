---
number: 16043
title: bug in latest uv version installation in wsl ubuntu 24.04
type: issue
state: closed
author: Dr-Aniekan-Udo
labels:
  - bug
assignees: []
created_at: 2025-09-26T18:12:13Z
updated_at: 2025-09-27T18:14:37Z
url: https://github.com/astral-sh/uv/issues/16043
synced_at: 2026-01-10T01:26:02Z
---

# bug in latest uv version installation in wsl ubuntu 24.04

---

_Issue opened by @Dr-Aniekan-Udo on 2025-09-26 18:12_

### Summary

encounter abnormal error related to ssl when trying to install uv on ubuntu running in wsl. has uv running for one of the user, but trying to install system wide to work with sudo is problematic. symlinking to the current user might post issue when i update, hence the need to install system wide. issue is not network as browsing is working.

output with vpn:
downloading uv 0.8.22 x86_64-unknown-linux-gnu
curl: (92) HTTP/2 stream 1 was not closed cleanly: PROTOCOL_ERROR (err 1)
failed to download https://github.com/astral-sh/uv/releases/download/0.8.22/uv-x86_64-unknown-linux-gnu.tar.gz
this may be a standard network error, but it may also indicate
that uv's release process is not working. When in doubt
please feel free to open an issue!

output without vpn
curl: (28) SSL connection timeout

### Platform

Ubuntu 24.04

### Version

uv 0.8.22 x86_64

### Python version

Python 3.12.3

---

_Label `bug` added by @Dr-Aniekan-Udo on 2025-09-26 18:12_

---

_Comment by @charliermarsh on 2025-09-27 18:14_

Unfortunately I'm not sure how we can help here -- The cURL script just downloads and unzips the binary from GitHub. You may want to consider using a different installation method, if you can't access GitHub.

---

_Closed by @charliermarsh on 2025-09-27 18:14_

---
