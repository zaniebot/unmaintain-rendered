```yaml
number: 6776
title: "fix: replace std::fs::canonicalize with Simplified::simple_canonicaliz…"
type: pull_request
state: merged
author: cning112
labels:
  - windows
assignees: []
merged: true
base: main
head: fix-canonical-path-of-net-drive-on-windows
created_at: 2024-08-28T21:18:13Z
updated_at: 2024-09-01T17:59:16Z
url: https://github.com/astral-sh/uv/pull/6776
synced_at: 2026-01-10T12:53:34Z
```

# fix: replace std::fs::canonicalize with Simplified::simple_canonicaliz…

---

_Pull request opened by @cning112 on 2024-08-28 21:18_

## Summary
This PR addresses an issue on Windows where `std::fs::canonicalize` can fail or panic when resolving paths on mapped network drives. By replacing it with `Simplified::simple_canonicalize`, we aim to improve the robustness and cross-platform compatibility of path resolution.

### Changes
* Updated `CANONICAL_CWD` in `path.rs` to use `Simplified::simple_canonicalize` instead of `std::fs::canonicalize`.

### Why
* `std::fs::canonicalize` has known issues with resolving paths on mapped network drives on Windows, which can lead to panics or incorrect path resolution.
* `Simplified::simple_canonicalize` internally uses `dunce::canonicalize`, which handles these cases more gracefully, ensuring better stability and reliability.

## Test Plan
Since `simple_canonicalize` has already been tested in a prior PR, this change is expected to work without introducing any new issues. No additional tests are necessary beyond ensuring existing tests pass, which will confirm the correctness of the change.


---

_Renamed from "fix: replace std::fs::canonicalize with Simplifed::simple_canonicaliz…" to "fix: replace std::fs::canonicalize with Simplified::simple_canonicaliz…" by @cning112 on 2024-08-29 06:46_

---

_Label `windows` added by @charliermarsh on 2024-08-29 19:44_

---

_Comment by @charliermarsh on 2024-08-29 19:45_

Interesting. But doesn't `dunce::canonicalize` just call `fs::canonicalize` under the hood? My read of the code is it's just `fs::canonicalize` with a post-processing step to remove the prefixes.

---

_Comment by @cning112 on 2024-08-30 00:01_

Absolutely true. Paths that can't be canonicalized by `fs:: canonicalize` are also beyond the capability of `dunce:: canonicalize`.

In the latest commit, I updated the `Simplified::simple_canonicalize` for paths that can't be canonicalised on Windows: it falls back on `std::path::absolute` and verifies if the path exists.

I've tested it locally on a mapped network drive on Windows, where the latest version of `uv pip compile` panicked as the path can't be canonicalized. But I am not sure how to add a unit test for this 😬 



---

_Comment by @charliermarsh on 2024-09-01 17:59_

Ok, I think something like this is reasonable, but I may follow-up to reduce the scope a bit.

---

_Merged by @charliermarsh on 2024-09-01 17:59_

---

_Closed by @charliermarsh on 2024-09-01 17:59_

---
