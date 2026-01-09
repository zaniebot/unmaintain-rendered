---
number: 4825
title: Python downloads should show a progress bar
type: issue
state: closed
author: konstin
labels:
  - enhancement
  - good first issue
  - help wanted
  - preview
assignees: []
created_at: 2024-07-05T08:04:16Z
updated_at: 2024-07-07T20:01:36Z
url: https://github.com/astral-sh/uv/issues/4825
synced_at: 2026-01-07T13:12:17-06:00
---

# Python downloads should show a progress bar

---

_Issue opened by @konstin on 2024-07-05 08:04_

When downloading a python toolchain for running a tool, i wait several seconds without any output. This is very untypical and feels wrong for uv. We should add a message indicating the start of a large download and a progress bar. The message should be shown anyway since non-interactive environments don't show progress bars. (The video is from the failing https://github.com/astral-sh/uv/pull/4810, but it applies also when this is fixed)

[Screencast from 2024-07-05 10-01-24.webm](https://github.com/astral-sh/uv/assets/6826232/19a39288-4f78-426a-8d9a-e9a5653c242e)


---

_Label `enhancement` added by @konstin on 2024-07-05 08:04_

---

_Label `preview` added by @konstin on 2024-07-05 08:04_

---

_Comment by @charliermarsh on 2024-07-05 12:34_

Agree!

---

_Comment by @charliermarsh on 2024-07-05 12:37_

We already have infra for this from wheel downloads so I’ll label it as a good first issue.

---

_Label `good first issue` added by @charliermarsh on 2024-07-05 12:38_

---

_Label `help wanted` added by @charliermarsh on 2024-07-05 12:38_

---

_Comment by @zanieb on 2024-07-05 15:32_

Oh there were some tracing progress bars in the `uv-dev` implementation but I guess we lost that. Big 👍 to adding progress bars here.

---

_Referenced in [astral-sh/uv#4840](../../astral-sh/uv/pulls/4840.md) on 2024-07-05 22:09_

---

_Closed by @charliermarsh on 2024-07-07 20:01_

---

_Closed by @charliermarsh on 2024-07-07 20:01_

---
