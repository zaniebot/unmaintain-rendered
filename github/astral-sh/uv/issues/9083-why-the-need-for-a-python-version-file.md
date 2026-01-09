---
number: 9083
title: Why the need for a .python-version file?
type: issue
state: closed
author: ANL06488
labels:
  - question
assignees: []
created_at: 2024-11-13T13:17:10Z
updated_at: 2024-11-13T14:01:19Z
url: https://github.com/astral-sh/uv/issues/9083
synced_at: 2026-01-07T13:12:18-06:00
---

# Why the need for a .python-version file?

---

_Issue opened by @ANL06488 on 2024-11-13 13:17_

Not really a bug, but just looking into this project and wondering why a new file is needed on top of the existing and standard pyproject.toml?

I see uv supports the `requires-python` config in pyproject.toml, and I really dislike adding more and more separate config files in projects.

Is there a requirement why `.python-version` needs to be a separate file and not a config within `pyproject.toml`? And if so, perhaps consider adding that reason to the documentation as I doubt I'm the only one that will have that question.

To clarify, great that it supports reading the file, not clear why creating it is standard for `uv init`.


---

_Comment by @charliermarsh on 2024-11-13 14:01_

Please see https://github.com/astral-sh/uv/issues/8920 and https://github.com/astral-sh/uv/issues/8247. The `requires-python` tells you the supported Python range. `.python-version` allows you to restrict the project (locally) to a specific version during development.

---

_Closed by @charliermarsh on 2024-11-13 14:01_

---

_Label `question` added by @charliermarsh on 2024-11-13 14:01_

---

_Referenced in [llamastack/llama-stack#1513](../../llamastack/llama-stack/pulls/1513.md) on 2025-03-10 14:29_

---
