```yaml
number: 9855
title: Fix bug in terms when collapsing unavailable versions in resolver errors
type: pull_request
state: closed
author: zanieb
labels:
  - bug
assignees: []
draft: true
base: main
head: zb/fix-terms
created_at: 2024-12-12T23:49:18Z
updated_at: 2024-12-13T06:02:36Z
url: https://github.com/astral-sh/uv/pull/9855
synced_at: 2026-01-10T12:00:01Z
```

# Fix bug in terms when collapsing unavailable versions in resolver errors

---

_Pull request opened by @zanieb on 2024-12-12 23:49_

_No description provided._

---

_Label `bug` added by @zanieb on 2024-12-12 23:49_

---

_@zanieb reviewed on 2024-12-12 23:50_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:13799 on 2024-12-12 23:50_

This is what spurred this investigation, this conclusion made.. no sense at all? Now it is just missing a `<=` instead of `=`. Hm.

---

_@zanieb reviewed on 2024-12-12 23:51_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:2184 on 2024-12-12 23:51_

It's nice to avoid that enumeration, but where does `4.7.0` come from!?

---

_@zanieb reviewed on 2024-12-12 23:54_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:13799 on 2024-12-12 23:54_

Here's the unsimplified version

```
          and open3d<=0.15.2 has no wheels with a matching Python ABI tag, we can conclude that all of:
              open3d<0.15.2
              open3d>0.15.2,<0.16.0
              open3d>0.16.0,<0.16.1
              open3d>0.16.1,<0.17.0
              open3d>0.17.0,<0.18.0
              open3d>0.18.0
           cannot be used.
```

---

_@zanieb reviewed on 2024-12-12 23:55_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:2184 on 2024-12-12 23:55_

The unsimplified version

```
          have no usable wheels and building from source is disabled, we can conclude that all of:
              anyio<4.7.0
              anyio>4.7.0
           cannot be used.
```

This suggests the terms merging is just wrong?

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:13799 on 2024-12-13 04:22_

And before... it's actually

```
 we can conclude that all of:
              open3d<0.9.0.0
              open3d>0.9.0.0,<0.10.0.0
              open3d>0.10.0.0,<0.10.0.1
              open3d>0.10.0.1,<0.11.0
              open3d>0.11.0,<0.11.1
              open3d>0.11.1,<0.11.2
              open3d>0.11.2,<0.12.0
              open3d>0.12.0,<0.13.0
              open3d>0.13.0,<0.14.1
              open3d>0.14.1,<0.15.1
              open3d>0.15.1,<0.15.2
              open3d>0.15.2,<0.16.0
              open3d>0.16.0,<0.16.1
              open3d>0.16.1,<0.17.0
              open3d>0.17.0,<0.18.0
              open3d>0.18.0
```

---

_@zanieb reviewed on 2024-12-13 04:22_

---

_Closed by @zanieb on 2024-12-13 06:02_

---
