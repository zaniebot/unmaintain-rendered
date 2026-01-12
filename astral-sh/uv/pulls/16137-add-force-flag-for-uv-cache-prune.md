```yaml
number: 16137
title: "Add `--force` flag for `uv cache prune`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/cache-prune-force
created_at: 2025-10-06T14:30:05Z
updated_at: 2025-10-06T16:37:00Z
url: https://github.com/astral-sh/uv/pull/16137
synced_at: 2026-01-12T16:12:07Z
```

# Add `--force` flag for `uv cache prune`

---

_@zanieb_

Matching #15992 

See https://github.com/astral-sh/setup-uv/issues/588

---

_Comment by @konstin on 2025-10-06 14:32_

How does this compare to releasing the lock, specifically for the case where people start background services in CI (the approach from https://github.com/astral-sh/uv/pull/15977#discussion_r2368145982)?

---

_Comment by @zanieb on 2025-10-06 14:43_

This ignores a held lock, so you can force clean of the cache if it's held.

I don't think releasing the lock is trivially safe, as I described in https://github.com/astral-sh/uv/pull/15990  and https://github.com/astral-sh/uv/pull/15977#issuecomment-3321919449 — I think we can consider doing so anyway, but I'd like that to be a separate holistic discussion instead of blocking this pull request which is just adding `--force` to `prune` for parity with `clean`.

---

_Label `cli` added by @zanieb on 2025-10-06 14:43_

---

_Comment by @zanieb on 2025-10-06 14:45_

I think @geofft is right that we may need to do something more advanced long-term, from [Discord](https://discord.com/channels/1039017663004942429/1343693513073623205/1419781277803876465)

---

_Comment by @konstin on 2025-10-06 14:57_

I'm not saying that `--force` is bad on its own, but from https://github.com/astral-sh/setup-uv/issues/588 it seems we're designing a footgun for CI use cases instead of releasing the lock that causes this problem.

---

_Comment by @zanieb on 2025-10-06 15:46_

I can only ask again that we take that conversation elsewhere — I think we should consider an alternative solution holistically and outside of this change. I don't see `--force` as inappropriate in the CI context after you've left a uv process dangling.

---

_@konstin approved on 2025-10-06 16:32_

---

_Comment by @zanieb on 2025-10-06 16:36_

Thank you!

---

_Merged by @zanieb on 2025-10-06 16:36_

---

_Closed by @zanieb on 2025-10-06 16:36_

---

_Branch deleted on 2025-10-06 16:37_

---
