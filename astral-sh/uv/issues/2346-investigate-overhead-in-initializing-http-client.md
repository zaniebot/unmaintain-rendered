```yaml
number: 2346
title: Investigate overhead in initializing HTTP client
type: issue
state: closed
author: charliermarsh
labels:
  - performance
  - macos
assignees: []
created_at: 2024-03-10T20:34:29Z
updated_at: 2024-03-12T04:05:51Z
url: https://github.com/astral-sh/uv/issues/2346
synced_at: 2026-01-12T15:58:37Z
```

# Investigate overhead in initializing HTTP client

---

_@charliermarsh_

If I add debug logging before and after:

```rust
// Initialize the base client.
let client = self.client.unwrap_or_else(|| {
    // Disallow any connections.
    let client_core = ClientBuilder::new()
        .user_agent(user_agent_string)
        .pool_max_idle_per_host(20)
        .timeout(std::time::Duration::from_secs(timeout));

    client_core.build().expect("Failed to build HTTP client.")
});
```

...in the registry client, it says it takes >120ms to initialize the client. That's a ton! In many cases, it's more expensive than the resolution itself.

Did this regress at some point? My best guess would be that we regressed by reading the system certificates, but we should look into it.

---

_Label `performance` added by @charliermarsh on 2024-03-10 20:34_

---

_Comment by @charliermarsh on 2024-03-10 20:35_

Yes, confirmed, dropping `rustls-tls-native-roots` removes this overhead.

---

_Comment by @charliermarsh on 2024-03-10 21:39_

I would be curious if this reproduces on Linux at all, or if it's macOS-only.

---

_Comment by @charliermarsh on 2024-03-10 22:02_

Either way, I’m going to make the client initialization lazy.

---

_Comment by @konstin on 2024-03-11 12:41_

Is that mac specific or do i need to set an environment variable? I tried to reproduce but client building is 4ms on my ubuntu machine.

---

_Comment by @charliermarsh on 2024-03-11 12:42_

I believe it’s specific to macOS.

---

_Label `macos` added by @konstin on 2024-03-11 13:22_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-11 13:54_

---

_Closed by @charliermarsh on 2024-03-12 04:05_

---
