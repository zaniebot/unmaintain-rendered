---
number: 12871
title: UV does not use the /etc/ssl/certs/ca-certificates.crt
type: issue
state: closed
author: niruiyu
labels:
  - question
assignees: []
created_at: 2025-04-14T07:12:06Z
updated_at: 2025-04-14T12:14:53Z
url: https://github.com/astral-sh/uv/issues/12871
synced_at: 2026-01-07T13:12:18-06:00
---

# UV does not use the /etc/ssl/certs/ca-certificates.crt

---

_Issue opened by @niruiyu on 2025-04-14 07:12_

### Summary

Write a.py with following code:
```python
import certifi
print(certifi.where())
```
run `uv run a.py` the output shows `/etc/ssl/certs/ca-certificates.crt`

But if modifying a.py with `uv add --script a.py 'requests'`

```python
# /// script
# requires-python = ">=3.12"
# dependencies = [
#     "requests",
# ]
# ///
import certifi
print(certifi.where())
```

run `uv run a.py` the output shows `/home/ray/.cache/uv/environments-v2/a-d1dc1de19e825f16/lib/python3.12/site-packages/certifi/cacert.pem`

The bug caused my own script cannot use Kerberos auth to retrieve the assets in the server.

### Platform

Ubuntu 24.04.2 LTS

### Version

uv 0.6.14

### Python version

Python 3.12.3

---

_Label `bug` added by @niruiyu on 2025-04-14 07:12_

---

_Comment by @konstin on 2025-04-14 07:17_

This is behavior by requests/certifi and not by uv, this needs to be configured in requests to use the system certificates instead of certifi's vendored certificates.

---

_Label `bug` removed by @konstin on 2025-04-14 07:17_

---

_Label `question` added by @konstin on 2025-04-14 07:17_

---

_Closed by @charliermarsh on 2025-04-14 12:14_

---
