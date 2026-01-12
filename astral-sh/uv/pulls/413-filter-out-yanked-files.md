```yaml
number: 413
title: Filter out yanked files
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: yanked
created_at: 2023-11-13T14:19:43Z
updated_at: 2023-11-13T21:08:50Z
url: https://github.com/astral-sh/uv/pull/413
synced_at: 2026-01-12T16:03:55Z
```

# Filter out yanked files

---

_@konstin_

Implement two behaviors for yanked versions:

* During `pip-compile`, yanked versions are filtered out entirely, we currently treat them is if they don't exist. This is leads to confusing error messages because a version that does exist seems to have suddenly disappeared.
* During `pip-sync`, we warn when we fetch a remote distribution and it has been yanked. We currently don't warn on cached or installed distributions that have been yanked.

---

_Converted to draft by @konstin on 2023-11-13 14:23_

---

_@charliermarsh reviewed on 2023-11-13 14:28_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/candidate_selector.rs`:124 on 2023-11-13 14:28_

We should just omit these when building the version map IMO. It'd be consistent with how we deal with non-compatible wheels.

---

_@charliermarsh reviewed on 2023-11-13 14:28_

Just asking because it's a draft and I can't tell from the summary: do you plan to finish this, or you're putting up the draft and moving on to something else? I know @zanieb self-assigned to https://github.com/astral-sh/puffin/issues/379.

---

_Comment by @charliermarsh on 2023-11-13 14:50_

I would also find it useful if we could document the intended behavior in the summary, since there is a lot of leeway for installers when dealing with yanked versions.

---

_Renamed from "WIP: Filter out yanked files" to "Filter out yanked files" by @konstin on 2023-11-13 18:41_

---

_@konstin reviewed on 2023-11-13 19:16_

---

_Review comment by @konstin on `crates/puffin-resolver/src/candidate_selector.rs`:124 on 2023-11-13 19:16_

Done


---

_Marked ready for review by @konstin on 2023-11-13 19:29_

---

_Comment by @konstin on 2023-11-13 19:32_

It's ready now, with the simple rules of banning resolving yanked versions but allowing them (with cache-dependent warning) when installing.

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_sync.rs`:155 on 2023-11-13 19:53_

Consider `let-else` with `continue` to reduce the indent depth here.

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_sync.rs`:170 on 2023-11-13 19:54_

Nit: for consistency with Ruff, let's use lowercase "warning", and the colon should also be bold.

---

_@charliermarsh approved on 2023-11-13 19:55_

---

_Comment by @charliermarsh on 2023-11-13 20:55_

(Merging to minimize conflicts.)

---

_Merged by @charliermarsh on 2023-11-13 20:58_

---

_Closed by @charliermarsh on 2023-11-13 20:58_

---

_Branch deleted on 2023-11-13 20:58_

---

_Comment by @charliermarsh on 2023-11-13 21:08_

Thanks!

---
