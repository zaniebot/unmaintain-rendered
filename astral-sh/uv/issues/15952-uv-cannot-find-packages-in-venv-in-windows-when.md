---
number: 15952
title: uv cannot find packages in venv in windows when cache dir was changed
type: issue
state: closed
author: Hoshino-Yumetsuki
labels:
  - needs-mre
assignees: []
created_at: 2025-09-19T17:02:23Z
updated_at: 2025-09-23T12:12:34Z
url: https://github.com/astral-sh/uv/issues/15952
synced_at: 2026-01-10T01:26:01Z
---

# uv cannot find packages in venv in windows when cache dir was changed

---

_Issue opened by @Hoshino-Yumetsuki on 2025-09-19 17:02_

### Summary

When I change uv`s cache dir with using UV_CACHE_DIR, uv cannot find packages was installed in venv. It can solve when I remove the UV_CACHE_DIR environment variable.

### Platform

Windows11

### Version

0.8.18 (c4c47814a 2025-09-17)

### Python version

python 3.13

---

_Label `bug` added by @Hoshino-Yumetsuki on 2025-09-19 17:02_

---

_Comment by @konstin on 2025-09-23 12:05_

Can you share a reproducible example? (https://github.com/astral-sh/uv/issues/9452)

---

_Label `bug` removed by @konstin on 2025-09-23 12:05_

---

_Label `needs-mre` added by @konstin on 2025-09-23 12:05_

---

_Comment by @Hoshino-Yumetsuki on 2025-09-23 12:12_

It's a bit strange, I can't reproduce the problem right now.

---

_Closed by @Hoshino-Yumetsuki on 2025-09-23 12:12_

---
