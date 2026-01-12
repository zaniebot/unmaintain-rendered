```yaml
number: 15306
title: Install non-build-isolation packages in a second phase
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/split-no-build-isolation
created_at: 2025-08-15T13:11:26Z
updated_at: 2025-08-15T22:03:42Z
url: https://github.com/astral-sh/uv/pull/15306
synced_at: 2026-01-12T16:11:41Z
```

# Install non-build-isolation packages in a second phase

---

_@charliermarsh_

## Summary

This PR productionizes an idea I saw in https://github.com/astral-sh/uv/issues/15248, which was added in Pixi: https://github.com/prefix-dev/pixi/pull/4247. The core of the idea is that if we install all build isolation-enabled packages first, and the build isolation-disabled packages in a second phase, the sync is more likely to "just work", because if all the build dependencies of the build isolation-disabled packages are included as dependencies (as is the case for `flash-attn`, at least), they'll be present.

This isn't really a silver bullet, because it requires that all the build dependencies are included as first-party dependencies, and if you have packages that want build isolation to be disabled but rely on other packages that also require build isolation disabled, that won't work either. I think `extra-build-dependencies` will be more robust and have much better caching behavior, but this will get more cases right than our current behavior, and I don't see any downsides.

Closes https://github.com/astral-sh/uv/issues/15301.


---

_Marked ready for review by @charliermarsh on 2025-08-15 13:38_

---

_Review requested from @zanieb by @charliermarsh on 2025-08-15 13:38_

---

_Label `enhancement` added by @charliermarsh on 2025-08-15 13:38_

---

_@charliermarsh reviewed on 2025-08-15 13:39_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:1567 on 2025-08-15 13:39_

This was a bit tedious (the labels) but I think it makes a big difference.

---

_Review comment by @charliermarsh on `crates/uv-installer/src/plan.rs`:679 on 2025-08-15 13:40_

The behavior here is a bit subtle. I tried to document it clearly.

---

_@charliermarsh reviewed on 2025-08-15 13:40_

---

_@zanieb reviewed on 2025-08-15 13:47_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:1484 on 2025-08-15 13:47_

Oh! I totally missed this on a first read. Could we just move this into the `pyproject.toml`?

---

_@charliermarsh reviewed on 2025-08-15 13:53_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:1484 on 2025-08-15 13:53_

We can, the other tests also do this so I was kind of mirroring the style.

---

_@zanieb reviewed on 2025-08-15 13:54_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:1567 on 2025-08-15 13:54_

I have a couple thoughts....

1. "isolated" and "non-isolated" seem a little vague, though I think it's nice it's only present if you're using `no-build-isolation`, you could be running `uv sync` in a project where you did not configure that. I wonder if we should drop "isolated" and say "packages without build isolation" (or similar) for the non-isolated case?
2. I wonder if we can get away with repeating the prefix, e.g., if having it on "Prepared" is sufficient?

```
    Uninstalled 5 packages in [TIME]
    Prepared 1 package without build isolation in [TIME]
    Uninstalled 1 package in [TIME]
    Installed 1 package in [TIME]
```

I guess if we have a cached wheel.. it'd just say "Uninstalled" twice in a row though?

---

_@zanieb reviewed on 2025-08-15 13:59_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:1484 on 2025-08-15 13:59_

I think I'd prefer it, if there aren't invocations that don't want it.

---

_@charliermarsh reviewed on 2025-08-15 14:36_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:1567 on 2025-08-15 14:36_

I think what you have in that snippet is probably good (just putting it on "prepared").

---

_@charliermarsh reviewed on 2025-08-15 17:29_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:1484 on 2025-08-15 17:29_

I will do it in a separate PR where I change it for all of the tests in this group.

---

_@charliermarsh reviewed on 2025-08-15 17:30_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:1567 on 2025-08-15 17:30_

Fixed...

---

_@zanieb reviewed on 2025-08-15 20:31_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:1405 on 2025-08-15 20:31_

Should these be changed like the `pip_install` test?


---

_@zanieb reviewed on 2025-08-15 20:31_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:1484 on 2025-08-15 20:31_

Thanks!

---

_@zanieb approved on 2025-08-15 20:32_

---

_Merged by @charliermarsh on 2025-08-15 22:00_

---

_Closed by @charliermarsh on 2025-08-15 22:00_

---

_Branch deleted on 2025-08-15 22:00_

---

_@charliermarsh reviewed on 2025-08-15 22:03_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:1484 on 2025-08-15 22:03_

https://github.com/astral-sh/uv/pull/15319

---
