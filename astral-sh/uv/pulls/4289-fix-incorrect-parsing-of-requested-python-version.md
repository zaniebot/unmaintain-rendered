```yaml
number: 4289
title: Fix incorrect parsing of requested Python version as empty version specifiers
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-exec-name
created_at: 2024-06-12T21:17:15Z
updated_at: 2024-06-13T00:48:59Z
url: https://github.com/astral-sh/uv/pull/4289
synced_at: 2026-01-10T13:54:02Z
```

# Fix incorrect parsing of requested Python version as empty version specifiers

---

_Pull request opened by @zanieb on 2024-06-12 21:17_

Before 0.2.10 we would parse `--python=python` as an executable name. After https://github.com/astral-sh/uv/pull/4214, we started treating this as a Python version range request (with an empty version range). This is not entirely unreasonable, but it was an unexpected regression and I don't think `VersionRequest` should support empty ranges in its `from_str` implementation without more consideration.

---

_Label `bug` added by @zanieb on 2024-06-12 21:17_

---

_@charliermarsh reviewed on 2024-06-12 21:22_

---

_Review comment by @charliermarsh on `crates/uv-toolchain/src/discovery.rs`:1239 on 2024-06-12 21:22_

What is `specifiers` in this case?

---

_@zanieb reviewed on 2024-06-12 21:23_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:1239 on 2024-06-12 21:23_

Sorry can you rephrase? It's an empty `VersionSpecifiers` that allows any version.

---

_@charliermarsh reviewed on 2024-06-12 21:45_

---

_Review comment by @charliermarsh on `crates/uv-toolchain/src/discovery.rs`:1239 on 2024-06-12 21:45_

How does `"python"` get parsed as an empty `VersionSpecifiers`? I'm just trying to understand the data flow.

---

_@zanieb reviewed on 2024-06-12 21:53_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:1239 on 2024-06-12 21:53_

Ah yes I can answer that :)

https://github.com/astral-sh/uv/blob/5f37395f4537026529c8dcb38a9151c9d45d388a/crates/uv-toolchain/src/discovery.rs#L883-L888

Previously an empty remainder here would result in us not treating this as a Python version request, now it does. I didn't expect `VersionSpecifiers` to allow empty strings, but it makes sense in hindsight.

I'm going to need to split `VersionRequest::Range` out of `VersionRequest` (or something like that) to have the user experience I want — trying to figure out how best to do that next.

---

_@charliermarsh approved on 2024-06-12 23:57_

---

_Merged by @zanieb on 2024-06-13 00:48_

---

_Closed by @zanieb on 2024-06-13 00:48_

---

_Branch deleted on 2024-06-13 00:48_

---
