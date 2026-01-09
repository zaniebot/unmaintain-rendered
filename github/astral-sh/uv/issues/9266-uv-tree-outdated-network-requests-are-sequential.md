---
number: 9266
title: "`uv tree --outdated` network requests are sequential"
type: issue
state: closed
author: konstin
labels:
  - performance
assignees: []
created_at: 2024-11-20T08:52:12Z
updated_at: 2024-11-20T16:45:16Z
url: https://github.com/astral-sh/uv/issues/9266
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv tree --outdated` network requests are sequential

---

_Issue opened by @konstin on 2024-11-20 08:52_

`uv tree --outdated` runs network requests sequentially, thereby being really slow. For example, project below has 120 packages, but takes 2.4s to resolve on a warm cache.

During the network requests, no progress spinner is shown, so it looks like the command is stuck.

In the image below, each small orange line is a network request. You can click on the image to open it in a new tab and hover over the simple API requests to see which request is running, and that they are running sequentially.

![outdated](https://github.com/user-attachments/assets/0da57508-8216-4405-a821-06795dbd7cde)

```toml
[project]
name = "data-science"
version = "0.1.0"
requires-python = ">=3.13"
dependencies = [
    "beautifulsoup4>=4.12.3,<5",
    "httpx>=0.27.0,<0.28",
    "matplotlib>=3.9.1,<4",
    "numpy>=2.0.1,<3",
    "pandas>=2.2.2,<3",
    "pydantic>=2.8.2,<3",
    "scikit-learn>=1.5.1,<2",
]

[tool.uv]
dev-dependencies = [
    "ruff>=0.7.1,<0.8",
    "jupyter>=1.0.0,<2.0.0",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

---

_Label `performance` added by @konstin on 2024-11-20 08:52_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-20 16:03_

---

_Comment by @charliermarsh on 2024-11-20 16:03_

Weird, thanks.

---

_Referenced in [astral-sh/uv#9280](../../astral-sh/uv/pulls/9280.md) on 2024-11-20 16:24_

---

_Closed by @charliermarsh on 2024-11-20 16:45_

---

_Closed by @charliermarsh on 2024-11-20 16:45_

---
