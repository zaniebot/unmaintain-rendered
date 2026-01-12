```yaml
number: 2434
title: Respect HTTP client options when reading remote requirements files
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/base-client-requirements
created_at: 2024-03-13T22:44:14Z
updated_at: 2024-03-21T18:48:58Z
url: https://github.com/astral-sh/uv/pull/2434
synced_at: 2026-01-12T16:05:02Z
```

# Respect HTTP client options when reading remote requirements files

---

_@zanieb_

Uses the base client introduced in #2431 to restore use of our fully configured client when reading remote requirements files.

Closes https://github.com/astral-sh/uv/issues/2357

## Test plan

```
npx http-server --username user --password password
cargo run -- pip install -r http://user:password@127.0.0.1:8080/requirements.txt
```

Fails on main succeeds on branch.

---

_Label `enhancement` added by @zanieb on 2024-03-13 22:44_

---

_Review comment by @zanieb on `crates/uv/src/requirements.rs`:292 on 2024-03-13 22:47_

These clones are alarming to me. We should pass a reference to a lazily built client instead?

---

_@zanieb reviewed on 2024-03-13 22:47_

---

_@zanieb reviewed on 2024-03-14 00:22_

---

_Review comment by @zanieb on `crates/uv/src/requirements.rs`:292 on 2024-03-14 00:22_

Attempting to address in https://github.com/astral-sh/uv/pull/2439

---

_@zanieb reviewed on 2024-03-15 16:57_

---

_Review comment by @zanieb on `crates/uv/src/requirements.rs`:292 on 2024-03-15 16:57_

Addressed here in https://github.com/astral-sh/uv/pull/2434/commits/2bcf617d6a4fe43538e2ceb4209178542495b726 instead

---

_Marked ready for review by @zanieb on 2024-03-15 16:58_

---

_Comment by @zanieb on 2024-03-15 17:12_

Should add a test case for this, might do it separately though

---

_Review requested from @charliermarsh by @zanieb on 2024-03-20 20:52_

---

_@charliermarsh reviewed on 2024-03-21 03:00_

---

_Review comment by @charliermarsh on `crates/uv-client/src/base_client.rs`:101 on 2024-03-21 03:00_

In lieu of calling `build` multiple times, did you explore trying to do this with interior mutability? E.g., making `client()` a `OnceCell` or something lazily constructed?

---

_@zanieb reviewed on 2024-03-21 03:08_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:101 on 2024-03-21 03:08_

Ah I looked at this a little in #2439 — I'd be happy to explore that more next. I tried using `Lazy` and `OnceCell`.

---

_Comment by @zanieb on 2024-03-21 17:29_

@charliermarsh did you feel like we need lazy building before merging this?

---

_@charliermarsh approved on 2024-03-21 18:48_

Na, it's an improvement for users and in correctness.

---

_Merged by @zanieb on 2024-03-21 18:48_

---

_Closed by @zanieb on 2024-03-21 18:48_

---

_Branch deleted on 2024-03-21 18:48_

---
