```yaml
number: 615
title: Allow injecting certificate for tests
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: zb/offlinepi
head: konsti/test-certificate
created_at: 2023-12-12T10:50:03Z
updated_at: 2023-12-14T18:27:32Z
url: https://github.com/astral-sh/uv/pull/615
synced_at: 2026-01-10T15:44:44Z
```

# Allow injecting certificate for tests

---

_Pull request opened by @konstin on 2023-12-12 10:50_

Built on #609

When activating the `puffin-test-custom-ca-cert` feature, you can inject a custom ssl certificate by setting `PUFFIN_TEST_CA_CERT_PEM` to a pem file, e.g.

```bash
PUFFIN_TEST_CA_CERT_PEM=$(pwd)/mitmproxy-ca-cert.pem ./offlinepi record cargo test --features pypi --features puffin-test-custom-ca-cert -- --test-threads=1 
```

This feature is off by default, so this is not possible in release builds.

---

_Review requested from @zanieb by @konstin on 2023-12-12 10:50_

---

_Comment by @konstin on 2023-12-12 11:07_

```
PUFFIN_TEST_CA_CERT_PEM=$(pwd)/mitmproxy-ca-cert.pem ./offlinepi replay cargo test --features pypi --features puffin-test-custom-ca-cert --test resolver
```
Raises "SIGSEGV: invalid memory reference" :face_with_spiral_eyes: 

---

_Converted to draft by @konstin on 2023-12-12 11:07_

---

_Comment by @zanieb on 2023-12-12 17:00_

Some commentary at https://github.com/astral-sh/puffin/pull/609#discussion_r1423169953

I like this, but also think we need a user-facing way to use system certs.

---

_Comment by @konstin on 2023-12-12 17:53_

I think those are two separate issues; I also wouldn't recommend people to change their system trust store to run puffin unit tests.

---

_Marked ready for review by @konstin on 2023-12-12 17:53_

---

_Comment by @konstin on 2023-12-12 17:54_

I think this PR is fine, but the segfault is a problem in general

---

_Comment by @zanieb on 2023-12-12 18:57_

Yeah the segfault seems non-deterministic and more likely to occur with parallelism 

---

_Merged by @zanieb on 2023-12-14 18:27_

---

_Closed by @zanieb on 2023-12-14 18:27_

---

_Branch deleted on 2023-12-14 18:27_

---
