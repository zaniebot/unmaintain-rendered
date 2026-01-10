---
number: 807
title: Simplify ranges in prerelease specifier hints
type: issue
state: closed
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2024-01-05T22:36:23Z
updated_at: 2024-01-07T17:40:24Z
url: https://github.com/astral-sh/uv/issues/807
synced_at: 2026-01-10T01:23:04Z
---

# Simplify ranges in prerelease specifier hints

---

_Issue opened by @zanieb on 2024-01-05 22:36_

e.g. https://github.com/astral-sh/puffin/blob/88adba83a01357dec22bc7e2a0d79ab53e589d5c/crates/puffin-cli/tests/pip_install_scenarios.rs#L1569

The literal specifier is included, but since this comes from resolution of transitive dependencies we should probably simplify it as done in the unsatisfiable error message

https://github.com/astral-sh/puffin/blob/88adba83a01357dec22bc7e2a0d79ab53e589d5c/crates/puffin-cli/tests/pip_install_scenarios.rs#L1566

---

_Label `error messages` added by @zanieb on 2024-01-05 22:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-07 02:18_

---

_Referenced in [astral-sh/uv#825](../../astral-sh/uv/pulls/825.md) on 2024-01-07 02:32_

---

_Closed by @charliermarsh on 2024-01-07 17:40_

---
