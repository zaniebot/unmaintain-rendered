```yaml
number: 15898
title: Better warning for no direct build
type: pull_request
state: merged
author: konstin
labels:
  - tracing
assignees: []
merged: true
base: main
head: konsti/better-warning-for-no-direct-build
created_at: 2025-09-16T17:13:12Z
updated_at: 2025-09-17T11:18:43Z
url: https://github.com/astral-sh/uv/pull/15898
synced_at: 2026-01-10T06:36:15Z
```

# Better warning for no direct build

---

_Pull request opened by @konstin on 2025-09-16 17:13_

**Setup**

```
$ git clone https://github.com/wheelnext/variant_aarch64
$ cd variant_aarch64
$ git checkout 1d047e667dbce4c74878a68c653a6b41bc3d3684
```

**Before**

```
$ uv build -v
[...]
DEBUG Not using uv build backend direct build of , no pyproject.toml: TOML parse error at line 5, column 1
  |
5 | [project]
  | ^^^^^^^^^
missing field `version`
[...]
```

**After**

```
$ uv build -v
[...]
DEBUG Not using uv build backend direct build of ``, pyproject.toml does not match: The value for `build_system.build-backend` should be `"uv_build"`, not `"flit_core.buildapi"`
[...]
```

The empty string gets fixed in https://github.com/astral-sh/uv/pull/15897

---

_Label `tracing` added by @konstin on 2025-09-16 17:13_

---

_Comment by @zanieb on 2025-09-16 17:39_

Is there a reason it's hard to write a snapshot test for?

---

_Comment by @zanieb on 2025-09-16 17:40_

Oh, it's a debug log.

---

_Comment by @konstin on 2025-09-16 17:41_

I came across this when debugging a variant provider and the debug log led me to first believe the provider was broken, which wasn't the case.

---

_@zanieb reviewed on 2025-09-16 17:51_

---

_Review comment by @zanieb on `crates/uv-build-backend/src/metadata.rs`:82 on 2025-09-16 17:51_

Maybe?

```suggestion
                    "Not using uv build backend direct build for source tree `{name}`, \
```

Separately, should we just use the unsimplified display here? A relative path doesn't seem super userful?

---

_Review comment by @konstin on `crates/uv-build-backend/src/metadata.rs`:82 on 2025-09-17 10:44_

I'm not generally opposed it's just that we're using the user display everywhere already, so if we only change it here we'd get different displays in different locations.

---

_@konstin reviewed on 2025-09-17 10:44_

---

_@zanieb reviewed on 2025-09-17 10:54_

---

_Review comment by @zanieb on `crates/uv-build-backend/src/metadata.rs`:82 on 2025-09-17 10:54_

We do use the other display in some other debug level logs. I don't have strong feelings.

---

_@zanieb approved on 2025-09-17 10:54_

---

_Merged by @konstin on 2025-09-17 11:18_

---

_Closed by @konstin on 2025-09-17 11:18_

---

_Branch deleted on 2025-09-17 11:18_

---
