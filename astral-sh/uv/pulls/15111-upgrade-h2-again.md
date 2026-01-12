```yaml
number: 15111
title: Upgrade h2 again
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/h2-up
created_at: 2025-08-06T16:57:08Z
updated_at: 2025-08-07T13:53:13Z
url: https://github.com/astral-sh/uv/pull/15111
synced_at: 2026-01-12T16:11:35Z
```

# Upgrade h2 again

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/15056

Following https://github.com/hyperium/h2/pull/858 I'm hoping the defaults there are more robust and no longer cause the problems reported in the above issue.

As @konstin noted at https://github.com/hyperium/h2/issues/856#issuecomment-3160720671, we may want to tweak the h2 settings during prefetches if we encounter this problem again.

---

_Review comment by @zanieb on `Cargo.lock`:1517 on 2025-08-06 16:58_

Not sure why this is happening

---

_@zanieb reviewed on 2025-08-06 16:58_

---

_@zanieb reviewed on 2025-08-06 18:12_

---

_Review comment by @zanieb on `Cargo.lock`:1517 on 2025-08-06 18:12_

Ah, we had a bound in our Cargo.toml

---

_@jtfmumm approved on 2025-08-06 18:26_

---

_Comment by @charliermarsh on 2025-08-06 18:44_

Can you link whatever changed in h2?

---

_Comment by @zanieb on 2025-08-06 21:01_

Yes! Updated

---

_Merged by @zanieb on 2025-08-07 13:53_

---

_Closed by @zanieb on 2025-08-07 13:53_

---

_Branch deleted on 2025-08-07 13:53_

---
