```yaml
number: 13339
title: Fix display of HTTP responses in trace logs for retry of errors
type: pull_request
state: merged
author: zanieb
labels:
  - tracing
assignees: []
merged: true
base: main
head: zb/wrapped
created_at: 2025-05-07T22:02:33Z
updated_at: 2025-05-08T14:23:26Z
url: https://github.com/astral-sh/uv/pull/13339
synced_at: 2026-01-12T16:10:39Z
```

# Fix display of HTTP responses in trace logs for retry of errors

---

_@zanieb_

Follows https://github.com/astral-sh/uv/pull/13228

Closes https://github.com/astral-sh/uv/issues/13338

I recall some discussion (maybe around https://github.com/astral-sh/uv/pull/4725) about how finding the source may not work properly? I can't find it though.

Now, I tested this, e.g.:

```
❯ cargo run -q -- pip install anyio -vv --index-url https://download.pytorch.org/whl/torch/ --no-cache --reinstall
...
TRACE Considering retry of response HTTP 403 Forbidden for https://download.pytorch.org/whl/torch/anyio/
```

I lament that I didn't think of that as a testing method in the first place :)

---

_Label `tracing` added by @zanieb on 2025-05-07 22:02_

---

_Renamed from "Use `WrappedReqwestError` instead of `reqwest::Error` for retry logs" to "Fix display of HTTP responses in trace logs for retry of errors" by @zanieb on 2025-05-07 22:10_

---

_Marked ready for review by @zanieb on 2025-05-07 22:10_

---

_Merged by @zanieb on 2025-05-08 14:23_

---

_Closed by @zanieb on 2025-05-08 14:23_

---

_Branch deleted on 2025-05-08 14:23_

---
