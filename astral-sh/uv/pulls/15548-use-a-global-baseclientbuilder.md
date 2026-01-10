```yaml
number: 15548
title: "Use a global `BaseClientBuilder`"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/global-client-builder
created_at: 2025-08-27T12:04:07Z
updated_at: 2025-09-03T08:35:49Z
url: https://github.com/astral-sh/uv/pull/15548
synced_at: 2026-01-10T06:44:33Z
```

# Use a global `BaseClientBuilder`

---

_Pull request opened by @konstin on 2025-08-27 12:04_

Alternative to #15105

Instead of building a `BaseClientBuilder` from `NetworkSettings` each time we need a client, we instead build a single `BaseClientBuilder` and pass it around. The `RegistryClientBuilder` then uses `BaseClientBuilder` exclusively for configuration. This removes a chunk of copy-and-paste code, and also moves the fallible `retries_from_env` into a single place

Borrow vs. clone is mostly ad-hoc, we can change it in either direction if it matters.

Closes #15105

---

_Review requested from @charliermarsh by @konstin on 2025-08-27 12:04_

---

_Review requested from @zanieb by @konstin on 2025-08-27 12:04_

---

_Label `internal` added by @konstin on 2025-08-27 12:04_

---

_@konstin reviewed on 2025-08-27 12:05_

---

_Review comment by @konstin on `crates/uv-client/src/registry_client.rs`:59 on 2025-08-27 12:05_

An alternative in one direction would be starting with a default base client builder (easier for the tests, easier to get wrong in production code), an alternative in the other direction would be making `IndexLocations` a parameter here to clarify them as mandatory.

---

_@konstin reviewed on 2025-08-27 12:08_

---

_Review comment by @konstin on `crates/uv/src/commands/project/add.rs`:394 on 2025-08-27 12:08_

This is a logic change as it was previously (accidentally?) using a temporary cache.

---

_@konstin reviewed on 2025-08-27 12:09_

---

_Review comment by @konstin on `crates/uv-client/src/registry_client.rs`:250 on 2025-08-27 12:09_

We avoid this fallible construction by passing the cache in explicitly. This avoids creating temp dirs we drop immediately after.

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/list.rs`:90 on 2025-08-29 18:19_

I worry about the friction this adds to adding a keyring provider, i.e., that it makes it easier to forget. I'm not sure what we can do though, it's not substantively worse than the status quo.

---

_@zanieb reviewed on 2025-08-29 18:19_

---

_@zanieb approved on 2025-08-29 18:30_

Alrighty

---

_Merged by @zanieb on 2025-08-29 18:30_

---

_Closed by @zanieb on 2025-08-29 18:30_

---

_Branch deleted on 2025-08-29 18:30_

---

_@konstin reviewed on 2025-09-03 08:35_

---

_Review comment by @konstin on `crates/uv/src/commands/pip/list.rs`:90 on 2025-09-03 08:35_

That's a motivation for this change! I want to move towards something that's more consolidated instead of having to remember to add each field to the builder each time. I looked at the keyring quickly, but it seemed to complex to pull out for now.

---
