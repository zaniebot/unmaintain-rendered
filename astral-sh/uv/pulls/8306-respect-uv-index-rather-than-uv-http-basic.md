```yaml
number: 8306
title: "Respect `UV_INDEX_` rather than `UV_HTTP_BASIC_`"
type: pull_request
state: merged
author: odie5533
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-env-var-credentials-docs
created_at: 2024-10-17T21:14:21Z
updated_at: 2024-10-17T21:42:05Z
url: https://github.com/astral-sh/uv/pull/8306
synced_at: 2026-01-12T16:08:15Z
```

# Respect `UV_INDEX_` rather than `UV_HTTP_BASIC_`

---

_@odie5533_

The docs reference `UV_INDEX_`, but the code actually uses UV_HTTP_BASIC_ as the prefix for environment variable credentials.

See PR #7741

Code is at https://github.com/astral-sh/uv/blob/main/crates/uv-static/src/env_vars.rs#L163

```rust
    /// Generates the environment variable key for the HTTP Basic authentication username.
    pub fn http_basic_username(name: &str) -> String {
        format!("UV_HTTP_BASIC_{name}_USERNAME")
    }

    /// Generates the environment variable key for the HTTP Basic authentication password.
    pub fn http_basic_password(name: &str) -> String {
        format!("UV_HTTP_BASIC_{name}_PASSWORD")
    }
```

---

_@zanieb reviewed on 2024-10-17 21:16_

---

_Review comment by @zanieb on `docs/configuration/indexes.md`:137 on 2024-10-17 21:16_

@charliermarsh I can't remember which one of these we thought was better. Wasn't it the `UV_INDEX_` prefix?

---

_@zanieb reviewed on 2024-10-17 21:16_

---

_Review comment by @zanieb on `docs/configuration/indexes.md`:137 on 2024-10-17 21:16_

(That's also what's mentioned in the PR summary, so I presume the code is wrong here)

---

_Assigned to @charliermarsh by @zanieb on 2024-10-17 21:17_

---

_@charliermarsh reviewed on 2024-10-17 21:18_

---

_Review comment by @charliermarsh on `docs/configuration/indexes.md`:137 on 2024-10-17 21:18_

Oh I think I meant to do `UV_HTTP_BASIC` but honestly I can't remember

---

_@zanieb reviewed on 2024-10-17 21:20_

---

_Review comment by @zanieb on `docs/configuration/indexes.md`:137 on 2024-10-17 21:20_

I thought we were intentionally deviating from what Poetry does?

---

_@charliermarsh reviewed on 2024-10-17 21:24_

---

_Review comment by @charliermarsh on `docs/configuration/indexes.md`:137 on 2024-10-17 21:24_

I don't remember honestly. We can't easily change it now that it's out there though -- it would be breaking.

---

_@odie5533 reviewed on 2024-10-17 21:26_

---

_Review comment by @odie5533 on `docs/configuration/indexes.md`:137 on 2024-10-17 21:26_

It's possible I might be the only one that's used it successfully though 🤣 
and I don't mind you breaking it on me

---

_Review comment by @charliermarsh on `docs/configuration/indexes.md`:137 on 2024-10-17 21:27_

I guess we could call it a bug since the documentation differs. That seems fine, actually. Ok, lets use `UV_INDEX_INTERNAL_USERNAME` and `UV_INDEX_INTERNAL_PASSWORD`.

---

_@charliermarsh reviewed on 2024-10-17 21:27_

---

_Renamed from "Update indexes.md: ch UV_INDEX_ to UV_HTTP_BASIC_" to "Respect `UV_INDEX_` rather than `UV_HTTP_BASIC_`" by @charliermarsh on 2024-10-17 21:34_

---

_Label `bug` added by @charliermarsh on 2024-10-17 21:34_

---

_Review requested from @zanieb by @charliermarsh on 2024-10-17 21:35_

---

_@zanieb approved on 2024-10-17 21:40_

Thank you!

---

_Merged by @charliermarsh on 2024-10-17 21:42_

---

_Closed by @charliermarsh on 2024-10-17 21:42_

---
