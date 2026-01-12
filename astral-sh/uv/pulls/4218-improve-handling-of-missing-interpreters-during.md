```yaml
number: 4218
title: Improve handling of missing interpreters during discovery
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/interp-io
created_at: 2024-06-10T21:29:51Z
updated_at: 2024-06-10T22:26:35Z
url: https://github.com/astral-sh/uv/pull/4218
synced_at: 2026-01-12T16:06:06Z
```

# Improve handling of missing interpreters during discovery

---

_@zanieb_

Cherry-picked from https://github.com/astral-sh/uv/pull/4214

The first commit gets us some context on an IO error during queries:

Previously:

```
failed to canonicalize path `[VENV]/bin/python3`
    Caused by: No such file or directory (os error 2)
```

Now:

```
Failed to query Python interpreter
    Caused by: failed to canonicalize path `[VENV]/bin/python3`
    Caused by: No such file or directory (os error 2)
```

but really we shouldn't attempt to query a missing interpreter during discovery anyway, so we improve handling of that too.

---

_Label `bug` added by @zanieb on 2024-06-10 21:29_

---

_@zanieb reviewed on 2024-06-10 21:50_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:142 on 2024-06-10 21:50_

Oh hey this is way better, why would we fail here anyway?

---

_@zanieb reviewed on 2024-06-10 21:51_

---

_Review comment by @zanieb on `crates/uv/tests/pip_sync.rs`:130 on 2024-06-10 21:51_

Now this isn't amazing, I guess with `-v` you'd see why your `VIRTUAL_ENV` was ignored. We could add some sort of hint in the future?

---

_@charliermarsh reviewed on 2024-06-10 21:51_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:142 on 2024-06-10 21:51_

Yeah I'm actually not sure.

---

_@zanieb reviewed on 2024-06-10 21:52_

---

_Review comment by @zanieb on `crates/uv/tests/pip_sync.rs`:130 on 2024-06-10 21:52_

Since it literally doesn't exist this seems okay for now though

---

_@charliermarsh reviewed on 2024-06-10 21:52_

---

_Review comment by @charliermarsh on `crates/uv-toolchain/src/interpreter.rs`:649 on 2024-06-10 21:52_

Can we instead match on `uv_fs::canonicalize_executable(executable)`, and just do this in the `NotFound` branch of the error? Otherwise, we're doing another fs operation that we can avoid.

---

_@zanieb reviewed on 2024-06-10 21:55_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:142 on 2024-06-10 21:55_

Well I know why we did but yeah there's no reason to 

---

_@zanieb reviewed on 2024-06-10 21:56_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/interpreter.rs`:649 on 2024-06-10 21:56_

Sure, I was worried that wasn't future proof but it _is_ faster...

---

_@charliermarsh approved on 2024-06-10 22:19_

---

_Merged by @zanieb on 2024-06-10 22:26_

---

_Closed by @zanieb on 2024-06-10 22:26_

---

_Branch deleted on 2024-06-10 22:26_

---
