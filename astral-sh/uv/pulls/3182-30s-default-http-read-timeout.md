```yaml
number: 3182
title: 30s default http read timeout
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/10s-http-timeout
created_at: 2024-04-22T12:18:23Z
updated_at: 2024-04-23T18:12:51Z
url: https://github.com/astral-sh/uv/pull/3182
synced_at: 2026-01-12T16:05:29Z
```

# 30s default http read timeout

---

_@konstin_

Since we're now using read timeouts and not total timeouts, we can use a lower threshold, a single read shouldn't take 5 min (and not even 10s).

The 10s value is somewhat arbitrary.

Like #3144, this is a breaking change in some sense.

---

_Label `enhancement` added by @konstin on 2024-04-22 12:18_

---

_Review requested from @charliermarsh by @konstin on 2024-04-22 12:18_

---

_Review requested from @zanieb by @konstin on 2024-04-22 12:18_

---

_Comment by @zanieb on 2024-04-22 14:29_

I think we should probably use a larger value given all the problems we've seen with timeouts?

---

_Comment by @konstin on 2024-04-22 16:24_

I can't speak for #2833, but for the top level #1912 report, a shorter timeout would be better because the stuck jobs fails earlier.

---

_Comment by @zanieb on 2024-04-22 16:30_

I guess I'd still prefer something like 30s, agree that 5 minutes is execessive.

---

_Renamed from "10s default http read timeout" to "30s default http read timeout" by @konstin on 2024-04-22 21:47_

---

_@zanieb approved on 2024-04-22 22:06_

ty

---

_@zanieb reviewed on 2024-04-22 22:06_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:115 on 2024-04-22 22:06_

Is this documented anywhere?

---

_@konstin reviewed on 2024-04-22 22:15_

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:115 on 2024-04-22 22:15_

Looks like the 5 min were previously undocumented.

It's also a bit misleading since it's not an http timeout properly, it's just a read timeout.

---

_@zanieb reviewed on 2024-04-22 22:17_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:115 on 2024-04-22 22:17_

👍 I don't think we'll ever want a real timeout per http request, I think a read timeout makes sense. If we expose a comprehensive timeout, I think it'd expect it to be like for an entire command invocation or installation of a package or something.

---

_Merged by @charliermarsh on 2024-04-22 23:05_

---

_Closed by @charliermarsh on 2024-04-22 23:05_

---

_Branch deleted on 2024-04-22 23:05_

---

_@samypr100 reviewed on 2024-04-23 15:57_

---

_Review comment by @samypr100 on `crates/uv-client/src/base_client.rs`:115 on 2024-04-23 15:57_

For what it's worth,

Poetry and Pip use 15 seconds as a default
* https://python-poetry.org/docs/faq/#my-requests-are-timing-out
* https://github.com/pypa/pip/blob/main/src/pip/_internal/cli/cmdoptions.py#L294

I'll still take 30 seconds over 300 😄 

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:115 on 2024-04-23 18:12_

I'd really like to have some data on this what real world values are

---

_@konstin reviewed on 2024-04-23 18:12_

---
