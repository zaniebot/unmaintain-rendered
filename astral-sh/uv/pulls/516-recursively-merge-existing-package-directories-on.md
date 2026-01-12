```yaml
number: 516
title: Recursively merge existing package directories on installation
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/fix-race-clone
created_at: 2023-11-29T19:08:35Z
updated_at: 2023-11-30T16:15:08Z
url: https://github.com/astral-sh/uv/pull/516
synced_at: 2026-01-12T16:04:00Z
```

# Recursively merge existing package directories on installation

---

_@zanieb_

Previously, when installing a package we would delete the target directory before copying (or linking) the contents of the package. However, this means that we do not properly support namespace packages which can share a target directory. Instead the last package to be installed would be override existing packages. Since we install packages in parallel, this could result in a race condition where the target directory already exists which is not allowed when using `clonefile`. See example error in #515. https://github.com/astral-sh/puffin/pull/516/commits/c7e63d2dcebab6aa37427c58a9db7603db546a4f provides a regression test for this — it fails on `main`.

Here, we implement a recursive merge when the target directory already exists. Both packages will be installed into the same directory. We no longer delete the target directory, which seems okay since we uninstall packages before installing now.

When files conflict, we will likely throw an error still. The correct behavior to implement in this case is unclear, as if we just take "first write wins" or "last write wins" we could end up with some files from one package and some from another resulting in two broken packages. A possible solution here is to lock the target directories while copying. See #520 

---

_Comment by @zanieb on 2023-11-29 19:23_

Note this requires a patch to `reflink-copy` such that it hands us the real OS error; see #515. I try the clone then check the error type instead of checking if the directory exists to avoid race conditions where we check for the directory before it is created by a parallel task.

I wonder if we should vendor it again as in https://github.com/astral-sh/puffin/pull/67 — until then I've changed to Konsti's fork.

Upstream PR https://github.com/cargo-bins/reflink-copy/pull/51

---

_Converted to draft by @zanieb on 2023-11-29 19:24_

---

_Marked ready for review by @zanieb on 2023-11-29 19:37_

---

_Review requested from @konstin by @zanieb on 2023-11-30 16:00_

---

_Comment by @konstin on 2023-11-30 16:05_

Could we add in-process locks around the directories we copy, and always override? Together with `LockedDir` making installation exclusive to a single puffin instance, this would ensure that each directory is exclusive written by one package and the merge always has a single winning package. 

---

_Comment by @zanieb on 2023-11-30 16:07_

@konstin do you mind if I create an issue for that and we do it in a follow-up? I need this to be able to run the test suite.

---

_@konstin approved on 2023-11-30 16:09_

Ok, let's merge this and fix the locking later

---

_Merged by @zanieb on 2023-11-30 16:14_

---

_Closed by @zanieb on 2023-11-30 16:14_

---

_Branch deleted on 2023-11-30 16:14_

---
