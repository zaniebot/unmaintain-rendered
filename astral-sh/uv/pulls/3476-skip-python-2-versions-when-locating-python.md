```yaml
number: 3476
title: Skip Python 2 versions when locating Python
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/py2
created_at: 2024-05-09T00:52:37Z
updated_at: 2024-05-09T03:25:23Z
url: https://github.com/astral-sh/uv/pull/3476
synced_at: 2026-01-10T14:37:54Z
```

# Skip Python 2 versions when locating Python

---

_Pull request opened by @charliermarsh on 2024-05-09 00:52_

## Summary

Unfortunately, the `-I` flag was added in Python 3.4. So if we query a Python version prior to 3.4 (e.g., Python 2.7), we can't run our script at all, and lose the ability to match against our structured error.

This PR adds an additional check against the stderr output for these cases.

Closes https://github.com/astral-sh/uv/issues/3474.

## Test Plan

Installed Python 2.7, and verified that it was skipped (and that we instead found my `python3`).

---

_Label `bug` added by @charliermarsh on 2024-05-09 00:52_

---

_Review requested from @zanieb by @charliermarsh on 2024-05-09 00:52_

---

_Review requested from @konstin by @charliermarsh on 2024-05-09 00:52_

---

_@zanieb reviewed on 2024-05-09 01:39_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/find_python.rs`:138 on 2024-05-09 01:39_

I think this is a breaking change? We support these versions right now, we just warn when we find them. I guess we can ban explicit requests? Seems a little weird to roll into this pull request.

---

_@zanieb reviewed on 2024-05-09 01:40_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/find_python.rs`:168 on 2024-05-09 01:40_

Is there something else in there that corroborates that this is Python 2? Very small concern we might see this output from some other executable.

---

_@zanieb approved on 2024-05-09 01:42_

---

_@charliermarsh reviewed on 2024-05-09 02:50_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/find_python.rs`:138 on 2024-05-09 02:50_

I think this is _only_ true for Python 3.7? Python 3.6 and lower don't work at all, at least on Linux -- we raise an error in the script.

---

_@charliermarsh reviewed on 2024-05-09 02:51_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/find_python.rs`:138 on 2024-05-09 02:51_

I can move this elsewhere.... I have to do something like this if we want to retain the behavior we had before, because I removed the `<= Some(2)` check, which will now never fire anyway (because we'll hit the `-I` error path, not the `UnsupportedVersion` error path).

---

_@charliermarsh reviewed on 2024-05-09 02:52_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/find_python.rs`:168 on 2024-05-09 02:52_

To be clear, it may not be Python 2. It could be any Python version before 3.4.

---

_@charliermarsh reviewed on 2024-05-09 02:52_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/find_python.rs`:168 on 2024-05-09 02:52_

But yes, I will look, I'm not sure.

---

_@charliermarsh reviewed on 2024-05-09 03:19_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/find_python.rs`:168 on 2024-05-09 03:19_

I can't run Python 3.3 and friends on my machine so hopefully this works as expected...

---

_Merged by @charliermarsh on 2024-05-09 03:25_

---

_Closed by @charliermarsh on 2024-05-09 03:25_

---

_Branch deleted on 2024-05-09 03:25_

---
