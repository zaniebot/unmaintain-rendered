```yaml
number: 7620
title: Fix link-mode=clone on linux
type: pull_request
state: merged
author: aroig
labels:
  - bug
assignees: []
merged: true
base: main
head: reflink_on_linux
created_at: 2024-09-22T14:48:20Z
updated_at: 2024-09-22T17:40:52Z
url: https://github.com/astral-sh/uv/pull/7620
synced_at: 2026-01-12T16:07:55Z
```

# Fix link-mode=clone on linux

---

_@aroig_

- **Do not attempt to reflink directories on linux**
- **Refactor clone_recursive**

## Summary

On linux, reflink does not work on a directory. Currently, we first attempt to reflink directory, and only if it fails with `AlreadyExists` we attempt to reflink recursively.

This has the effect that, on linux, `uv pip install --link-mode=clone` would always fall back to `copy`.

We resolve this by only attempting to reflink directories on macos. In the process, we refactored `clone_recursive` in an attempt to make it easier to reason about its logic.


## Test Plan

I tested that after this change, `uv pip install --link-mode=clone numpy` would behave as expected in the following cases:

* linux, btrfs filesystem, venv on the same filesystem as cache (correctly reflinked)
* linux, btrfs filesystem, venv on a different filesystem than cache (fallback to copy)

I have not tested it on macos or windows, as I currently don't have access to any macos or windows machines, unfortunately.


---

_Comment by @aroig on 2024-09-22 15:52_

I pushed a couple of extra commits to make the linter happy, and close a gap for a race condition I missed initially, and caused a CI failure on macos.

This is ready for review.

---

_Comment by @charliermarsh on 2024-09-22 16:13_

Oh smart, thank you. Will review. I’ll admit I’ve never actually tested this on btrfs.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-22 16:13_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-09-22 16:13_

---

_Label `bug` added by @charliermarsh on 2024-09-22 16:13_

---

_@charliermarsh reviewed on 2024-09-22 16:59_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:375 on 2024-09-22 16:59_

Why would it not be sufficient to extend this to Linux?

---

_@aroig reviewed on 2024-09-22 17:14_

---

_Review comment by @aroig on `crates/install-wheel-rs/src/linker.rs`:375 on 2024-09-22 17:14_

It is. This is the first commit in the PR, actually.

I then tried to decouple a bit this code for the next person who needs to touch it. I found it a bit difficult to reason about because it was handling 2 separate fallback logic, plus the windows and linux special case in `clone_recursive`.

I do think this makes it easier to follow, but I understand this is somewhat subjective. If you prefer, I can stick with the minimal change that fixes the bug.

---

_@charliermarsh reviewed on 2024-09-22 17:23_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:375 on 2024-09-22 17:23_

While I definitely appreciate the intent, I would prefer to use the minimal change, since this function is _so_ critical and making large changes to it requires extensive testing.

---

_@aroig reviewed on 2024-09-22 17:31_

---

_Review comment by @aroig on `crates/install-wheel-rs/src/linker.rs`:375 on 2024-09-22 17:31_

Ok. I updated the branch with a minimal change to fix the bug.

---

_@charliermarsh approved on 2024-09-22 17:40_

---

_@charliermarsh reviewed on 2024-09-22 17:40_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:375 on 2024-09-22 17:40_

Thanks. This I can easily and confidently merge.

---

_Merged by @charliermarsh on 2024-09-22 17:40_

---

_Closed by @charliermarsh on 2024-09-22 17:40_

---
