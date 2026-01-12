```yaml
number: 14167
title: "make `--check` outdated a non-error status 1"
type: pull_request
state: merged
author: Gankra
labels:
  - error messages
  - breaking
assignees: []
merged: true
base: release/080
head: gankra/status-2
created_at: 2025-06-20T18:44:59Z
updated_at: 2025-07-14T17:47:54Z
url: https://github.com/astral-sh/uv/pull/14167
synced_at: 2026-01-12T16:11:03Z
```

# make `--check` outdated a non-error status 1

---

_@Gankra_

In the case of `uv sync` all we really need to do is handle the `OutdatedEnvironment` error (precisely the error we yield only on dry-runs when everything Works but we determine things are outdated) in `OperationDiagnostic::report` (the post-processor on all `operations::install` calls) because any diagnostic handled by that gets downgraded to from status 2 to status 1 (although I don't know if that's really intentional or a random other bug in our status handling... but I figured it's best to highlight that other potential status code incongruence than not rely on it 😄).

Fixes #12603

---

_Review requested from @zanieb by @Gankra on 2025-06-20 18:45_

---

_Comment by @zanieb on 2025-06-20 18:49_

Also `uv lock --check` and maybe `uv sync --locked`?

---

_Comment by @Gankra on 2025-06-20 18:50_

Yeah I'm currently writing tests for `uv lock --check` because none seem to exist 😓 

---

_Renamed from "make `uv sync --check` outdated a non-error status 1" to "make `--check` outdated a non-error status 1" by @Gankra on 2025-06-20 19:17_

---

_Label `error messages` added by @Gankra on 2025-06-20 19:17_

---

_@Gankra reviewed on 2025-06-20 19:20_

---

_Review comment by @Gankra on `crates/uv/tests/it/lock.rs`:11839 on 2025-06-20 19:20_

While I was in there I upgraded `lock --check` to behave like `sync --check` in that it prints the changes -- using the printing logic of `lock --dry-run` (unlike sync the existing lock change printing code doesn't mess with the tense, so it more looks like we're actually modifying the lock).

---

_Label `breaking` added by @Gankra on 2025-06-20 21:20_

---

_Added to milestone `v0.8.0` by @Gankra on 2025-06-20 21:20_

---

_@zanieb reviewed on 2025-06-27 23:35_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:11838 on 2025-06-27 23:35_

These messages are a bit misleading, they make it sound like we have performed these actions but really they _would be_ performed?

---

_@zanieb reviewed on 2025-06-27 23:36_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:11838 on 2025-06-27 23:36_

I would probably drop this from the pull request, it's

- Not a breaking change, and can be released outside 0.8
- Not related or relevant for this change, and shouldn't block review of the core change here

---

_@jtfmumm reviewed on 2025-07-14 09:01_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/lock.rs`:11838 on 2025-07-14 09:01_

Removed

---

_Marked ready for review by @jtfmumm on 2025-07-14 13:14_

---

_@zanieb approved on 2025-07-14 14:03_

---

_Merged by @jtfmumm on 2025-07-14 17:47_

---

_Closed by @jtfmumm on 2025-07-14 17:47_

---

_Branch deleted on 2025-07-14 17:47_

---
