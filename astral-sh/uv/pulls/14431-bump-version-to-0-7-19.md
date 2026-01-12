```yaml
number: 14431
title: Bump version to 0.7.19
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/0719
created_at: 2025-07-02T20:43:52Z
updated_at: 2025-07-02T21:19:53Z
url: https://github.com/astral-sh/uv/pull/14431
synced_at: 2026-01-12T16:11:13Z
```

# Bump version to 0.7.19

---

_@zanieb_

_No description provided._

---

_@zanieb reviewed on 2025-07-02 20:47_

---

_Review comment by @zanieb on `CHANGELOG.md`:10 on 2025-07-02 20:47_

I figure we can post elsewhere about the speed? Otherwise, we could append

 "— `uv sync` on a new project (from `uv init`) is 10-30x faster than with other build backends"

---

_@charliermarsh reviewed on 2025-07-02 20:55_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:8 on 2025-07-02 20:55_

```suggestion
The **[uv build backend](https://docs.astral.sh/uv/concepts/build-backend/) is now stable** and considered ready for production use.
```

---

_@charliermarsh reviewed on 2025-07-02 20:56_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:10 on 2025-07-02 20:56_

```suggestion
The uv build backend is a great choice for pure Python projects. It has reasonable defaults, with the goal of requiring zero configuration for most users, but provides flexible configuration to accommodate most Python project structures. It integrates tightly with uv, to improve messaging and user experience. It validates project metadata and structures, preventing common mistakes. And, finally, it's very fast.
```

---

_@charliermarsh reviewed on 2025-07-02 20:56_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:10 on 2025-07-02 20:56_

I'd add that last point, yeah.

---

_@zanieb reviewed on 2025-07-02 20:58_

---

_Review comment by @zanieb on `CHANGELOG.md`:10 on 2025-07-02 20:58_

Note to self, I'd need to reflect these in https://github.com/astral-sh/uv/blob/8dc61af690047b4f87e03664e39b9666e45229b1/docs/concepts/build-backend.md#L17-L21

I want beginners to understand that it's a good choice for their projects without needing them to understand the concept of "pure Python". We do follow with that in the documentation. Maybe I should copy that note here? Or maybe I should change the release notes as you've suggested by leave the docs as-is? 

---

_Review comment by @zanieb on `CHANGELOG.md`:10 on 2025-07-02 20:59_

"that are using uv" is reasonable to drop, it's good for everyone :)

---

_@zanieb reviewed on 2025-07-02 20:59_

---

_Merged by @zanieb on 2025-07-02 21:19_

---

_Closed by @zanieb on 2025-07-02 21:19_

---

_Branch deleted on 2025-07-02 21:19_

---
