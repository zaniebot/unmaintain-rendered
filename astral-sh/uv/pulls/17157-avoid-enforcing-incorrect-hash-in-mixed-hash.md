```yaml
number: 17157
title: Avoid enforcing incorrect hash in mixed-hash settings
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/mixed-hash
created_at: 2025-12-17T02:08:23Z
updated_at: 2025-12-17T16:02:01Z
url: https://github.com/astral-sh/uv/pull/17157
synced_at: 2026-01-12T16:12:38Z
```

# Avoid enforcing incorrect hash in mixed-hash settings

---

_@charliermarsh_

## Summary

Right now, when we return a `Dist` from a lockfile, we concatenate all hashes for all distributions for a given package. In the case of https://github.com/astral-sh/uv/issues/17143, I think that means we'll return the SHA256 from the sdist, plus the SHA512 from the wheel. If the wheel was previously installed (i.e., it's in the cache), and we computed the SHA256 at that point in time, then `Hashed::has_digests` would return `true` because we have _at least_ one SHA256. We now limit the hashes to the distribution that we expect to install.

Closes https://github.com/astral-sh/uv/issues/17143.


---

_Label `bug` added by @charliermarsh on 2025-12-17 02:08_

---

_Review requested from @konstin by @charliermarsh on 2025-12-17 02:08_

---

_Marked ready for review by @charliermarsh on 2025-12-17 02:08_

---

_Comment by @charliermarsh on 2025-12-17 02:10_

Alternatively, we can get rid of `has_digests` and just use `satisfies` when we load wheels from the cache. That would also fix this issue, though the change here felt more structurally correct.

---

_@konstin approved on 2025-12-17 10:07_

Can you add a test for that that mixed hashes situation where there's a SHA256 here and a SHA512 there?

---

_Comment by @charliermarsh on 2025-12-17 13:00_

Do you have any advice on how to do that easily?

---

_Comment by @konstin on 2025-12-17 15:21_

A mockserver with a find links page that links to three entries in our `test/links`, one entry with SHA256, one with SHA512 and one with both.

---

_Merged by @charliermarsh on 2025-12-17 16:02_

---

_Closed by @charliermarsh on 2025-12-17 16:02_

---

_Branch deleted on 2025-12-17 16:02_

---
