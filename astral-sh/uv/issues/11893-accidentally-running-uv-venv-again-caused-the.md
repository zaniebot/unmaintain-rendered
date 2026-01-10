---
number: 11893
title: Accidentally running uv venv again caused the .venv folder to be deleted
type: issue
state: closed
author: qingchunyy
labels:
  - duplicate
  - question
assignees: []
created_at: 2025-03-02T15:05:22Z
updated_at: 2025-03-02T15:20:21Z
url: https://github.com/astral-sh/uv/issues/11893
synced_at: 2026-01-10T01:25:12Z
---

# Accidentally running uv venv again caused the .venv folder to be deleted

---

_Issue opened by @qingchunyy on 2025-03-02 15:05_

### Question

I accidentally ran uv venv again and it deleted my entire .venv folder, my internet is only 1mb per second and my dependencies took a long time to download I'm not sure if there's any of my user data in there, is it possible to have UV deleted files deleted to the recycle bin or to make a prompt to choose whether or not to delete them? :(

### Platform

Windows 10

### Version

0.6.2

---

_Label `question` added by @qingchunyy on 2025-03-02 15:05_

---

_Comment by @zanieb on 2025-03-02 15:08_

Sorry about that. We are considering changing this behavior, see https://github.com/astral-sh/uv/issues/1472

---

_Label `duplicate` added by @zanieb on 2025-03-02 15:08_

---

_Comment by @qingchunyy on 2025-03-02 15:13_

> Sorry about that. We are considering changing this behavior, see [#1472](https://github.com/astral-sh/uv/issues/1472)

Okay. Thank you

---

_Closed by @qingchunyy on 2025-03-02 15:13_

---
