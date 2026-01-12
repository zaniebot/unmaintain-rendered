```yaml
number: 8440
title: Respect allow insecure host in publish
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - network
assignees: []
merged: true
base: main
head: konsti/insecure-host-in-publish
created_at: 2024-10-22T09:27:20Z
updated_at: 2024-10-22T12:07:34Z
url: https://github.com/astral-sh/uv/pull/8440
synced_at: 2026-01-12T16:08:19Z
```

# Respect allow insecure host in publish

---

_@konstin_

Switch to respecting the allow insecure host setting in all places where we use the client.

Fixes #8423
Part of #6985
Doesn't touch #6983


---

_Label `bug` added by @konstin on 2024-10-22 09:27_

---

_Review requested from @zanieb by @konstin on 2024-10-22 09:27_

---

_Label `network` added by @konstin on 2024-10-22 10:48_

---

_@zanieb approved on 2024-10-22 11:12_

---

_Merged by @konstin on 2024-10-22 11:36_

---

_Closed by @konstin on 2024-10-22 11:36_

---

_Branch deleted on 2024-10-22 11:36_

---

_@zanieb reviewed on 2024-10-22 11:38_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:361 on 2024-10-22 11:38_

Is this going to break a downstream consumer?

---

_@konstin reviewed on 2024-10-22 12:05_

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:361 on 2024-10-22 12:05_

Who would be affected? I can't find anything

```
$ git log -S "fn raw_client"
commit e7ae0f50d218aede8d7283d8cf414e8f7782fbba (origin/main, origin/HEAD, main)
Author: konsti <konstin@mailbox.org>
Date:   Tue Oct 22 13:36:18 2024 +0200

    Respect allow insecure host in publish (#8440)

commit 205bf8cabef5c931203df15bca5e524dbfaac08a
Author: konsti <konstin@mailbox.org>
Date:   Tue Sep 24 18:07:20 2024 +0200

    Implement trusted publishing (#7548)
    
    Co-authored-by: Charlie Marsh <charlie.r.marsh@gmail.com>
```

---

_@zanieb reviewed on 2024-10-22 12:07_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:361 on 2024-10-22 12:07_

Oh you just added it? Okee

---
