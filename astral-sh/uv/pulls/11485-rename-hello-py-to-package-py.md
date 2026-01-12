```yaml
number: 11485
title: "rename hello.py to <package>.py"
type: pull_request
state: closed
author: Gankra
labels: []
assignees: []
base: tracking/060
head: gankra/package_juice
created_at: 2025-02-13T16:51:36Z
updated_at: 2025-02-13T22:06:20Z
url: https://github.com/astral-sh/uv/pull/11485
synced_at: 2026-01-12T16:09:52Z
```

# rename hello.py to <package>.py

---

_@Gankra_

Alternative for #10369

Closes #7782 

---

_Added to milestone `v0.6.0` by @Gankra on 2025-02-13 16:51_

---

_@zanieb reviewed on 2025-02-13 16:59_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:812 on 2025-02-13 16:59_

I think we need the normalized name here so it's a valid module

---

_@Gankra reviewed on 2025-02-13 17:09_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/init.rs`:812 on 2025-02-13 17:09_

done with hopefully the right normalization

---

_Closed by @Gankra on 2025-02-13 22:06_

---
