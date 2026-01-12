```yaml
number: 5863
title: Avoid panic when re-locking with precise commit
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/lock-git
created_at: 2024-08-07T14:38:58Z
updated_at: 2024-08-07T14:56:16Z
url: https://github.com/astral-sh/uv/pull/5863
synced_at: 2026-01-12T16:07:04Z
```

# Avoid panic when re-locking with precise commit

---

_@charliermarsh_

## Summary

Very subtle bug. The scenario is as follows:

- We resolve: `elmer-circuitbuilder = { git = "https://github.com/ElmerCSC/elmer_circuitbuilder.git" }`

- The user then changes the request to: `elmer-circuitbuilder = { git = "https://github.com/ElmerCSC/elmer_circuitbuilder.git", rev = "44d2f4b19d6837ea990c16f494bdf7543d57483d" }`

- When we go to re-lock, we note two facts:

  1. The "default branch" resolves to `44d2f4b19d6837ea990c16f494bdf7543d57483d`.
  2. The metadata for `44d2f4b19d6837ea990c16f494bdf7543d57483d` is (whatever we grab from the lockfile).

- In the resolver, we then ask for the metadata for `44d2f4b19d6837ea990c16f494bdf7543d57483d`. It's already in the cache, so we return it; thus, we never add the `44d2f4b19d6837ea990c16f494bdf7543d57483d` -> `44d2f4b19d6837ea990c16f494bdf7543d57483d` mapping to the Git resolver, because we never have to resolve it.

This would apply for any case in which a requested tag or branch was replaced by its precise SHA. Replacing with a different commit is fine.

It only applied to `tool.uv.sources`, and not PEP 508 URLs, because the underlying issue is that we aren't consistent about "automatically" extracting the precise commit from a Git reference.

Closes https://github.com/astral-sh/uv/issues/5860.


---

_Review comment by @charliermarsh on `crates/uv-git/src/lib.rs`:30 on 2024-08-07 14:39_

Main change: we now always extract precise if it's a Git reference.

---

_@charliermarsh reviewed on 2024-08-07 14:39_

---

_Label `bug` added by @charliermarsh on 2024-08-07 14:39_

---

_Label `preview` added by @charliermarsh on 2024-08-07 14:39_

---

_Merged by @charliermarsh on 2024-08-07 14:56_

---

_Closed by @charliermarsh on 2024-08-07 14:56_

---

_Branch deleted on 2024-08-07 14:56_

---
