```yaml
number: 773
title: Generate environment variable reference from code
type: issue
state: closed
author: zanieb
labels:
  - documentation
assignees: []
created_at: 2025-07-07T17:27:32Z
updated_at: 2025-07-08T15:34:52Z
url: https://github.com/astral-sh/ty/issues/773
synced_at: 2026-01-10T02:07:36Z
```

# Generate environment variable reference from code

---

_Issue opened by @zanieb on 2025-07-07 17:27_

e.g., this would have automated https://github.com/astral-sh/ty/pull/768

This is how we document environment variables in uv

https://github.com/astral-sh/uv/blob/efc361223c134840b7650fed1ef9ebd9577b5454/crates/uv-static/src/env_vars.rs#L6-L7

We require using constants from that file rather than accessing environment variables via strings.

---

_Label `documentation` added by @zanieb on 2025-07-07 17:27_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-08 13:00_

---

_Closed by @charliermarsh on 2025-07-08 15:34_

---
