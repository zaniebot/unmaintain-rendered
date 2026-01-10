```yaml
number: 2949
title: Fall back to distributions without hashes in resolver
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/check-hashes-v
created_at: 2024-04-09T23:12:11Z
updated_at: 2024-04-10T19:19:48Z
url: https://github.com/astral-sh/uv/pull/2949
synced_at: 2026-01-10T14:43:31Z
```

# Fall back to distributions without hashes in resolver

---

_Pull request opened by @charliermarsh on 2024-04-09 23:12_

## Summary

This represents a change to `--require-hashes` in the event that we don't find a matching hash from the registry. The behavior in this PR is closer to pip's.

Prior to this PR, if a distribution had no reported hash, or only mismatched hashes, we would mark it as incompatible. Now, we mark it as compatible, but we use the hash-agreement as part of the ordering, such that we prefer any distribution with a matching hash, then any distribution with no hash, then any distribution with a mismatched hash.

As a result, if an index reports incorrect hashes, but the user provides the correct one, resolution now succeeds, where it would've failed.

Similarly, if an index omits hashes altogether, but the user provides the correct one, resolution now succeeds, where it would've failed.

If we end up picking a distribution whose hash ultimately doesn't match, we'll reject it later, after resolution.


---

_Label `enhancement` added by @charliermarsh on 2024-04-09 23:12_

---

_Marked ready for review by @charliermarsh on 2024-04-09 23:12_

---

_Review requested from @zanieb by @charliermarsh on 2024-04-09 23:13_

---

_@charliermarsh reviewed on 2024-04-09 23:16_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:4305 on 2024-04-09 23:16_

These aren't as nice-looking, but keep in mind that for `--require-hashes`, the user has to pin _all_ dependencies upfront anyway. There's no backtracking or anything.

---

_@zanieb reviewed on 2024-04-10 17:37_

---

_Review comment by @zanieb on `crates/uv/tests/pip_sync.rs`:4305 on 2024-04-10 17:37_

They're pretty ugly but 🤷‍♂️ we could address later if we want.

---

_@zanieb approved on 2024-04-10 17:38_

Yeah this UX seems much better.

---

_@charliermarsh reviewed on 2024-04-10 17:49_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:4305 on 2024-04-10 17:49_

Absolutely DESTROYING my error messages

---

_Merged by @charliermarsh on 2024-04-10 19:19_

---

_Closed by @charliermarsh on 2024-04-10 19:19_

---

_Branch deleted on 2024-04-10 19:19_

---
