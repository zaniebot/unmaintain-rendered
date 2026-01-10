---
number: 7035
title: Special case error message when build fails on a very old package version
type: issue
state: open
author: zanieb
labels:
  - help wanted
  - error messages
assignees: []
created_at: 2024-09-04T17:00:32Z
updated_at: 2024-09-08T00:14:03Z
url: https://github.com/astral-sh/uv/issues/7035
synced_at: 2026-01-10T01:24:09Z
---

# Special case error message when build fails on a very old package version

---

_Issue opened by @zanieb on 2024-09-04 17:00_

1. We know very old package versions fail often fail to build due to outdated metadata and dependencies
2. We generally know when a package version was published
3. We get a ton of questions about this (e.g., https://github.com/astral-sh/uv/issues/6052#issuecomment-2329559758)

Let's display a special message for these errors?

---

_Label `error messages` added by @zanieb on 2024-09-04 17:00_

---

_Label `help wanted` added by @zanieb on 2024-09-04 17:00_

---

_Referenced in [astral-sh/uv#7183](../../astral-sh/uv/issues/7183.md) on 2024-09-08 00:10_

---

_Comment by @hauntsaninja on 2024-09-08 00:12_

In particular, it would be nice to have a special error if the package version predates the release of the target Python version's first release candidate (like in https://github.com/astral-sh/uv/issues/7183)

---
