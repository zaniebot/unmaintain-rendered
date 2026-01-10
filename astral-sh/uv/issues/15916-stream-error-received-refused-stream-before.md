---
number: 15916
title: "Stream error received: refused stream before processing any application logic"
type: issue
state: closed
author: konstin
labels:
  - ci-flake
assignees: []
created_at: 2025-09-17T17:40:42Z
updated_at: 2025-10-09T13:53:16Z
url: https://github.com/astral-sh/uv/issues/15916
synced_at: 2026-01-10T01:26:01Z
---

# Stream error received: refused stream before processing any application logic

---

_Issue opened by @konstin on 2025-09-17 17:40_

Streaming downloads can fail with a new error: https://github.com/astral-sh/uv/actions/runs/17805600726/job/50616507485

```
    error: Failed to install cpython-3.11.8-linux-x86_64-gnu
      Caused by: Failed to download https://github.com/astral-sh/python-build-standalone/releases/download/20240224/cpython-3.11.8%2B20240224-x86_64-unknown-linux-gnu-install_only.tar.gz
      Caused by: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20240224/cpython-3.11.8%2B20240224-x86_64-unknown-linux-gnu-install_only.tar.gz)
      Caused by: client error (SendRequest)
      Caused by: http2 error
      Caused by: stream error received: refused stream before processing any application logic
    error: Failed to install cpython-3.12.8-linux-x86_64-gnu
      Caused by: Failed to download https://github.com/astral-sh/python-build-standalone/releases/download/20250115/cpython-3.12.8%2B20250115-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
      Caused by: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250115/cpython-3.12.8%2B20250115-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz)
      Caused by: client error (SendRequest)
      Caused by: http2 error
      Caused by: stream error received: refused stream before processing any application logic
    error: Failed to install cpython-3.13.1-linux-x86_64-gnu
      Caused by: Failed to download https://github.com/astral-sh/python-build-standalone/releases/download/20250115/cpython-3.13.1%2B20250115-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
      Caused by: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250115/cpython-3.13.1%2B20250115-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz)
      Caused by: client error (SendRequest)
      Caused by: http2 error
      Caused by: stream error received: refused stream before processing any application logic
```

---

_Label `ci-flake` added by @zanieb on 2025-09-17 18:41_

---

_Renamed from "CI flake: stream error received: refused stream before processing any application logic" to "Stream error received: refused stream before processing any application logic" by @zanieb on 2025-09-17 18:41_

---

_Referenced in [seanmonstar/reqwest#2819](../../seanmonstar/reqwest/issues/2819.md) on 2025-09-18 07:21_

---

_Referenced in [astral-sh/uv#16038](../../astral-sh/uv/pulls/16038.md) on 2025-09-26 12:36_

---

_Closed by @konstin on 2025-10-09 13:53_

---
