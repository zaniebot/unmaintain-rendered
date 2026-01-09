---
number: 4689
title: "`uv tool` should install the latest version of the tool"
type: issue
state: closed
author: konstin
labels:
  - needs-decision
  - preview
assignees: []
created_at: 2024-07-01T12:15:16Z
updated_at: 2024-07-10T05:00:25Z
url: https://github.com/astral-sh/uv/issues/4689
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv tool` should install the latest version of the tool

---

_Issue opened by @konstin on 2024-07-01 12:15_

In https://github.com/astral-sh/uv/issues/4685, the installation of azure-cli failed because the latest version was incompatible. Should `uv tool` force to installing the latest version of a tool instead? I wouldn't know when i'd want `uv tool install foo` to install an older version of foo.

---

_Label `needs-decision` added by @konstin on 2024-07-01 12:15_

---

_Label `preview` added by @konstin on 2024-07-01 12:15_

---

_Comment by @zanieb on 2024-07-01 21:16_

Can you clarify what you mean here? I'm kind of confused.

---

_Comment by @konstin on 2024-07-02 09:03_

In #4685, `uv tool install azure-cli` was backtracking from azure-cli 2.61.0 to 2.0.67, with no indication that we're not installing the latest version. When i install a cli tool by name, i'd expect it to install the latest version of that tool, not backtrack through the versions.

---

_Comment by @charliermarsh on 2024-07-09 22:22_

I guess I don't see how that's different than `uv pip install azure-cli`.

---

_Closed by @charliermarsh on 2024-07-10 05:00_

---
