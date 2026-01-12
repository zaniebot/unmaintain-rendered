```yaml
number: 2513
title: Download Python versions concurrently in bootstrapping script
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/install-conc
created_at: 2024-03-18T16:02:37Z
updated_at: 2024-03-20T00:27:52Z
url: https://github.com/astral-sh/uv/pull/2513
synced_at: 2026-01-12T16:05:05Z
```

# Download Python versions concurrently in bootstrapping script

---

_@zanieb_

_No description provided._

---

_Label `internal` added by @zanieb on 2024-03-18 16:02_

---

_Comment by @zanieb on 2024-03-18 16:19_

Doesn't really seem to provide a speed up on CI since it's network IO bound :(

---

_Comment by @zanieb on 2024-03-18 17:32_

It is quite a bit faster locally though, biasing towards merging.

---

_@samypr100 reviewed on 2024-03-19 04:05_

---

_Review comment by @samypr100 on `scripts/bootstrap/install.py`:194 on 2024-03-19 04:05_

```suggestion
    with concurrent.futures.ProcessPoolExecutor(max_workers=min(os.cpu_count(), len(versions))) as executor:
```

Might be worth to limit by what's available

---

_@konstin approved on 2024-03-19 09:30_

---

_@zanieb reviewed on 2024-03-20 00:27_

---

_Review comment by @zanieb on `scripts/bootstrap/install.py`:194 on 2024-03-20 00:27_

I don't think overallocating here has much of an effect, I'm just going to leave this as-is for now I hope to move this all to Rust in the near future.

---

_Merged by @zanieb on 2024-03-20 00:27_

---

_Closed by @zanieb on 2024-03-20 00:27_

---

_Branch deleted on 2024-03-20 00:27_

---
