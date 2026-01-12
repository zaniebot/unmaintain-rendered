```yaml
number: 13050
title: Implement RFC 7231 compliant relative URI and fragment handling in redirects
type: pull_request
state: merged
author: jtfmumm
labels:
  - bug
  - error messages
assignees: []
merged: true
base: main
head: jtfm/restore-and-fix-redirect-handling
created_at: 2025-04-22T10:57:14Z
updated_at: 2025-04-28T07:07:08Z
url: https://github.com/astral-sh/uv/pull/13050
synced_at: 2026-01-12T16:10:32Z
```

# Implement RFC 7231 compliant relative URI and fragment handling in redirects

---

_@jtfmumm_

This PR restores #13041 and integrates two PRs from @zanieb:
* #13038
* #13040

It also adds tests for relative URI and fragment handling.

Closes #13037.


---

_Comment by @jtfmumm on 2025-04-22 11:00_

@zanieb We could either close your other PRs in favor of this one or you could cherry-pick the test commit.

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:577 on 2025-04-22 11:19_

Can we copy what reqwest is doing here? That should be the most robust implementation Rust has. https://github.com/seanmonstar/reqwest/blob/36fd0381ae1aa25e89475a90834cbd5ca0ca6404/src/async_impl/client.rs#L2920-L2943

---

_@konstin approved on 2025-04-22 11:23_

---

_@konstin reviewed on 2025-04-22 11:24_

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:577 on 2025-04-22 11:24_

(kinda annoying that this modifies the request, otherwise we could pull it into a public reqwest method)

---

_@konstin approved on 2025-04-22 11:27_

---

_@konstin reviewed on 2025-04-22 12:24_

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:1028 on 2025-04-22 12:24_

We need to mask those, or ideally capture only `RequestBuilder`

---

_@jtfmumm reviewed on 2025-04-22 12:54_

---

_Review comment by @jtfmumm on `crates/uv-publish/src/lib.rs`:1028 on 2025-04-22 12:54_

I've updated to capture just the `RequestBuilder`

---

_Label `bug` added by @jtfmumm on 2025-04-22 13:45_

---

_Label `error messages` added by @jtfmumm on 2025-04-22 13:45_

---

_@jtfmumm reviewed on 2025-04-22 15:43_

---

_Review comment by @jtfmumm on `crates/uv-client/src/base_client.rs`:577 on 2025-04-22 15:43_

I added their check that the URL can be parsed as a valid HTTP URI. Otherwise, I think we're covering the same ground but in a way that allows us to provide more detailed error messages.

---

_@zanieb approved on 2025-04-25 23:55_

---

_Merged by @jtfmumm on 2025-04-28 07:07_

---

_Closed by @jtfmumm on 2025-04-28 07:07_

---

_Branch deleted on 2025-04-28 07:07_

---
