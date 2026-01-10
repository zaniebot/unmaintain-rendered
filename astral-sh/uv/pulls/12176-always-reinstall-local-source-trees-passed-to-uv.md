```yaml
number: 12176
title: "Always reinstall local source trees passed to `uv pip install`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cache
  - no-build
assignees: []
merged: true
base: main
head: charlie/explicit
created_at: 2025-03-14T20:15:13Z
updated_at: 2025-03-17T21:12:22Z
url: https://github.com/astral-sh/uv/pull/12176
synced_at: 2026-01-10T11:10:39Z
```

# Always reinstall local source trees passed to `uv pip install`

---

_Pull request opened by @charliermarsh on 2025-03-14 20:15_

## Summary

This ended up being more involved than expected. The gist is that we setup all the packages we want to reinstall upfront (they're passed in on the command-line); but at that point, we don't have names for all the packages that the user has specified. (Consider, e.g., `uv pip install .` -- we don't have a name for `.`, so we can't add it to the list of `Reinstall` packages.)

Now, `Reinstall` also accepts paths, so we can augment `Reinstall` based on the user-provided paths.

Closes #12038.


---

_Label `no-build` added by @charliermarsh on 2025-03-14 20:15_

---

_Label `enhancement` added by @charliermarsh on 2025-03-14 20:16_

---

_Label `cache` added by @charliermarsh on 2025-03-14 20:16_

---

_Review requested from @zanieb by @charliermarsh on 2025-03-14 20:16_

---

_Review requested from @konstin by @charliermarsh on 2025-03-14 20:16_

---

_@konstin approved on 2025-03-17 10:38_

---

_@zanieb reviewed on 2025-03-17 18:39_

---

_Review comment by @zanieb on `crates/uv-cache/src/lib.rs`:264 on 2025-03-17 18:39_

On failure we assume `true` and always invalidate? That seems surprising.

---

_Review comment by @zanieb on `crates/uv-configuration/src/package_options.rs`:67 on 2025-03-17 18:40_

See previous comment

---

_@zanieb reviewed on 2025-03-17 18:40_

---

_@zanieb approved on 2025-03-17 18:42_

The pull request says "source trees" but this implementation is just **local** source trees, right? Can you change that?

Do we want to change this for remote source trees, i.e., Git repositories too? Should we add test coverage for that now?

---

_@zanieb reviewed on 2025-03-17 18:43_

---

_Review comment by @zanieb on `crates/uv-cache/src/lib.rs`:264 on 2025-03-17 18:43_

(I don't feel strongly, but am curious for your thinking here)

---

_Review comment by @charliermarsh on `crates/uv-cache/src/lib.rs`:264 on 2025-03-17 18:46_

Removed it. I didn't realize it errors when one file doesn't exist; I just didn't think about it deeply, thanks!

---

_@charliermarsh reviewed on 2025-03-17 18:46_

---

_Comment by @charliermarsh on 2025-03-17 18:46_

Yeah, just local source trees.

---

_Renamed from "Always reinstall source trees passed to `uv pip install`" to "Always reinstall local source trees passed to `uv pip install`" by @zanieb on 2025-03-17 19:03_

---

_Merged by @charliermarsh on 2025-03-17 21:12_

---

_Closed by @charliermarsh on 2025-03-17 21:12_

---

_Branch deleted on 2025-03-17 21:12_

---
