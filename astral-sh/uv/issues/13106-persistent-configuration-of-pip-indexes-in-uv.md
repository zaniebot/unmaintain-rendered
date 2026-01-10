---
number: 13106
title: Persistent configuration of pip indexes in uv
type: issue
state: closed
author: juhaszp95
labels:
  - question
assignees: []
created_at: 2025-04-25T12:21:20Z
updated_at: 2025-04-25T12:40:37Z
url: https://github.com/astral-sh/uv/issues/13106
synced_at: 2026-01-10T01:25:29Z
---

# Persistent configuration of pip indexes in uv

---

_Issue opened by @juhaszp95 on 2025-04-25 12:21_

### Question

Hi,

I'm trying to migrate from `pip` to `uv`. I have a `pip.conf` file with a `[global] index-url` and `extra-index-url` specified. These are persistent configurations so that `pip` always installs from these indexes.

I'd like to achieve the same behaviour in `uv`, so that I can use these indexes automatically when I call `uv add` in any project. I've tried to add them to a user-level `uv.toml` file, but without success - I'm probably using the wrong format, but I couldn't figure out what the right format is (I couldn't find in the docs).

Any help would be appreciated!

### Platform

Linux 4.18.0-425.3.1.el8.x86_64 x86_64 GNU/Linux

### Version

uv 0.6.16

---

_Label `question` added by @juhaszp95 on 2025-04-25 12:21_

---

_Comment by @juhaszp95 on 2025-04-25 12:38_

Answered in [https://github.com/astral-sh/uv/issues/1404](https://github.com/astral-sh/uv/issues/1404).

---

_Closed by @juhaszp95 on 2025-04-25 12:38_

---

_Comment by @charliermarsh on 2025-04-25 12:40_

Thanks for following up.

---
