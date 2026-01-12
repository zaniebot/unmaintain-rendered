```yaml
number: 16322
title: "Running `uv lock --check` with outdated lockfile will print that `--check` was passed, instead of `--locked` "
type: pull_request
state: merged
author: cmnemoi
labels:
  - error messages
assignees: []
merged: true
base: main
head: fix/14105
created_at: 2025-10-15T18:27:38Z
updated_at: 2025-10-21T17:46:11Z
url: https://github.com/astral-sh/uv/pull/16322
synced_at: 2026-01-12T16:12:13Z
```

# Running `uv lock --check` with outdated lockfile will print that `--check` was passed, instead of `--locked` 

---

_@cmnemoi_

Hello,

# Summary

This PR fixes the confusing error message when running `uv lock --check` with an outdated lockfile.

Previously, the error message incorrectly stated that `--locked` was provided, even when the user used `--check`.

Now, the error message correctly indicates which flag was used: either `--check` or `--locked`.

This closes #14105.

# Test plan

- I updated the existing integration test (`check_outdated_lock` in `lock.rs`) to verify the new error message includes the correct flag.
- I ran existing tests to ensure I have no introduced regressions for other commands.

---

_Comment by @zanieb on 2025-10-15 23:06_

I was imaging instead setting up a type like `Mode::Lock | Mode::Check(CheckSource::Check | CheckSource::Locked)` instead  of a `bool` so we can report this value appropriately?

sort of like https://github.com/astral-sh/uv/blob/b4168e665e585dff4e21cef352aa2c8a438c4f29/crates/uv/src/commands/tool/run.rs#L65-L72 or https://github.com/astral-sh/uv/blob/13a976a89763442c5fb13e1db3804c9e9920968c/crates/uv/src/commands/python/install.rs#L153-L176

---

_Comment by @cmnemoi on 2025-10-16 07:15_

@zanieb Thanks for the review, I applied necessary changes.


---

_Renamed from "fix: Running `uv lock --check` with outdated lockfile will print that `--check` or `--locked` was passed, instead of `--locked` only" to "fix: Running `uv lock --check` with outdated lockfile will print that `--check` was passed, instead of `--locked` " by @cmnemoi on 2025-10-16 07:19_

---

_Renamed from "fix: Running `uv lock --check` with outdated lockfile will print that `--check` was passed, instead of `--locked` " to "Running `uv lock --check` with outdated lockfile will print that `--check` was passed, instead of `--locked` " by @konstin on 2025-10-20 18:40_

---

_Label `error messages` added by @konstin on 2025-10-21 11:02_

---

_@konstin approved on 2025-10-21 11:15_

Thank you!

---

_@zanieb reviewed on 2025-10-21 13:47_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3731 on 2025-10-21 13:47_

We should hide this one, I think.

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:183 on 2025-10-21 13:49_

Shouldn't we use the source in this warning message?

---

_@zanieb reviewed on 2025-10-21 13:49_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:981 on 2025-10-21 13:50_

We should use `if let LockCheck::Enabled(source) = lock_check {` so you can avoid the `unwrap`

---

_@zanieb reviewed on 2025-10-21 13:50_

---

_@zanieb reviewed on 2025-10-21 13:50_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:981 on 2025-10-21 13:50_

(there are some more instances of this following)

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:183 on 2025-10-21 13:50_

(there are some more instances of this following)

---

_@zanieb reviewed on 2025-10-21 13:50_

---

_@zanieb reviewed on 2025-10-21 13:52_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:13143 on 2025-10-21 13:52_

I'm a bit confused that the setup for this test case changed. Why did it?

---

_Comment by @zanieb on 2025-10-21 13:53_

 Nice! That's what I had in mind :)

---

_@cmnemoi reviewed on 2025-10-21 14:24_

---

_Review comment by @cmnemoi on `crates/uv/tests/it/lock.rs`:13143 on 2025-10-21 14:24_

This is from my old implementation. 

I badly reverted the test case I added for that, I will fix that.

---

_Comment by @cmnemoi on 2025-10-21 15:01_

@zanieb Thanks for the insightful review again, I applied needed changes :+1: 

---

_@zanieb approved on 2025-10-21 17:46_

---

_Merged by @zanieb on 2025-10-21 17:46_

---

_Closed by @zanieb on 2025-10-21 17:46_

---
