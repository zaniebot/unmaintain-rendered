```yaml
number: 281
title: Add stable hash crate
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/hash
created_at: 2023-11-01T20:38:55Z
updated_at: 2023-11-01T23:41:46Z
url: https://github.com/astral-sh/uv/pull/281
synced_at: 2026-01-10T15:50:28Z
```

# Add stable hash crate

---

_Pull request opened by @charliermarsh on 2023-11-01 20:38_

This PR adds a `puffin-hash` crate that we can share across a variety of other crates to generate stable hashes.

---

_Marked ready for review by @charliermarsh on 2023-11-01 20:38_

---

_@zanieb reviewed on 2023-11-01 20:40_

---

_Review comment by @zanieb on `crates/puffin-hash/src/lib.rs`:11 on 2023-11-01 20:40_

Why's it short?

---

_@zanieb approved on 2023-11-01 20:41_

---

_@MichaReiser reviewed on 2023-11-01 21:22_

Should we use a custom trait similar to ruff because not all types might produce stable hashes (e.g absolute paths are not portable)

---

_Comment by @charliermarsh on 2023-11-01 21:26_

Sure, I can do that.

---

_Merged by @charliermarsh on 2023-11-01 23:41_

---

_Closed by @charliermarsh on 2023-11-01 23:41_

---

_Branch deleted on 2023-11-01 23:41_

---
