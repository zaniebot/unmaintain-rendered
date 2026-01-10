```yaml
number: 8310
title: Patch Python executable name for Windows free-threaded builds
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/313t-name
created_at: 2024-10-17T22:21:23Z
updated_at: 2024-10-17T23:27:58Z
url: https://github.com/astral-sh/uv/pull/8310
synced_at: 2026-01-10T12:54:07Z
```

# Patch Python executable name for Windows free-threaded builds

---

_Pull request opened by @zanieb on 2024-10-17 22:21_

A temporary fix for https://github.com/astral-sh/uv/issues/8298 while we wait for my slower upstream fix at https://github.com/indygreg/python-build-standalone/pull/373

I think we'll want this machinery anyway to ensure that the various executable names are available? Otherwise we need to special-case all the `python` names in `uv run`?

We don't have unit test coverage of managed downloads, so I added an [integration test](https://github.com/astral-sh/uv/actions/runs/11394150653/job/31703956805?pr=8310) similar to what we have for Linux.

---

_Label `bug` added by @zanieb on 2024-10-17 22:21_

---

_@zanieb reviewed on 2024-10-17 22:42_

---

_Review comment by @zanieb on `crates/uv-fs/src/lib.rs`:111 on 2024-10-17 22:42_

Bitten (once again) by the fact that `replace_symlink` is only valid for directories on Windows, so I've added a new method for files. I plan to do some sort of refactor following this to make it hard to call the wrong thing here.

---

_Marked ready for review by @zanieb on 2024-10-17 22:44_

---

_Review comment by @carljm on `crates/uv-fs/src/lib.rs`:110 on 2024-10-17 22:46_

This doesn't seem to describe what the function below does. (The first sentence does, though.)

---

_Review comment by @carljm on `crates/uv-python/src/managed.rs`:340 on 2024-10-17 22:48_

Is it "missing the standard Python executable on Windows" or "missing the standard Python executable for free-threaded builds on Windows"?

If the former, then the code below seems like an incomplete workaround.

If the latter, maybe clarify the comment.

---

_Review comment by @carljm on `crates/uv-python/src/managed.rs`:335 on 2024-10-17 22:49_

For non-free-threaded Windows builds, `python.exe` already will exist? We don't need to copy over a versioned executable?

In this case where the expected Python executable doesn't exist, and we have no intention to do anything about it, are we just silently creating a broken experience down the line? Should we error here and warn the user?

---

_Review comment by @carljm on `crates/uv-python/src/managed.rs`:353 on 2024-10-17 22:52_

What if this executable doesn't exist? Should we explicitly test that and error if not?

I assume the symlink/copy below will fail, but will it fail with a comprehensible error?

---

_@carljm approved on 2024-10-17 22:53_

(insert "I have no idea what I'm doing" dog-science meme here)

---

_@zanieb reviewed on 2024-10-17 23:03_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:340 on 2024-10-17 23:03_

It's missing `python.exe`, instead it has `python3.13t.exe` which... I think is the former? What am I missing?

---

_@zanieb reviewed on 2024-10-17 23:04_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:335 on 2024-10-17 23:04_

Yeah `python.exe` exists in every existing Python distribution _except_ the free-threaded ones.

> In this case where the expected Python executable doesn't exist, and we have no intention to do anything about it, are we just silently creating a broken experience down the line? Should we error here and warn the user?

Hm. Good question. Yeah we can't _fix_ things here but we sure can complain. Thanks!

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:353 on 2024-10-17 23:07_

Yeah I thought about testing it but was okay with the failure. It should "never occur" and we should get some error context but we could probably add more. I'll try to do that.

---

_@zanieb reviewed on 2024-10-17 23:07_

---

_@zanieb reviewed on 2024-10-17 23:14_

---

_Review comment by @zanieb on `crates/uv-fs/src/lib.rs`:110 on 2024-10-17 23:14_

I believe I've addressed this by tweaking the wording.

---

_@zanieb reviewed on 2024-10-17 23:15_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:335 on 2024-10-17 23:15_

We will throw an error now.

---

_@zanieb reviewed on 2024-10-17 23:15_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:353 on 2024-10-17 23:15_

We have a couple error variants for this now.

---

_@zanieb reviewed on 2024-10-17 23:19_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:340 on 2024-10-17 23:19_

Oh I misread what you said, definitely the latter.

---

_@carljm approved on 2024-10-17 23:23_

Looks great to me!

---

_Merged by @zanieb on 2024-10-17 23:27_

---

_Closed by @zanieb on 2024-10-17 23:27_

---

_Branch deleted on 2024-10-17 23:27_

---
