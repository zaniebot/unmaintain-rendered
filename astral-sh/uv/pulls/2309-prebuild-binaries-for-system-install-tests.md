```yaml
number: 2309
title: "Prebuild binaries for `system-install` tests"
type: pull_request
state: closed
author: zanieb
labels:
  - testing
  - test-extra
assignees: []
draft: true
base: zb/system-install
head: zb/system-install-build
created_at: 2024-03-08T22:10:19Z
updated_at: 2024-03-11T23:28:14Z
url: https://github.com/astral-sh/uv/pull/2309
synced_at: 2026-01-10T14:49:08Z
```

# Prebuild binaries for `system-install` tests

---

_Pull request opened by @zanieb on 2024-03-08 22:10_

Rather than build the binaries in every test case, we build the necessary binaries up-front then perform the tests on downloaded artifacts.

---

_Label `test-extra` added by @zanieb on 2024-03-08 22:16_

---

_Label `testing` added by @zanieb on 2024-03-08 23:48_

---

_Label `testing` removed by @zanieb on 2024-03-08 23:52_

---

_Label `testing` added by @zanieb on 2024-03-08 23:53_

---

_Label `testing` removed by @zanieb on 2024-03-08 23:54_

---

_Label `testing` added by @zanieb on 2024-03-08 23:54_

---

_@konstin reviewed on 2024-03-11 17:08_

---

_Review comment by @konstin on `.github/workflows/system-install.yml`:61 on 2024-03-11 17:08_

Could we do a musl build? Than it's only one linux build and we can add alpine to testing

---

_@zanieb reviewed on 2024-03-11 18:36_

---

_Review comment by @zanieb on `.github/workflows/system-install.yml`:61 on 2024-03-11 18:36_

I'll look into it! I tried this briefly and failed but I probably was doing something wrong. I presume it doesn't, but just to confirm this should not degrade test coverage right?

---

_Review comment by @konstin on `.github/workflows/system-install.yml`:61 on 2024-03-11 20:03_

No, the libc of uv shouldn't matter at all, we check what libc the host uses. If you want i can make the musl build work, i've used musl a bunch

---

_@konstin reviewed on 2024-03-11 20:03_

---

_Comment by @zanieb on 2024-03-11 23:11_

Replacing with https://github.com/astral-sh/uv/pull/2312

---

_Closed by @zanieb on 2024-03-11 23:11_

---

_@zanieb reviewed on 2024-03-11 23:28_

---

_Review comment by @zanieb on `.github/workflows/system-install.yml`:61 on 2024-03-11 23:28_

Thanks! https://github.com/astral-sh/uv/pull/2370

---
