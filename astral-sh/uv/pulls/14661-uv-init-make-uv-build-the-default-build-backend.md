```yaml
number: 14661
title: "`uv init`: Make `uv_build` the default build backend (from `hatchling`)"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - breaking
assignees: []
merged: true
base: release/080
head: konsti/make-uv-build-backend-the-default-in-uv-init
created_at: 2025-07-16T15:08:00Z
updated_at: 2025-07-16T18:07:10Z
url: https://github.com/astral-sh/uv/pull/14661
synced_at: 2026-01-12T16:11:20Z
```

# `uv init`: Make `uv_build` the default build backend (from `hatchling`)

---

_@konstin_

Closes https://github.com/astral-sh/uv/issues/14298

Switch the default build backend for `uv init` from `hatchling` to `uv_build`.

This change affects the following two commands:

* `uv init --lib`
* `uv init [--app] --package`

It does not affect `uv init` or `uv init --app` without `--package`. `uv init --build-backend <...>` also works as before. 

**Before**

```
$ uv init --lib project
$ cat project/pyproject.toml
[project]
name = "project"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "konstin", email = "konstin@mailbox.org" }
]
requires-python = ">=3.13.2"
dependencies = []

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

**After**

```
$ uv init --lib project
$ cat project/pyproject.toml
[project]
name = "project"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "konstin", email = "konstin@mailbox.org" }
]
requires-python = ">=3.13.2"
dependencies = []

[build-system]
requires = ["uv_build>=0.7.20,<0.8"]
build-backend = "uv_build"
```

I cleaned up some tests for consistency in the second commit.

---

_Label `enhancement` added by @konstin on 2025-07-16 15:08_

---

_Label `breaking` added by @konstin on 2025-07-16 15:08_

---

_@konstin reviewed on 2025-07-16 15:10_

---

_Review comment by @konstin on `crates/uv/src/commands/project/init.rs`:800 on 2025-07-16 15:10_

We could have this as enum default but I like having this explicit since we're doing a multi-step change of defaults.

---

_@konstin reviewed on 2025-07-16 15:11_

---

_Review comment by @konstin on `crates/uv/tests/it/init.rs`:450 on 2025-07-16 15:11_

Preview merged with the regular test case.

---

_@zanieb approved on 2025-07-16 15:12_

---

_Marked ready for review by @konstin on 2025-07-16 15:37_

---

_Merged by @zanieb on 2025-07-16 18:07_

---

_Closed by @zanieb on 2025-07-16 18:07_

---

_Branch deleted on 2025-07-16 18:07_

---
