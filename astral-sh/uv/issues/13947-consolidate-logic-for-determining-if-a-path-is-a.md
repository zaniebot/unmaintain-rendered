---
number: 13947
title: Consolidate logic for determining if a path is a virtual environment
type: issue
state: closed
author: zanieb
labels:
  - internal
assignees: []
created_at: 2025-06-10T13:16:39Z
updated_at: 2025-06-23T13:12:44Z
url: https://github.com/astral-sh/uv/issues/13947
synced_at: 2026-01-10T01:25:40Z
---

# Consolidate logic for determining if a path is a virtual environment

---

_Issue opened by @zanieb on 2025-06-10 13:16_

e.g.

https://github.com/astral-sh/uv/blob/bf96c60e3eb805ba9d66630e87cd639c75f81f0a/crates/uv-python/src/interpreter.rs#L948-L952

https://github.com/astral-sh/uv/blob/fb0811680054a8aea9d8babdbf6877c6e3da9afe/crates/uv-python/src/virtualenv.rs#L134

https://github.com/astral-sh/uv/blob/7310ea75da16e6b8c86bbc11c12d6c46a15b8f8e/crates/uv-python/src/discovery.rs#L829-L831

_Originally posted by @zanieb in https://github.com/astral-sh/uv/pull/13531#discussion_r2137832363_
            

---

_Label `internal` added by @zanieb on 2025-06-10 13:16_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-06-10 16:05_

---

_Referenced in [astral-sh/uv#14214](../../astral-sh/uv/pulls/14214.md) on 2025-06-23 11:51_

---

_Closed by @jtfmumm on 2025-06-23 13:12_

---
