---
number: 12853
title: Sync Error
type: issue
state: closed
author: hczs
labels:
  - needs-mre
assignees: []
created_at: 2025-04-13T08:54:53Z
updated_at: 2025-04-14T12:12:57Z
url: https://github.com/astral-sh/uv/issues/12853
synced_at: 2026-01-07T13:12:18-06:00
---

# Sync Error

---

_Issue opened by @hczs on 2025-04-13 08:54_

### Summary

When i execute `uv sync --verbose`, it's had error
but i can access "https://github.com/astral-sh/python-build-standalone/releases/download/20250409/cpython-3.11.12%2B20250409-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz"
```shell
powercheng@powercheng:~/VscodeProjects/hello$ uv sync --verbose
DEBUG uv 0.6.14
DEBUG Found project root: `/home/powercheng/VscodeProjects/hello`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/home/powercheng/VscodeProjects/hello`
DEBUG Reading Python requests from version file at `/home/powercheng/VscodeProjects/hello/.python-version`
DEBUG Using Python request `3.11` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG Searching for Python 3.11 in managed installations or search path
DEBUG Searching for managed installations at `/home/powercheng/.local/share/uv/python`
DEBUG Found `cpython-3.10.12-linux-x86_64-gnu` at `/usr/bin/python3` (first executable in the search path)
DEBUG Skipping interpreter at `/usr/bin/python3` from first executable in the search path: does not satisfy request `3.11`
DEBUG Requested Python not found, checking for available download...
DEBUG Acquired lock for `/home/powercheng/.local/share/uv/python`
DEBUG Using request timeout of 30s
INFO Fetching requested Python...
DEBUG Transient request failure for https://github.com/astral-sh/python-build-standalone/releases/download/20250409/cpython-3.11.12%2B20250409-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz, retrying: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250409/cpython-3.11.12%2B20250409-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz)
  Caused by: operation timed out
DEBUG Transient request failure for https://github.com/astral-sh/python-build-standalone/releases/download/20250409/cpython-3.11.12%2B20250409-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz, retrying: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250409/cpython-3.11.12%2B20250409-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz)
  Caused by: operation timed out
DEBUG Transient request failure for https://github.com/astral-sh/python-build-standalone/releases/download/20250409/cpython-3.11.12%2B20250409-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz, retrying: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250409/cpython-3.11.12%2B20250409-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz)
  Caused by: operation timed out
DEBUG Transient request failure for https://github.com/astral-sh/python-build-standalone/releases/download/20250409/cpython-3.11.12%2B20250409-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz, retrying: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250409/cpython-3.11.12%2B20250409-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz)
  Caused by: client error (SendRequest)
  Caused by: connection error
  Caused by: peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof
DEBUG Transient failure while handling response for cpython-3.11.12-linux-x86_64-gnu; retrying...
DEBUG Transient request failure for https://github.com/astral-sh/python-build-standalone/releases/download/20250409/cpython-3.11.12%2B20250409-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz, retrying: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250409/cpython-3.11.12%2B20250409-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz)
  Caused by: operation timed out
DEBUG Transient request failure for https://github.com/astral-sh/python-build-standalone/releases/download/20250409/cpython-3.11.12%2B20250409-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz, retrying: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250409/cpython-3.11.12%2B20250409-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz)
  Caused by: operation timed out
```

### Platform

Zorin OS 17 amd64

### Version

0.6.14

### Python version

3.11

---

_Label `bug` added by @hczs on 2025-04-13 08:54_

---

_Comment by @charliermarsh on 2025-04-14 12:11_

Is this persistent? Are you able to access GitHub normally, or do you need to go through a mirror?

---

_Label `bug` removed by @charliermarsh on 2025-04-14 12:11_

---

_Label `needs-mre` added by @charliermarsh on 2025-04-14 12:11_

---

_Comment by @hczs on 2025-04-14 12:12_

> Is this persistent? Are you able to access GitHub normally, or do you need to go through a mirror?

Sorry, it's my Internet problem

---

_Closed by @hczs on 2025-04-14 12:12_

---
