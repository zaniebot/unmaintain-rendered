---
number: 3506
title: iron out how to represent path dependencies in the universal lock file
type: issue
state: closed
author: BurntSushi
labels:
  - preview
assignees: []
created_at: 2024-05-10T14:02:39Z
updated_at: 2024-06-11T14:09:32Z
url: https://github.com/astral-sh/uv/issues/3506
synced_at: 2026-01-07T13:12:17-06:00
---

# iron out how to represent path dependencies in the universal lock file

---

_Issue opened by @BurntSushi on 2024-05-10 14:02_

We want to support path dependencies in our lock file. But the data model for path dependencies in our lock file is, at the time of writing, not quite right. I believe what we _ought_ to have for a path dependency is a single file URL to a directory, source distribution or wheel. But the data model currently allows for a source distribution _and_ zero or more wheels, but specifically does not allow for a directory.

I think the current data model is mostly just a reaction to the types that we have after resolution. Namely, I think we have two related types. The first is for wheel path dependencies:

https://github.com/astral-sh/uv/blob/616ed530430f08577af5ae549c3d2b525e7e34ce/crates/distribution-types/src/lib.rs#L174-L180

And the second is for source distribution path dependencies:

https://github.com/astral-sh/uv/blob/616ed530430f08577af5ae549c3d2b525e7e34ce/crates/distribution-types/src/lib.rs#L206-L213

My understanding is that the second is _also_ used when we have a path dependency to a directory.

This all matters for the lock file because if a path dependency points to a wheel or a source dist, then we want to record a hash for it. But if it comes from a directory, then we very specifically do not want to record a hash.

So to fix this issue, I think we need to:

* [x] Adjust the `PathSourceDist` type so that it indicates whether it came from a real source distribution or a directory. Possibly one way to do this is to split the type in two, with one type representing source distributions and the other representing a directory.
* [ ] Adjust the `Lock` data model so that distributions with a `path` source kind are more limited than what they are today. I think we actually do _not_ want to encode the file path itself, and instead derive that from the `pyproject.toml`. (This would match what Cargo does.)

---

_Label `preview` added by @BurntSushi on 2024-05-10 14:02_

---

_Referenced in [astral-sh/uv#3347](../../astral-sh/uv/issues/3347.md) on 2024-05-10 14:02_

---

_Comment by @charliermarsh on 2024-05-10 14:03_

I can look at the first bullet there, splitting `PathSourceDist`.

---

_Referenced in [astral-sh/uv#3505](../../astral-sh/uv/pulls/3505.md) on 2024-05-10 14:05_

---

_Referenced in [astral-sh/uv#3519](../../astral-sh/uv/pulls/3519.md) on 2024-05-10 21:19_

---

_Comment by @charliermarsh on 2024-05-20 14:10_

What does the second bullet mean exactly? Can you flesh it out a bit more? Perhaps I can help with it.

---

_Comment by @charliermarsh on 2024-05-20 14:12_

Is it that the path gets omitted entirely when it's part of the workspace?

---

_Comment by @BurntSushi on 2024-05-20 14:20_

> Is it that the path gets omitted entirely when it's part of the workspace?

Yeah exactly. Cargo rebuilds the path from the `Cargo.toml` instead, as I understand it. So in order to get the path of a path dependency in a lock file, I think there is some interaction point between what's in `pyproject.toml` and what's in the lock file.

(There is at least one other interaction point as well: finding the relevant root package in the lock file at which to start a graph traversal.)

---

_Referenced in [astral-sh/uv#3965](../../astral-sh/uv/issues/3965.md) on 2024-06-02 12:52_

---

_Referenced in [astral-sh/uv#3611](../../astral-sh/uv/issues/3611.md) on 2024-06-04 13:42_

---

_Assigned to @konstin by @konstin on 2024-06-10 13:01_

---

_Comment by @konstin on 2024-06-11 14:09_

Fixed by https://github.com/astral-sh/uv/pull/4205, we can revisit this later if we want to omit paths

---

_Closed by @konstin on 2024-06-11 14:09_

---
