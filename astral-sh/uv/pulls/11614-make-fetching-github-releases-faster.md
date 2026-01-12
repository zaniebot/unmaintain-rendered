```yaml
number: 11614
title: Make fetching github releases faster
type: pull_request
state: merged
author: j178
labels:
  - internal
assignees: []
merged: true
base: main
head: py-fetch
created_at: 2025-02-19T03:50:17Z
updated_at: 2025-02-20T14:37:43Z
url: https://github.com/astral-sh/uv/pull/11614
synced_at: 2026-01-12T16:09:55Z
```

# Make fetching github releases faster

---

_@j178_

I noticed that the latest two `sync-python-releases` jobs failed due to `httpx.RemoteProtocolError: peer closed connection without sending complete message body (incomplete chunked read)`.

For the current python-build-standalone release, each request page (defaulting to 30 items per page) takes about 20 seconds and loads around 32MB of data. This extensive data load might be causing the request to frequently fail.

In this PR, I reduced number of items per page to 10 and added `Accept-Encoding: gzip, deflate` to the request header. Now, it takes about 6 seconds to load, and the compressed response size has been reduced to 534KB. I hope this would addresses the request failure.

---

_Renamed from "Reduce fetching github releases faster" to "Make fetching github releases faster" by @j178 on 2025-02-19 03:50_

---

_Marked ready for review by @j178 on 2025-02-19 04:04_

---

_Review requested from @zanieb by @konstin on 2025-02-19 08:46_

---

_Comment by @zanieb on 2025-02-19 14:57_

I worry a little that more requests means we're more likely to encounter the rate limit, but 🤷‍♀️ 

Thanks!

---

_Merged by @zanieb on 2025-02-19 14:57_

---

_Closed by @zanieb on 2025-02-19 14:57_

---

_Label `internal` added by @zanieb on 2025-02-19 14:58_

---

_Branch deleted on 2025-02-20 14:37_

---
