```yaml
number: 12717
title: Use index-level instead of realm-level credential caching for known indexes
type: pull_request
state: merged
author: jtfmumm
labels:
  - network
assignees: []
merged: true
base: jtfm/index-url-auth
head: jtfm/index-level-credential-cache
created_at: 2025-04-07T14:32:03Z
updated_at: 2025-04-10T09:52:41Z
url: https://github.com/astral-sh/uv/pull/12717
synced_at: 2026-01-10T11:10:40Z
```

# Use index-level instead of realm-level credential caching for known indexes

---

_Pull request opened by @jtfmumm on 2025-04-07 14:32_

The current uv behavior is to cache credentials either at the request URL or realm level. But in general, the expected behavior for indexes is to apply credentials at the index level (as implemented in #12651). This means that we also need to cache credentials at this level. Note that when uv does not detect an index URL for a request URL, it will continue to apply the old behavior.

Depends on #12651. 


---

_Label `network` added by @jtfmumm on 2025-04-07 14:32_

---

_Review requested from @zanieb by @jtfmumm on 2025-04-07 16:18_

---

_Comment by @jtfmumm on 2025-04-07 16:19_

@zanieb We can either merge this into [jtfm/index-url-auth](https://github.com/astral-sh/uv/tree/jtfm/index-url-auth) if approved or sequence it after #12651.

---

_@zanieb reviewed on 2025-04-07 23:03_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:290 on 2025-04-07 23:03_

If there's no cache entry for the index, should we fall back to the realm still? I would think so, for cases where a file is hosted elsewhere on the realm?

---

_@jtfmumm reviewed on 2025-04-08 10:41_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/middleware.rs`:290 on 2025-04-08 10:41_

The problem is that two indexes could have different credentials but the same realm. If both indexes have files hosted elsewhere on the realm, we could only cache the correct credentials at the realm level for one of them. That seems confusing.

But if we did want to fall back to the realm, we couldn't do it here. Instead, we'd need to check the realm cache again after the credentials fetch step. Otherwise uv will potentially find the wrong credentials here and use those without, e.g., checking the keyring for the index URL. There's a test in this PR that covers that case.

---

_@zanieb reviewed on 2025-04-08 18:29_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:290 on 2025-04-08 18:29_

> The problem is that two indexes could have different credentials but the same realm. If both indexes have files hosted elsewhere on the realm, we could only cache the correct credentials at the realm level for one of them. That seems confusing.

I agree it seems confusing, but it's also better than not authenticating at all, right? I think the more common case is that you have a single index per realm — mixing indexes per realm is rare. As-is, this could be a regression from our existing behavior.

> Otherwise uv will potentially find the wrong credentials here and use those without, e.g., checking the keyring for the index URL.

I agree we shouldn't use the realm-level cache until we've checked for credentials at the index-level.

---

_@jtfmumm reviewed on 2025-04-09 08:15_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/middleware.rs`:290 on 2025-04-09 08:15_

Ok, I've added realm-level cache check as a final fallback for an index URL (and a new test to check this behavior)

---

_Merged by @jtfmumm on 2025-04-10 09:52_

---

_Closed by @jtfmumm on 2025-04-10 09:52_

---

_Branch deleted on 2025-04-10 09:52_

---
