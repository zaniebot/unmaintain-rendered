```yaml
number: 10912
title: Require tests to opt-in to managed Python installation
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/install-test
created_at: 2025-01-23T19:59:04Z
updated_at: 2025-01-23T21:27:59Z
url: https://github.com/astral-sh/uv/pull/10912
synced_at: 2026-01-12T16:09:34Z
```

# Require tests to opt-in to managed Python installation

---

_@zanieb_

First of all, I want to test automatic managed installs (see #10913) and need to set that up. Second of all, some tests were _implicitly_ downloading interpreters instead of using the one from their context — which is unexpected and naughty and very slow.
 

---

_Label `testing` added by @zanieb on 2025-01-23 19:59_

---

_@zanieb reviewed on 2025-01-23 20:00_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock_conflict.rs`:7652 on 2025-01-23 20:00_

Confused this changed 🤔 

---

_@zanieb reviewed on 2025-01-23 21:13_

---

_Review comment by @zanieb on `scripts/packages/anyio_local/.python-version`:1 on 2025-01-23 21:13_

Annoying, but... `uv build` treats this package as the start of the workspace and it picks up the `.python-version` pin in our repository (3.13) which cannot be satisfied during the build tests (which use 3.12).

The better solution here is to _copy_ this project into the test context instead of using this possibly dirty directory in each test! but I don't want to deal with that here.

---

_@zanieb reviewed on 2025-01-23 21:24_

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:541 on 2025-01-23 21:24_

This bans Python downloads in most tests.

---

_Merged by @zanieb on 2025-01-23 21:24_

---

_Closed by @zanieb on 2025-01-23 21:24_

---

_Branch deleted on 2025-01-23 21:24_

---

_@Gankra approved on 2025-01-23 21:27_

Seems reasonable/better.

---

_@Gankra reviewed on 2025-01-23 21:27_

---

_Review comment by @Gankra on `scripts/packages/anyio_local/.python-version`:1 on 2025-01-23 21:27_

This is such a mood 😭 

---
