---
number: 8503
title: "Move `BaseClientBuilder` into `run`"
type: issue
state: open
author: konstin
labels:
  - internal
assignees: []
created_at: 2024-10-23T14:35:46Z
updated_at: 2025-01-24T21:16:39Z
url: https://github.com/astral-sh/uv/issues/8503
synced_at: 2026-01-07T13:12:17-06:00
---

# Move `BaseClientBuilder` into `run`

---

_Issue opened by @konstin on 2024-10-23 14:35_

We're building the same basic client builder in a lot of place:

```
let client_builder = BaseClientBuilder::new()
    .connectivity(connectivity)
    .native_tls(native_tls)
    .allow_insecure_host(allow_insecure_host.to_vec());
```

The arguments here are global arguments, so they come from the top level `run` scope and always have to be set. Instead of copying the same code over and over, we should build this client in the top level `run` function, pass it down to the command functions and stop passing down `connectivity`, `native_tls`, and `allow_insecure_host` to the command functions. We should also reuse this client builder in `RegistryClientBuilder`.

---

_Label `internal` added by @konstin on 2024-10-23 14:35_

---

_Comment by @issokuos on 2025-01-24 21:05_

I can have a look if this is something we still want to do

---

_Comment by @zanieb on 2025-01-24 21:16_

I think I was looking at this in https://github.com/astral-sh/uv/issues/2597 — I don't recall where it was left though.

---
