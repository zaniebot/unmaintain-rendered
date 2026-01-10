---
number: 4359
title: "Support reading Python versions from `tool.uv` "
type: issue
state: open
author: zanieb
labels:
  - configuration
assignees: []
created_at: 2024-06-17T16:34:26Z
updated_at: 2024-08-24T08:26:23Z
url: https://github.com/astral-sh/uv/issues/4359
synced_at: 2026-01-10T01:23:37Z
---

# Support reading Python versions from `tool.uv` 

---

_Issue opened by @zanieb on 2024-06-17 16:34_

https://github.com/astral-sh/uv/pull/4335 adds support for Python version files, but we should also support a section in the `pyproject.toml` with more structured data.

---

_Label `configuration` added by @zanieb on 2024-06-17 16:34_

---

_Label `preview` added by @zanieb on 2024-06-17 16:34_

---

_Referenced in [astral-sh/uv#4335](../../astral-sh/uv/pulls/4335.md) on 2024-06-17 18:28_

---

_Renamed from "Support reading Python versions `tool.uv` " to "Support reading Python versions from `tool.uv` " by @zanieb on 2024-07-24 02:39_

---

_Referenced in [astral-sh/uv#4970](../../astral-sh/uv/issues/4970.md) on 2024-07-24 02:40_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:22_

---

_Comment by @TommasoAmici on 2024-08-23 15:08_

for projects with multiple languages/tools, it's possible to have the python version in a `.tool-versions` file which is very similar in spirit to `.python-versions`, I don't think `uv`  currently supports this and I can't find an existing issue. I suppose this is more of a feature request, though.

e.g.:
https://asdf-vm.com/manage/configuration.html#tool-versions
https://mise.jdx.dev/configuration.html#tool-versions

---

_Comment by @zanieb on 2024-08-23 15:15_

@TommasoAmici would you mind opening a dedicated issue for supporting that?

---

_Comment by @TommasoAmici on 2024-08-24 08:26_

sure! https://github.com/astral-sh/uv/issues/6574

---

_Referenced in [astral-sh/uv#8247](../../astral-sh/uv/issues/8247.md) on 2024-11-14 15:06_

---

_Referenced in [astral-sh/uv#9494](../../astral-sh/uv/issues/9494.md) on 2024-11-28 10:13_

---

_Referenced in [astral-sh/uv#11697](../../astral-sh/uv/issues/11697.md) on 2025-02-21 13:10_

---
