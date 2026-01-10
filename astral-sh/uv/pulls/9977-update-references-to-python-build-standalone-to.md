```yaml
number: 9977
title: "Update references to `python-build-standalone` to reflect the transferred project"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/pbs
created_at: 2024-12-17T17:19:43Z
updated_at: 2024-12-17T20:20:00Z
url: https://github.com/astral-sh/uv/pull/9977
synced_at: 2026-01-10T12:00:01Z
```

# Update references to `python-build-standalone` to reflect the transferred project

---

_Pull request opened by @zanieb on 2024-12-17 17:19_

_No description provided._

---

_Label `documentation` added by @zanieb on 2024-12-17 19:59_

---

_@charliermarsh approved on 2024-12-17 20:01_

---

_@charliermarsh reviewed on 2024-12-17 20:01_

---

_Review comment by @charliermarsh on `crates/uv-python/src/downloads.rs`:660 on 2024-12-17 20:01_

Is this necessary to retain?

---

_@zanieb reviewed on 2024-12-17 20:03_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:660 on 2024-12-17 20:03_

I think it'd be a breaking change otherwise, yes.

---

_@zanieb reviewed on 2024-12-17 20:04_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:660 on 2024-12-17 20:04_

Oh nevermind we strip _our_ prefix. Yeah I guess we can drop it?

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:660 on 2024-12-17 20:06_

Why do we throw an error here?

```
                    else {
                        return Err(Error::Mirror(EnvVars::UV_PYTHON_INSTALL_MIRROR, self.url));
                    };
```

This is what confused me. Our own URL doesn't meet our expectations?

---

_@zanieb reviewed on 2024-12-17 20:06_

---

_@charliermarsh reviewed on 2024-12-17 20:08_

---

_Review comment by @charliermarsh on `crates/uv-python/src/downloads.rs`:660 on 2024-12-17 20:08_

Yeah

---

_Merged by @zanieb on 2024-12-17 20:19_

---

_Closed by @zanieb on 2024-12-17 20:19_

---

_Branch deleted on 2024-12-17 20:20_

---
