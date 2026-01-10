```yaml
number: 3748
title: Improve display of selected interpreter sources
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/source-selector
created_at: 2024-05-22T17:44:56Z
updated_at: 2024-05-22T19:00:29Z
url: https://github.com/astral-sh/uv/pull/3748
synced_at: 2026-01-10T14:32:20Z
```

# Improve display of selected interpreter sources

---

_Pull request opened by @zanieb on 2024-05-22 17:44_

e.g. this error message is not great

```
❯ uv venv --python 3.12.2
  × No interpreter found for Python 3.12.2 in provided path, search path, managed toolchains, or parent interpreter
```

---

_Label `error messages` added by @zanieb on 2024-05-22 17:44_

---

_Comment by @zanieb on 2024-05-22 17:46_

We had test coverage for this in e.g. `create_venv_unknown_python_patch` but we set the `UV_TEST_PYTHON_PATH` so it forces a subset of sources — I've updated those to avoid the mock.

---

_Marked ready for review by @zanieb on 2024-05-22 18:01_

---

_Review requested from @charliermarsh by @zanieb on 2024-05-22 18:04_

---

_@charliermarsh approved on 2024-05-22 18:39_

---

_@charliermarsh reviewed on 2024-05-22 18:39_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:53 on 2024-05-22 18:39_

So this is effectively shorthand for a specific set of sources, and lets us add a better error message later?

---

_Merged by @zanieb on 2024-05-22 18:39_

---

_Closed by @zanieb on 2024-05-22 18:39_

---

_Branch deleted on 2024-05-22 18:39_

---

_@zanieb reviewed on 2024-05-22 19:00_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:53 on 2024-05-22 19:00_

Yeah. I actually don't think there's a use-case for a specific set of sources outside of testing. Instead of enumerating all the possible sources, we'll list the sources that we considered in a way that is actually relevant to the user.

I think there are some other directions we could go in the future, like ruling out some sources given the request kind but this was a simple path forward for this release. 



---
