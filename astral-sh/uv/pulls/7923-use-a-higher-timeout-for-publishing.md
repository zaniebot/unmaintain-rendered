```yaml
number: 7923
title: Use a higher timeout for publishing
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/higher-timeout-for-publish
created_at: 2024-10-04T11:17:14Z
updated_at: 2024-10-04T13:52:24Z
url: https://github.com/astral-sh/uv/pull/7923
synced_at: 2026-01-12T16:08:04Z
```

# Use a higher timeout for publishing

---

_@konstin_

A 30s timeout is too low when uploading a large package over a home internet connection, where uploads can be 10x slower than downloads. We increase the default limit to 15min, while keeping the environment variables to override this intact.

In the process, I've lifted the conversion of the timeout seconds to `Duration` up. `Duration::from_mins` is nightly only so we don't quite get the benefit yet.

Fixes #7809

---

_Label `bug` added by @konstin on 2024-10-04 11:17_

---

_Review requested from @zanieb by @konstin on 2024-10-04 11:17_

---

_Comment by @charliermarsh on 2024-10-04 11:20_

IIUC, this value shouldn't be the end-to-end timeout duration though, it's the time between client-server interactions.

---

_Comment by @konstin on 2024-10-04 11:28_

Speculating, but could it be the timeout goes wrong here because we're doing an upload and get the first server read only after the whole upload is done?

---

_Comment by @zanieb on 2024-10-04 13:12_

It'd be nice to know what's going on instead of just bumping the read timeout to 15 minutes

---

_Comment by @konstin on 2024-10-04 13:43_

It looks like the reqwest bug https://github.com/seanmonstar/reqwest/issues/2403, since we would need to set a write timeout and reqwest doesn't support that.

---

_@zanieb approved on 2024-10-04 13:47_

Let's open an issue to track a better solution, probably makes sense to get this to users immediately though.



---

_Comment by @konstin on 2024-10-04 13:52_

Done: #7925

---

_Merged by @konstin on 2024-10-04 13:52_

---

_Closed by @konstin on 2024-10-04 13:52_

---

_Branch deleted on 2024-10-04 13:52_

---
