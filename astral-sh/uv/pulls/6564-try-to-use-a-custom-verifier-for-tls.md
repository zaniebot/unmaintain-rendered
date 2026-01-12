```yaml
number: 6564
title: Try to use a custom verifier for TLS
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/tls
created_at: 2024-08-24T02:36:48Z
updated_at: 2024-08-29T19:47:09Z
url: https://github.com/astral-sh/uv/pull/6564
synced_at: 2026-01-12T16:07:25Z
```

# Try to use a custom verifier for TLS

---

_@charliermarsh_

## Summary

This is one attempt to do what's described in https://github.com/astral-sh/uv/pull/4944#issuecomment-2219285090. I'm stuck though because there is no public API to add an `Identity` to a client builder.


---

_Converted to draft by @charliermarsh on 2024-08-24 02:36_

---

_Review comment by @charliermarsh on `crates/uv-client/src/base_client.rs`:240 on 2024-08-24 02:37_

All the of above has to be vendored and kept in-sync with `reqwest`.

---

_@charliermarsh reviewed on 2024-08-24 02:37_

---

_@charliermarsh reviewed on 2024-08-24 02:37_

---

_Review comment by @charliermarsh on `crates/uv-client/src/base_client.rs`:254 on 2024-08-24 02:37_

Basically stuck here, we need to read `SSL_CERT` and add it to `config_builder`, but the APIs that `reqwest` uses aren't public.

---

_Closed by @charliermarsh on 2024-08-29 19:47_

---
