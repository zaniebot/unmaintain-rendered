```yaml
number: 4968
title: Add documentation for running scripts
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: zb/docs-running-scripts
created_at: 2024-07-10T16:58:10Z
updated_at: 2024-07-15T21:59:40Z
url: https://github.com/astral-sh/uv/pull/4968
synced_at: 2026-01-10T13:42:52Z
```

# Add documentation for running scripts

---

_Pull request opened by @zanieb on 2024-07-10 16:58_

_No description provided._

---

_Label `documentation` added by @zanieb on 2024-07-10 16:58_

---

_Label `preview` added by @zanieb on 2024-07-10 16:58_

---

_Review requested from @charliermarsh by @zanieb on 2024-07-10 18:59_

---

_@zanieb reviewed on 2024-07-10 19:00_

---

_Review comment by @zanieb on `docs/guides/scripts.md`:99 on 2024-07-10 19:00_

Somewhere around here we'd cover #4973 

---

_@ibraheemdev reviewed on 2024-07-11 20:42_

---

_Review comment by @ibraheemdev on `docs/guides/scripts.md`:2 on 2024-07-11 20:42_

Should we add a bit of top-level context about scripts and the benefit of running them through `uv`?

---

_Review comment by @ibraheemdev on `docs/guides/scripts.md`:56 on 2024-07-11 20:45_

```suggestion
When a script requires dependencies, they must be installed into the environment that the script runs in. uv prefers to create these environments on-demand instead of maintaining a long-lived virtual environment with manually managed dependencies. This requires explicit declaration
```

---

_Review comment by @ibraheemdev on `docs/guides/scripts.md`:118 on 2024-07-11 20:46_

```suggestion
uv will automatically create an environment with the dependencies necessary to run the script, e.g.:
```

---

_@ibraheemdev approved on 2024-07-11 20:47_

---

_Merged by @zanieb on 2024-07-15 21:59_

---

_Closed by @zanieb on 2024-07-15 21:59_

---

_Branch deleted on 2024-07-15 21:59_

---
