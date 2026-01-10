```yaml
number: 9339
title: Treat less compatible tags as lower priority in resolver
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/prio
created_at: 2024-11-21T22:47:31Z
updated_at: 2024-11-26T14:51:33Z
url: https://github.com/astral-sh/uv/pull/9339
synced_at: 2026-01-10T12:00:00Z
```

# Treat less compatible tags as lower priority in resolver

---

_Pull request opened by @charliermarsh on 2024-11-21 22:47_

## Summary

This is a second pass at https://github.com/astral-sh/uv/pull/7556, which was reverted in https://github.com/astral-sh/uv/pull/7608 due to a regression in https://github.com/astral-sh/uv/issues/7606. The behavior is actually correct, but a package (`nmslib`) publishes inconsistent metadata, and the change here happened to cause us to select a wheel with "wrong" metadata. It's arbitrary, but it did cause a regression for folks.

Since we're now seeing other issues caused by the wrongness here (and since the reporter in https://github.com/astral-sh/uv/issues/7606 has since removed the dependency), I'm inclined to ship this fix.

Closes https://github.com/astral-sh/uv/issues/7553.
Closes https://github.com/astral-sh/uv/issues/9283.


---

_Review requested from @zanieb by @charliermarsh on 2024-11-21 22:47_

---

_Label `bug` added by @charliermarsh on 2024-11-21 22:47_

---

_@zanieb approved on 2024-11-22 16:23_

I'm still sort of hesitant.

One of the linked issues comes from selecting Python 2 wheels? I feel like we should never read metadata from those?

---

_Comment by @charliermarsh on 2024-11-22 18:16_

We can change that, but why hesitant? The current behavior is legitimately incorrect — it treats less compatible wheels as more compatible.

---

_Comment by @zanieb on 2024-11-22 18:26_

Perhaps I'm not following. I'm going off the title:

> invalid platform as more compatible than invalid Python

This sounds like we prefer an invalid platform over an invalid Python version — I don't see one of those as more-correct?


---

_Comment by @charliermarsh on 2024-11-22 18:59_

I think the PR title is kind of bad (I copied it over from the previous PR). My understanding of what's happening is that, at present, if we have two wheels with incompatible tags, we treat the wheel the wheel with "less compatible" tags as "higher priority" (which is wrong). Later, in error reporting, we say `no wheels with a matching Python implementation tag` if the "highest priority" tag is `IncompatibleTag::Python`, because we're demonstrating that the best wheel we could find uses a different Python implementation. But that's not true -- there _are_ wheels with matching Python implementations, we just treated them as "lower priority" by accident.

---

_Comment by @charliermarsh on 2024-11-22 19:09_

Basically: I think everywhere else we actually _do_ encode that mismatching platform tag is more compatible than mismatching Python implementation or ABI:

```rust
#[derive(Debug, Eq, Ord, PartialEq, PartialOrd, Clone)]
pub enum IncompatibleTag {
    /// The tag is invalid and cannot be used.
    Invalid,
    /// The Python implementation tag is incompatible.
    Python,
    /// The ABI tag is incompatible.
    Abi,
    /// The Python version component of the ABI tag is incompatible with `requires-python`.
    AbiPythonVersion,
    /// The platform tag is incompatible.
    Platform,
}
```

So the system assumes that in various places. Right now, we would actually actively prioritize Python 2 wheels over Python 3 wheels.

---

_Comment by @zanieb on 2024-11-22 19:51_

Thanks for explaining! Makes sense to me then.

---

_Renamed from "Treat invalid platform as more compatible than invalid Python" to "Treat less compatible tags as lower priority in resolver" by @charliermarsh on 2024-11-26 14:23_

---

_Merged by @charliermarsh on 2024-11-26 14:51_

---

_Closed by @charliermarsh on 2024-11-26 14:51_

---

_Branch deleted on 2024-11-26 14:51_

---
