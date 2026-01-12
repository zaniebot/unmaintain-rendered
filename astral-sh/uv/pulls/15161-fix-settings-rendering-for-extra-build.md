```yaml
number: 15161
title: "Fix settings rendering for `extra-build-dependencies`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/s
created_at: 2025-08-08T09:37:06Z
updated_at: 2025-09-01T14:07:27Z
url: https://github.com/astral-sh/uv/pull/15161
synced_at: 2026-01-12T16:11:36Z
```

# Fix settings rendering for `extra-build-dependencies`

---

_@charliermarsh_

## Summary

It would be nice if this rendered as `[tool.uv.extra-build-dependencies]` and `[extra-build-dependencies]` (in `uv.toml`), but this is at least correct.

Closes https://github.com/astral-sh/uv/issues/15124.


---

_Label `documentation` added by @charliermarsh on 2025-08-08 09:37_

---

_Review requested from @zanieb by @charliermarsh on 2025-08-08 09:37_

---

_Marked ready for review by @charliermarsh on 2025-08-08 09:37_

---

_@charliermarsh reviewed on 2025-08-08 09:38_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject.rs`:458 on 2025-08-08 09:38_

I think these were unread and basically duplicated from the `ResolverSettings`.

---

_@konstin approved on 2025-08-08 09:43_

---

_@charliermarsh reviewed on 2025-08-08 10:51_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:12768 on 2025-08-08 10:51_

I guess this is "right" but it's kind of weird (we only warn, not fail, when we fail to parse options from a `pyproject.toml`). Before, we were both warning and erroring because these settings were parsed twice, once as `pyproject.toml` options and once as project settings.

---

_@zanieb reviewed on 2025-08-08 11:22_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:12768 on 2025-08-08 11:22_

Hm. I think I'd prefer the hard error still here. Does that mean we need to enforce it later? I think this is not a "parse" error.

---

_@charliermarsh reviewed on 2025-08-08 12:18_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:12768 on 2025-08-08 12:18_

I guess, yeah.

---

_Merged by @charliermarsh on 2025-08-11 21:24_

---

_Closed by @charliermarsh on 2025-08-11 21:24_

---

_Branch deleted on 2025-08-11 21:24_

---

_@charliermarsh reviewed on 2025-08-11 21:24_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:12768 on 2025-08-11 21:24_

Oh shoot sorry, I forgot about this.

---

_Branch restored on 2025-08-11 21:24_

---

_@charliermarsh reviewed on 2025-08-11 21:25_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:12768 on 2025-08-11 21:25_

I opened a revert here: https://github.com/astral-sh/uv/pull/15228. Unless you think this is a net improvement for the release.

---

_@zanieb reviewed on 2025-08-12 00:37_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:12768 on 2025-08-12 00:37_

I had intentionally not been merging. I'm a bit on the fence, but let's just avoid the churn for users — I merged the revert.

---

_@charliermarsh reviewed on 2025-09-01 14:07_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:12768 on 2025-09-01 14:07_

The sad thing about enforcing it "later" is that we won't be able to show you the source file because it'll no longer come from deserialization. It's nice that, right now, we can show you the exact line.

---
