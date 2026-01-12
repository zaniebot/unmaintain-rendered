```yaml
number: 300
title: Implement custom resolution failure reporter to hide root package versions
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/root-version
created_at: 2023-11-02T21:24:57Z
updated_at: 2023-11-03T15:47:03Z
url: https://github.com/astral-sh/uv/pull/300
synced_at: 2026-01-12T16:03:51Z
```

# Implement custom resolution failure reporter to hide root package versions

---

_@zanieb_

Extends #295 
Closes #214 

Copies some of the implementations from `pubgrub::report` so we can implement Puffin `PubGrubPackage` specific display when explaining failed resolutions.

Here, we just drop the dummy version number if it's a `PubGrubPackage::Root` package. In the future, we can further customize reporting.

---

_@zanieb reviewed on 2023-11-02 21:29_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/snapshots/pip_compile__compile_unsolvable_requirements.snap`:19 on 2023-11-02 21:29_

At what cost... :D

---

_@zanieb reviewed on 2023-11-02 21:30_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:24 on 2023-11-02 21:30_

This is all copied without change

---

_@zanieb reviewed on 2023-11-02 21:30_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:314 on 2023-11-02 21:30_

Here, we change from `external.to_string()` to use our internal type

---

_@zanieb reviewed on 2023-11-02 21:31_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:357 on 2023-11-02 21:31_

Here, we replicate the entire `External` type so we can `impl fmt::Display`

---

_@charliermarsh approved on 2023-11-03 00:20_

Respect

---

_Marked ready for review by @zanieb on 2023-11-03 15:24_

---

_Merged by @zanieb on 2023-11-03 15:47_

---

_Closed by @zanieb on 2023-11-03 15:47_

---

_Branch deleted on 2023-11-03 15:47_

---
