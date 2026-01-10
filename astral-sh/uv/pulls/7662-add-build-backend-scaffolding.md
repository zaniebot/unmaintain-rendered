```yaml
number: 7662
title: Add build backend scaffolding
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/build-backend-scaffolding
created_at: 2024-09-24T16:15:04Z
updated_at: 2024-09-24T17:23:19Z
url: https://github.com/astral-sh/uv/pull/7662
synced_at: 2026-01-10T12:53:52Z
```

# Add build backend scaffolding

---

_Pull request opened by @konstin on 2024-09-24 16:15_

Add the PEP 517 and PEP 660 hooks and minimal `todo!()` backend implementations (they are e.g. still missing passing `sys.executable`). The CLI commands are hidden and will stay hidden (calling them directly is not supported), the Python part of the interface is hidden behind a `UV_PREVIEW` until they have an actual implementation, at which point we'll switch to the regular, warn-only preview mechanism.

I'm putting this up for early merging to avoid more merge conflicts.

---

_Label `preview` added by @konstin on 2024-09-24 16:15_

---

_@zanieb reviewed on 2024-09-24 17:07_

---

_Review comment by @zanieb on `python/uv/_build_backend.py`:12 on 2024-09-24 17:07_

I don't think we need to include this

---

_@zanieb approved on 2024-09-24 17:09_

---

_@konstin reviewed on 2024-09-24 17:22_

---

_Review comment by @konstin on `python/uv/_build_backend.py`:12 on 2024-09-24 17:22_

I've had many cases where something was done due to perfomance reasons but there was no way for me to find out if those still apply.

If Python makes improvement in the future to import times (something that's highly requested, so it's not unlikely to happen), we should document how to check them. It's also good to have some real numbers so we know what we're talking about and whether that matters. In this case, it's 40% or 4ms, the latter is too much for just invoking uv (it's about the time we need to create a venv last time it checked). I feel strongly that when we make unintuitive decisions for performance, we should have the impact both documented and measurable.

---

_Merged by @konstin on 2024-09-24 17:23_

---

_Closed by @konstin on 2024-09-24 17:23_

---

_Branch deleted on 2024-09-24 17:23_

---
