```yaml
number: 11448
title: Split uv-git and uv-git-types
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/optional-reqwests
created_at: 2025-02-12T15:59:24Z
updated_at: 2025-02-17T09:37:58Z
url: https://github.com/astral-sh/uv/pull/11448
synced_at: 2026-01-10T11:10:38Z
```

# Split uv-git and uv-git-types

---

_Pull request opened by @konstin on 2025-02-12 15:59_

We want to build `uv-build` without depending on the network crates. In preparation for that, we split uv-git into uv-git and uv-git-types, where only uv-git depends on reqwest, so that uv-build can use uv-git-types.

---

_Label `internal` added by @konstin on 2025-02-12 15:59_

---

_Review requested from @Gankra by @zanieb on 2025-02-12 16:00_

---

_Comment by @charliermarsh on 2025-02-12 16:03_

Is the network feature just used for GitHub fast path thing?

---

_Comment by @konstin on 2025-02-12 16:05_

The network feature will be deactivated only for `uv-build`, so uv-build won't have a network dependency.

---

_Comment by @charliermarsh on 2025-02-12 16:05_

👍 Just wondering where they're actually used? Is it just that one route?

---

_Comment by @konstin on 2025-02-12 16:38_

Yes, in another branch, i found that we were pulling in reqwest through uv-git, which was pulled in by uv-pypi-types. Since the build backend does not need any networking features currently (and likely not in the future) and the networking is known to cause build problems, often through the TLS backend, this refactoring makes the upcoming build backend crate build without reqwest.

---

_Comment by @charliermarsh on 2025-02-12 16:42_

That all makes sense! As a reviewer, I'm just trying to understand, like, where `uv-git` uses `reqwest`, and which parts of `uv-git` are needed by the build backend. I'm not objecting to the change, I just want that context.

---

_Review comment by @Gankra on `crates/uv-git/src/reference.rs`:70 on 2025-02-12 20:26_

why does this one need to be cfg'd? dead code lints?

---

_@Gankra approved on 2025-02-12 20:28_

Not applicable to this PR specifically but it's worth noting that testing/building uv-build properly is going to be annoyingly hard, because if you ever build the --workspace it will implicitly have the "network" flipped on by build feature unification.

You need to explicitly build+test it separately from everything else to avoid picking up bonus features it doesn't ask for.

---

_@charliermarsh reviewed on 2025-02-12 20:46_

---

_Review comment by @charliermarsh on `crates/uv-git/src/reference.rs`:70 on 2025-02-12 20:46_

:thumbsup: Probably makes sense to not cfg this and just allow unused on it

---

_Comment by @charliermarsh on 2025-02-12 20:47_

I guess the other option would be to split it into two crates?

---

_Renamed from "Optional reqwest in uv-git" to "Split uv-git and uv-git-types" by @konstin on 2025-02-13 10:48_

---

_Comment by @konstin on 2025-02-13 11:33_

Split the crate into two, much nicer now.

Figures: The uv workspace inter-crate dependencies, with transitive edges removes (`cargo depgraph --dedup-transitive-deps --workspace-only | dot -Tpng`).

Before:

![image](https://github.com/user-attachments/assets/239c8947-7916-4427-bd8e-bbc2a55d9122)

After:

![image](https://github.com/user-attachments/assets/8005222b-edc4-4b43-abee-c0821da55276)


---

_@charliermarsh approved on 2025-02-13 14:48_

Great, thanks!

---

_Comment by @charliermarsh on 2025-02-13 14:48_

I think this is easier to understand (and will be easier to test and maintain).

---

_Merged by @konstin on 2025-02-17 09:37_

---

_Closed by @konstin on 2025-02-17 09:37_

---

_Branch deleted on 2025-02-17 09:37_

---
