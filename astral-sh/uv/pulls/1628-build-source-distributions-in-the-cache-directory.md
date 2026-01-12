```yaml
number: 1628
title: Build source distributions in the cache directory instead of the global temporary directory
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/tempdir
created_at: 2024-02-18T07:37:45Z
updated_at: 2024-02-18T21:05:40Z
url: https://github.com/astral-sh/uv/pull/1628
synced_at: 2026-01-12T16:04:40Z
```

# Build source distributions in the cache directory instead of the global temporary directory

---

_@zanieb_

Addresses report in https://github.com/astral-sh/uv/issues/1444 where a temporary directory is created outside of the cache directory or current virtual environment.

There is one additional usage of bare `tempdir` outside of tests we may want to change:

https://github.com/astral-sh/uv/blob/2586f655bbf650a9797c8f88b6d9066eefe7a3dc/crates/install-wheel-rs/src/wheel.rs#L567

---

_Comment by @zanieb on 2024-02-18 08:27_

There's another case where we use the cache directory root to create temporary directories so this seems okay. I don't know if we want a dedicated bucket for these eventually?

---

_@charliermarsh reviewed on 2024-02-18 12:57_

---

_Review comment by @charliermarsh on `crates/uv-build/src/lib.rs`:286 on 2024-02-18 12:57_

Should this maybe actually be `wheel_dir`, i.e., the directory we pass to `build`? Unfortunately that's not an argument to this. In practice `wheel_dir` is _also_ always inside the cache so it's probably fine, but perhaps worth documenting.

---

_@charliermarsh approved on 2024-02-18 12:57_

---

_@zanieb reviewed on 2024-02-18 17:16_

---

_Review comment by @zanieb on `crates/uv-build/src/lib.rs`:286 on 2024-02-18 17:16_

Maybe, we can consider a slightly larger change next?

What's the meaning of "wheel_dir" anyway? Where we're going to place the built wheel?

---

_@charliermarsh reviewed on 2024-02-18 18:56_

---

_Review comment by @charliermarsh on `crates/uv-build/src/lib.rs`:286 on 2024-02-18 18:56_

Yeah I'm cool with this change.

Yeah the `wheel_dir` is the directory into which we build the wheel. So if it's `foo`, the wheel gets built and saved at `foo/Jinja2.whl` or whatever (IIRC).

---

_Label `bug` added by @zanieb on 2024-02-18 21:05_

---

_Merged by @zanieb on 2024-02-18 21:05_

---

_Closed by @zanieb on 2024-02-18 21:05_

---

_Branch deleted on 2024-02-18 21:05_

---
