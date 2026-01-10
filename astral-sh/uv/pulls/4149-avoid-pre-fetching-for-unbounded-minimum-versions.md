```yaml
number: 4149
title: Avoid pre-fetching for unbounded minimum versions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/min
created_at: 2024-06-07T21:49:55Z
updated_at: 2024-06-08T12:31:13Z
url: https://github.com/astral-sh/uv/pull/4149
synced_at: 2026-01-10T13:54:02Z
```

# Avoid pre-fetching for unbounded minimum versions

---

_Pull request opened by @charliermarsh on 2024-06-07 21:49_

## Summary

I think we should be able to model PubGrub such that this isn't necessary (at least for the case described in the issue), but for now, let's just avoid attempting to build very old distributions in prefetching.

Closes https://github.com/astral-sh/uv/issues/4136.


---

_Label `bug` added by @charliermarsh on 2024-06-07 21:50_

---

_@zanieb reviewed on 2024-06-07 21:54_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:1406 on 2024-06-07 21:54_

Should we say why here?

---

_@zanieb approved on 2024-06-07 21:54_

---

_@zanieb reviewed on 2024-06-07 21:54_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:1406 on 2024-06-07 21:54_

I guess it's in the other comment "could be expensive"?

---

_@zanieb reviewed on 2024-06-07 21:58_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:1408 on 2024-06-07 21:58_

Love it :)

---

_Merged by @charliermarsh on 2024-06-07 22:05_

---

_Closed by @charliermarsh on 2024-06-07 22:05_

---

_Branch deleted on 2024-06-07 22:05_

---

_@potiuk reviewed on 2024-06-08 12:31_

---

_Review comment by @potiuk on `crates/uv-resolver/src/resolver/mod.rs`:1408 on 2024-06-08 12:31_

Nice, simple solution to a non-simple problem. Let met confirm it works :)

---
