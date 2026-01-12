```yaml
number: 3010
title: Avoid cache invalidation on credentials renewal
type: pull_request
state: merged
author: jvdw-synth
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/cache-key-without-creds-to-avoid-cache-invalidation
created_at: 2024-04-12T07:14:06Z
updated_at: 2024-04-14T17:14:29Z
url: https://github.com/astral-sh/uv/pull/3010
synced_at: 2026-01-12T16:05:22Z
```

# Avoid cache invalidation on credentials renewal

---

_@jvdw-synth_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

# Avoid cache invalidation on credentials renewal

Addresses

- https://github.com/astral-sh/uv/issues/3009#issue-2239221126

## Summary

Some private package registries (e.g. AWS CodeArtifact) use short-lived credentials. Since they are short-lived, the exact URL that is assigned to `UV_INDEX_URL` changes frequently and with that the cache key / hashes of these URLs. This causes the cache to be missed on token renewal.

This PR attempts to fix this by hashing URLs for cache keys without their user credentials.

## Test Plan

I added a test that validates that:
1. Changing user credentials returns the same hash
2. Setting no user credentials yields the same as some user credentials

## Question
I'm not sure if we should also change the `hash` implementation of `CanonicalUrl` / `RepositoryUrl`. They also run `.hash` within.

PS. this is the first time I'm writing `rust` so if I'm wasting your precious time, let me know and I'll try to up my skills before I ask again. Anyway, I figured it's good to get this issue on your radar :)

---

_Review requested from @zanieb by @konstin on 2024-04-12 07:46_

---

_Comment by @charliermarsh on 2024-04-12 13:36_

This makes sense to me... I think we probably _do_ need to change the `Hash` implementation of those structs.

---

_@zanieb reviewed on 2024-04-12 18:44_

---

_Review comment by @zanieb on `crates/cache-key/src/canonical_url.rs`:32 on 2024-04-12 18:44_

Is there any reason to do these checks? Should we just perform the `set` calls?

---

_@zanieb reviewed on 2024-04-12 18:44_

---

_Review comment by @zanieb on `crates/cache-key/src/canonical_url.rs`:26 on 2024-04-12 18:44_

I'd just have this take `self` instead of hiding the clone.

---

_Review comment by @charliermarsh on `crates/cache-key/src/canonical_url.rs`:32 on 2024-04-13 18:22_

I think we could plausibly make this method return `Cow<'_, Url>` to avoid a clone in the event that there are no credentials. (It would need to continue taking `&self`.)

---

_@charliermarsh reviewed on 2024-04-13 18:22_

---

_@charliermarsh approved on 2024-04-13 23:32_

---

_Label `bug` added by @charliermarsh on 2024-04-13 23:35_

---

_Merged by @charliermarsh on 2024-04-13 23:38_

---

_Closed by @charliermarsh on 2024-04-13 23:38_

---

_Comment by @jvdw-synth on 2024-04-14 17:14_

Thank you for taking this over! ❤️ Keep up the great work 💪  

---
