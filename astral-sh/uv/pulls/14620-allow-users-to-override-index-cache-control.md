```yaml
number: 14620
title: "Allow users to override index `cache-control` headers"
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
  - network
assignees: []
merged: true
base: main
head: charlie/cache-control-headers
created_at: 2025-07-14T23:54:34Z
updated_at: 2025-07-15T14:00:06Z
url: https://github.com/astral-sh/uv/pull/14620
synced_at: 2026-01-12T16:11:18Z
```

# Allow users to override index `cache-control` headers

---

_@charliermarsh_

## Summary

You can now override the cache control headers for the Simple API, file downloads, or both:

```toml
[[tool.uv.index]]
name = "example"
url = "https://example.com/simple"
cache-control = { api = "max-age=600", files = "max-age=365000000, immutable" }
```

Closes https://github.com/astral-sh/uv/issues/10444.


---

_Label `configuration` added by @charliermarsh on 2025-07-14 23:54_

---

_Label `network` added by @charliermarsh on 2025-07-14 23:54_

---

_Marked ready for review by @charliermarsh on 2025-07-15 00:03_

---

_@zanieb approved on 2025-07-15 13:38_

---

_Merged by @charliermarsh on 2025-07-15 14:00_

---

_Closed by @charliermarsh on 2025-07-15 14:00_

---

_Branch deleted on 2025-07-15 14:00_

---
