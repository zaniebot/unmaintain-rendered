---
number: 808
title: Improve message when a transitive dependency requires a package that does not exist
type: issue
state: closed
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2024-01-05T22:40:01Z
updated_at: 2024-04-23T19:55:20Z
url: https://github.com/astral-sh/uv/issues/808
synced_at: 2026-01-07T13:12:16-06:00
---

# Improve message when a transitive dependency requires a package that does not exist

---

_Issue opened by @zanieb on 2024-01-05 22:40_

e.g. in the following scenario

https://github.com/astral-sh/puffin/blob/88adba83a01357dec22bc7e2a0d79ab53e589d5c/crates/puffin-cli/tests/pip_install_scenarios.rs#L1810-L1819


We say that package "b" was not found, but we do not explain why "b" was required.

https://github.com/astral-sh/puffin/blob/88adba83a01357dec22bc7e2a0d79ab53e589d5c/crates/puffin-cli/tests/pip_install_scenarios.rs#L1848


---

_Label `error messages` added by @zanieb on 2024-01-05 22:47_

---

_Comment by @ibraheemdev on 2024-04-23 19:54_

I believe this was resolved by https://github.com/astral-sh/uv/pull/1241.

---

_Comment by @zanieb on 2024-04-23 19:55_

Yeah that looks correct, thanks!

---

_Closed by @zanieb on 2024-04-23 19:55_

---
