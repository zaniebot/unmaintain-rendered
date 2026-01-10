---
number: 1252
title: Warn about yanked package in pip install
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2024-02-05T16:56:36Z
updated_at: 2024-02-05T17:15:46Z
url: https://github.com/astral-sh/uv/issues/1252
synced_at: 2026-01-10T01:23:05Z
---

# Warn about yanked package in pip install

---

_Issue opened by @konstin on 2024-02-05 16:56_

`puffin pip install` is currently missing warnings for yanked packages

```
$ puffin pip install "colorama==0.4.2" --no-cache
Resolved 1 package in 110ms
Downloaded 1 package in 18ms
Installed 1 package in 16ms
 + colorama==0.4.2
````

---

_Label `bug` added by @konstin on 2024-02-05 16:56_

---

_Comment by @charliermarsh on 2024-02-05 16:59_

Weird!

---

_Added to milestone `Initial release` by @charliermarsh on 2024-02-05 16:59_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-05 17:04_

---

_Comment by @charliermarsh on 2024-02-05 17:10_

Fixing.

---

_Referenced in [astral-sh/uv#1253](../../astral-sh/uv/pulls/1253.md) on 2024-02-05 17:11_

---

_Closed by @charliermarsh on 2024-02-05 17:15_

---
