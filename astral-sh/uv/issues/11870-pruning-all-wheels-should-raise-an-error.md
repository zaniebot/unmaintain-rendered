---
number: 11870
title: Pruning all wheels should raise an error
type: issue
state: open
author: charliermarsh
labels:
  - error messages
assignees: []
created_at: 2025-02-28T22:34:24Z
updated_at: 2025-02-28T22:35:25Z
url: https://github.com/astral-sh/uv/issues/11870
synced_at: 2026-01-10T01:25:12Z
---

# Pruning all wheels should raise an error

---

_Issue opened by @charliermarsh on 2025-02-28 22:34_

In https://github.com/astral-sh/uv/issues/11664, the resolution succeeded, but syncing failed. It turned out that we pruned _all_ wheels for a specific distribution, and it didn't have a source distribution either, so it was actually impossible to install it at runtime _ever_. If we prune all wheels, we should probably raise an error in the resolver.

---

_Label `error messages` added by @charliermarsh on 2025-02-28 22:34_

---
