```yaml
number: 3420
title: Configuration files should be merged hierarchically
type: issue
state: closed
author: charliermarsh
labels:
  - configuration
  - preview
assignees: []
created_at: 2024-05-07T14:12:39Z
updated_at: 2024-05-08T18:49:44Z
url: https://github.com/astral-sh/uv/issues/3420
synced_at: 2026-01-10T05:31:37Z
```

# Configuration files should be merged hierarchically

---

_Issue opened by @charliermarsh on 2024-05-07 14:12_

Following Cargo's strategy here: https://doc.rust-lang.org/cargo/reference/config.html.

We should at least merge the local configuration with the user configuration (e.g., our `$HOME/.cargo/config.toml` equivalent). We can probably skip merging configuration all along the path.


---

_Label `configuration` added by @charliermarsh on 2024-05-07 14:12_

---

_Label `preview` added by @charliermarsh on 2024-05-07 14:12_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-08 16:17_

---

_Closed by @charliermarsh on 2024-05-08 18:49_

---
