```yaml
number: 1908
title: Fix uv-created venv detection
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/fix-venv-creator-detection
created_at: 2024-02-23T10:54:00Z
updated_at: 2024-02-23T17:11:23Z
url: https://github.com/astral-sh/uv/pull/1908
synced_at: 2026-01-10T14:54:43Z
```

# Fix uv-created venv detection

---

_Pull request opened by @konstin on 2024-02-23 10:54_

Read the key read for uv from `pyenv.cfg` from `gourgeist` instead of `uv`. I missed that we're also reading pyenv.cfg when reviewing #1852. We could check for gourgeist for backwards compatibility, but i think it's fine this way.

---

_Label `bug` added by @konstin on 2024-02-23 10:54_

---

_@BurntSushi approved on 2024-02-23 12:35_

Nice!

Out of curiosity, what impact was this bug having at an end-user level? (I'm just trying to understand the context of the change a little more.)

---

_Comment by @konstin on 2024-02-23 15:03_

We use this to determine whether to remove the seed packages: Keep if they came from `virtualenv`/`venv`, remove if they were installed by uv.

---

_@zanieb approved on 2024-02-23 15:06_

---

_Merged by @zanieb on 2024-02-23 17:11_

---

_Closed by @zanieb on 2024-02-23 17:11_

---

_Branch deleted on 2024-02-23 17:11_

---
