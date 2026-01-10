```yaml
number: 4805
title: Always use base interpreter for cached environments
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/cached
created_at: 2024-07-04T13:58:28Z
updated_at: 2024-07-04T17:38:54Z
url: https://github.com/astral-sh/uv/pull/4805
synced_at: 2026-01-10T13:48:28Z
```

# Always use base interpreter for cached environments

---

_Pull request opened by @charliermarsh on 2024-07-04 13:58_

Closes #4801.

---

_Label `enhancement` added by @charliermarsh on 2024-07-04 13:58_

---

_Label `preview` added by @charliermarsh on 2024-07-04 13:58_

---

_@zanieb reviewed on 2024-07-04 15:24_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/environment.rs`:51 on 2024-07-04 15:24_

Can we add a `Interpreter::into_base_intepreter(self) -> Interpreter` method to do this? Seems like we'll want this other places.

---

_@zanieb approved on 2024-07-04 15:24_

---

_@charliermarsh reviewed on 2024-07-04 15:40_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/environment.rs`:51 on 2024-07-04 15:40_

Sure thing.

---

_Merged by @charliermarsh on 2024-07-04 17:38_

---

_Closed by @charliermarsh on 2024-07-04 17:38_

---

_Branch deleted on 2024-07-04 17:38_

---
