---
number: 13388
title: running uv add ./package should reinstall the local package, same as uv pip install
type: issue
state: closed
author: danra
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2025-05-11T19:27:45Z
updated_at: 2025-05-15T14:08:56Z
url: https://github.com/astral-sh/uv/issues/13388
synced_at: 2026-01-07T13:12:18-06:00
---

# running uv add ./package should reinstall the local package, same as uv pip install

---

_Issue opened by @danra on 2025-05-11 19:27_

### Summary

Following up on https://github.com/astral-sh/uv/issues/12038, wouldn't it make sense for `uv add ./<package>` to behave similar to `uv pip install ./package` and always reinstall the local package? Apparently, that's not currently the case.

### Platform

macOS

### Version

uv 0.7.3 (3c413f74b 2025-05-07)

### Python version

Python 3.13.1

---

_Label `bug` added by @danra on 2025-05-11 19:27_

---

_Label `enhancement` added by @konstin on 2025-05-12 09:27_

---

_Label `needs-decision` added by @konstin on 2025-05-12 09:27_

---

_Comment by @charliermarsh on 2025-05-13 03:02_

I think that makes sense, yeah.

---

_Label `needs-decision` removed by @charliermarsh on 2025-05-13 03:02_

---

_Label `bug` removed by @charliermarsh on 2025-05-13 03:03_

---

_Label `help wanted` added by @charliermarsh on 2025-05-13 03:03_

---

_Referenced in [astral-sh/uv#13462](../../astral-sh/uv/pulls/13462.md) on 2025-05-15 10:03_

---

_Closed by @zanieb on 2025-05-15 14:08_

---
