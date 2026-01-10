```yaml
number: 17104
title: Fix retry counts in cached client
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/better-retry-handling-4
created_at: 2025-12-12T17:01:20Z
updated_at: 2025-12-18T10:51:02Z
url: https://github.com/astral-sh/uv/pull/17104
synced_at: 2026-01-10T05:49:14Z
```

# Fix retry counts in cached client

---

_Pull request opened by @konstin on 2025-12-12 17:01_

Previously, we dropped the counts from the middleware layer, potentially doing to many retries and/or reporting too few.

Not pretty but fixes the bug.


---

_Review requested from @EliteTK by @zanieb on 2025-12-12 18:18_

---

_Review comment by @EliteTK on `crates/uv-client/src/cached_client.rs`:711 on 2025-12-15 13:34_

nitpick: I think the second comment doesn't make much sense now the addition (consideration) has moved.

```suggestion
                Err(err) => {
                    // Check if the middleware already performed retries and consider it in our retry budget
                    total_retries += err.retries().unwrap_or_default();
                    if is_transient_network_error(err.error()) {
                        let retry_decision = retry_policy.should_retry(start_time, total_retries);
```

---

_@konstin reviewed on 2025-12-15 13:46_

---

_Review comment by @konstin on `crates/uv-client/src/cached_client.rs`:129 on 2025-12-15 13:46_

Removed the optional as zero retries is indistinguishable from `None`.

---

_Label `bug` added by @konstin on 2025-12-15 13:46_

---

_Marked ready for review by @konstin on 2025-12-15 13:46_

---

_Review comment by @EliteTK on `crates/uv-client/src/cached_client.rs`:734 on 2025-12-15 15:38_

```suggestion
                    return Err(err.with_retries(total_retries));
```

Since the retry count is non-optional, conditionally doing `with_retries` here is now somewhat meaningless.

---

_@EliteTK approved on 2025-12-15 17:16_

I'm a little bit too unfamiliar with the underlying code to understand why you consider this to be a hack but I've given it a thorough looking over and I can't see anything that stands out.

The surrounding code may benefit from some future rework, but I haven't looked deeply into it here as it's not really related to this specific PR.

---

_@konstin reviewed on 2025-12-15 17:46_

---

_Review comment by @konstin on `crates/uv-client/src/cached_client.rs`:711 on 2025-12-15 17:46_

Both of those should be better in the upstack PR: https://github.com/astral-sh/uv/pull/17105/changes#diff-554df8b1efa8f690930e0d00e1c7302751a6e9193fd812a9b1f7305859ceee29R1171-R1201

---

_@EliteTK reviewed on 2025-12-15 17:47_

---

_Review comment by @EliteTK on `crates/uv-client/src/cached_client.rs`:711 on 2025-12-15 17:47_

Will look at it tomorrow.

---

_Merged by @konstin on 2025-12-18 10:51_

---

_Closed by @konstin on 2025-12-18 10:51_

---

_Branch deleted on 2025-12-18 10:51_

---
