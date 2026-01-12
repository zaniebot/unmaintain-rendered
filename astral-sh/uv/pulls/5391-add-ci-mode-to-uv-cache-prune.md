```yaml
number: 5391
title: "Add `--ci` mode to `uv cache prune`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: charlie/extract
created_at: 2024-07-24T01:52:42Z
updated_at: 2024-07-24T19:35:30Z
url: https://github.com/astral-sh/uv/pull/5391
synced_at: 2026-01-12T16:06:47Z
```

# Add `--ci` mode to `uv cache prune`

---

_@charliermarsh_

## Summary

Users can now run `uv cache prune --ci` (open to feedback on the name of that flag) to remove all pre-built wheels from the cache, leaving behind zipped, built wheels (which tend to be the most expensive assets to re-create). This should greatly increase cache performance in CI environments, since uploading unzipped wheels can actually hurt performance if you're persisting the uv cache.

Closes https://github.com/astral-sh/uv/issues/5282.


---

_Label `enhancement` added by @charliermarsh on 2024-07-24 01:52_

---

_Label `cli` added by @charliermarsh on 2024-07-24 01:52_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-24 01:53_

---

_Review requested from @konstin by @charliermarsh on 2024-07-24 01:53_

---

_@charliermarsh reviewed on 2024-07-24 03:39_

---

_Review comment by @charliermarsh on `crates/uv-cache/src/removal.rs`:48 on 2024-07-24 03:39_

This is a bug. We have handling for this below (for entries), but not for the top-level thing we passed to `rm_rf`.

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:305 on 2024-07-24 07:12_

What about calling it `--ci` and documenting it as keeping only the artifacts you want to upload to the CI cache?

---

_@konstin approved on 2024-07-24 07:13_

:100:

---

_@Jorricks reviewed on 2024-07-24 08:14_

---

_Review comment by @Jorricks on `crates/uv-cli/src/lib.rs`:305 on 2024-07-24 08:14_

From a user perspective, I do feel this would be the main use-case. It would also make it immediately clear to me that I probably should use this in CI pipelines.

---

_Renamed from "Add `--all-unzipped` to `uv cache prune`" to "Add `--ci` mode to `uv cache prune`" by @charliermarsh on 2024-07-24 19:21_

---

_Comment by @zanieb on 2024-07-24 19:33_

Would you be interested in adding documentation for this to:

- `docs/cache.md`
- `docs/integration/github.md` (needs cache docs in general, okay to skip now)

It could be brief. 

---

_Merged by @charliermarsh on 2024-07-24 19:34_

---

_Closed by @charliermarsh on 2024-07-24 19:34_

---

_Branch deleted on 2024-07-24 19:34_

---

_Comment by @charliermarsh on 2024-07-24 19:35_

Yeah I can.

---
