```yaml
number: 1293
title: Report incompatible distributions to users
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/no-wheel-platform
created_at: 2024-02-13T03:12:09Z
updated_at: 2024-02-15T16:48:16Z
url: https://github.com/astral-sh/uv/pull/1293
synced_at: 2026-01-10T15:33:24Z
```

# Report incompatible distributions to users

---

_Pull request opened by @zanieb on 2024-02-13 03:12_

Instead of dropping versions without a compatible distribution, we track them as incompatibilities in the solver. This implementation follows patterns established in https://github.com/astral-sh/puffin/pull/1290.

This required some significant refactoring of how we track incompatible distributions. Notably:

- `Option<TagPriority>` is now `WheelCompatibility` which allows us to track the reason a wheel is incompatible instead of just `None`.
- `Candidate` now has a `CandidateDist` with `Compatible` and `Incompatibile` variants instead of just `ResolvableDist`; candidates are not strictly compatible anymore
- `ResolvableDist` was renamed to `CompatibleDist`
- `IncompatibleWheel` was given an ordering implementation so we can track the "most compatible" (but still incompatible) wheel. This allows us to collapse the reason a version cannot be used to a single incompatibility.
- The filtering in the `VersionMap` is retained, we still only store one incompatible wheel per version. This is sufficient for error reporting.
- A `TagCompatibility` type was added for tracking which part of a wheel tag is incompatible
- `Candidate::validate_python` moved to `PythonRequirement::validate_dist` 

I am doing more refactoring in #1298 — I think a couple passes will be necessary to clarify the relationships of these types.

Includes improved error message snapshots for multiple incompatible Python tag types from #1285 — we should add more scenarios for coverage of behavior when multiple tags with different levels are present.


---

_Renamed from "zb/no wheel platform" to "Report incompatible distributions to users" by @zanieb on 2024-02-13 03:13_

---

_Label `error messages` added by @zanieb on 2024-02-13 20:08_

---

_@zanieb reviewed on 2024-02-13 20:18_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_install.rs`:77 on 2024-02-13 20:18_

This conclusion is incorrect and kind of wild. Need to debug.

---

_@zanieb reviewed on 2024-02-13 20:30_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_install.rs`:77 on 2024-02-13 20:30_

Ah this is because of `exclude_newer`... will fix.

---

_Marked ready for review by @zanieb on 2024-02-14 16:09_

---

_@zanieb reviewed on 2024-02-14 16:09_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_install.rs`:77 on 2024-02-14 16:09_

We might want to ship this as-is — exclude-newer is internal and the only way these are broken. I'm fixing in a new branch because there's a lot to do.

---

_@zanieb reviewed on 2024-02-14 22:25_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_install.rs`:77 on 2024-02-14 22:25_

Small patch merged from #1299 — larger refactor in progress at #1298 

---

_Review requested from @BurntSushi by @zanieb on 2024-02-14 23:07_

---

_Review requested from @charliermarsh by @zanieb on 2024-02-14 23:07_

---

_@zanieb reviewed on 2024-02-15 15:39_

---

_Review comment by @zanieb on `crates/distribution-types/src/prioritized_distribution.rs`:17 on 2024-02-15 15:39_

Moved down; will be removed entirely in #1298 

---

_Review comment by @BurntSushi on `crates/distribution-types/src/prioritized_distribution.rs`:254 on 2024-02-15 16:24_

I might accept a `yes: bool` parameter here. Or rename to `enable_exclude_newer`. (I usually like the former since it's a little more flexible.)

---

_Review comment by @BurntSushi on `crates/platform-tags/src/lib.rs`:44 on 2024-02-15 16:30_

Nice. I looked at this a bit to see if I could find a way that it breaks the [contract for `Ord`](https://doc.rust-lang.org/std/cmp/trait.Ord.html), but nothing came to mind. :)

---

_@BurntSushi approved on 2024-02-15 16:32_

This is such an amazing improvement to the error messages. What a win!

---

_@zanieb reviewed on 2024-02-15 16:35_

---

_Review comment by @zanieb on `crates/distribution-types/src/prioritized_distribution.rs`:254 on 2024-02-15 16:35_

👍 it's going away in the next pull request and seems fine so I'll leave it for now but keep in mind for the future

---

_Merged by @zanieb on 2024-02-15 16:48_

---

_Closed by @zanieb on 2024-02-15 16:48_

---

_Branch deleted on 2024-02-15 16:48_

---
