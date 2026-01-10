---
number: 11227
title: Improve error message for incompatible virtual environment
type: issue
state: open
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2025-02-04T21:41:28Z
updated_at: 2025-02-04T21:41:41Z
url: https://github.com/astral-sh/uv/issues/11227
synced_at: 2026-01-10T01:25:02Z
---

# Improve error message for incompatible virtual environment

---

_Issue opened by @zanieb on 2025-02-04 21:41_

The test case for error messages when a virtual environment has a different Python version regressed, undoing the work from https://github.com/astral-sh/uv/pull/11143 if the interpreter is at the front of the PATH

_Originally posted by @zanieb in https://github.com/astral-sh/uv/pull/11218#discussion_r1941804822_

It's not clear how to do better yet. I think the main problem is we filter the interpreters by version early so we don't know a compatible one is available at all.
            

---

_Label `error messages` added by @zanieb on 2025-02-04 21:41_

---

_Referenced in [astral-sh/uv#11389](../../astral-sh/uv/issues/11389.md) on 2025-02-10 16:18_

---
