```yaml
number: 15825
title: "Rename `provides_extras` to `provides_extra`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/provides
created_at: 2025-09-13T02:52:04Z
updated_at: 2025-09-14T13:27:46Z
url: https://github.com/astral-sh/uv/pull/15825
synced_at: 2026-01-12T16:11:57Z
```

# Rename `provides_extras` to `provides_extra`

---

_@charliermarsh_

## Summary

This is now consistent with `requires_dist` (singular).


---

_Review requested from @konstin by @charliermarsh on 2025-09-13 02:52_

---

_Label `internal` added by @charliermarsh on 2025-09-13 02:52_

---

_Marked ready for review by @charliermarsh on 2025-09-13 02:52_

---

_Review comment by @konstin on `crates/uv-client/src/registry_client.rs`:1248 on 2025-09-13 12:51_

Should we rename `metadata.provides_extra` too?

---

_@konstin reviewed on 2025-09-13 12:51_

---

_@konstin approved on 2025-09-13 12:51_

---

_Review requested from @konstin by @charliermarsh on 2025-09-13 16:14_

---

_Comment by @charliermarsh on 2025-09-13 16:15_

@konstin -- Do you mind skimming again? I had to change this on a bunch of other structs once I followed the thread from your comment.

---

_Comment by @konstin on 2025-09-14 11:31_

Thanks, I renamed the tiny remaining items.

---

_Comment by @konstin on 2025-09-14 11:35_

I think the PR is ready to go as-is.

Since we're already bumping the cache version, we should decide before the next release whether we also want to change `package.metadata.provides-extras` in `uv.lock` to `package.metadata.provides-extra` (minor version bump) and `provides-extras` in `tool.uv.dependency-metadata` to `provides-extra`. The latter would happen without dropping support for the `provides-extras` alias, we only need to update the docs to recommend the singular term. I'd do those in follow-up PRs for size and release notes.

---

_@konstin approved on 2025-09-14 11:36_

---

_Comment by @charliermarsh on 2025-09-14 13:14_

I think I did change `tool.uv.dependency-metadata` to use `provides-extra` so let me just update the docs there (without removing the alias).


---

_Merged by @charliermarsh on 2025-09-14 13:27_

---

_Closed by @charliermarsh on 2025-09-14 13:27_

---

_Branch deleted on 2025-09-14 13:27_

---
