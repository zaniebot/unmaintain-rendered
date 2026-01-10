---
number: 42
title: "Rooster links to fictitious PRs if you have `origin` pointing to a fork locally instead of the upstream repo"
type: issue
state: closed
author: AlexWaygood
labels: []
assignees: []
created_at: 2024-05-09T17:35:01Z
updated_at: 2025-09-08T14:45:10Z
url: https://github.com/zanieb/rooster/issues/42
synced_at: 2026-01-10T00:06:57Z
---

# Rooster links to fictitious PRs if you have `origin` pointing to a fork locally instead of the upstream repo

---

_Issue opened by @AlexWaygood on 2024-05-09 17:35_

E.g. I tried running the release script at Ruff earlier and this happened (none of those "PRs to my fork" actually exist, the links are all 404s):

![image](https://github.com/zanieb/rooster/assets/66076021/15e3fe45-ddee-4fdc-bc3f-6d91d444a5f9)

This is because my git remotes were set so that `origin` pointed to my fork, and `upstream` pointed to the upstream `Ruff` repo:

```
(rufflease)⚡ % git remote -v
origin    https://github.com/AlexWaygood/ruff (fetch)
origin    https://github.com/AlexWaygood/ruff (push)
upstream    https://github.com/astral-sh/ruff (fetch)
upstream    https://github.com/astral-sh/ruff (push)
```

---

_Closed by @zanieb on 2025-09-08 14:45_

---
