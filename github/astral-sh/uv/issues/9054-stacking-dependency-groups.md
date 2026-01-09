---
number: 9054
title: Stacking dependency groups
type: issue
state: closed
author: patrick-egenlauf
labels:
  - needs-design
  - cli
assignees: []
created_at: 2024-11-12T11:52:41Z
updated_at: 2025-12-01T17:42:46Z
url: https://github.com/astral-sh/uv/issues/9054
synced_at: 2026-01-07T13:12:18-06:00
---

# Stacking dependency groups

---

_Issue opened by @patrick-egenlauf on 2024-11-12 11:52_

The [example of dependency groups in PEP 735](https://peps.python.org/pep-0735/#example-dependency-groups-table) shows that dependency groups can depend on other dependency groups.
Is there an uv command that allows a group to be added to another group? I could not find anything about this.


---

_Comment by @zanieb on 2024-11-12 13:52_

You can't do this via the CLI yet, you can edit the `pyproject.toml` and we'll respect it though.

---

_Comment by @patrick-egenlauf on 2024-11-12 15:35_

Thanks for the quick response. Yes, I know about the possibility to edit the `toml` file manually, but I was wondering if it is also possible via the CLI. Are you planning to add this feature to the CLI?

---

_Comment by @zanieb on 2024-11-12 16:59_

I don't know how we'd express it via the CLI yet, it needs to be designed still.

---

_Label `cli` added by @charliermarsh on 2024-11-12 17:47_

---

_Label `needs-design` added by @charliermarsh on 2024-11-12 17:47_

---

_Referenced in [astral-sh/uv#16912](../../astral-sh/uv/issues/16912.md) on 2025-12-01 17:42_

---

_Comment by @zanieb on 2025-12-01 17:42_

Closing in favor of https://github.com/astral-sh/uv/issues/16912 which is easier to find / more descriptive.

---

_Closed by @zanieb on 2025-12-01 17:42_

---
