```yaml
number: 13617
title: blocklist the linux cpython builds from 20250517
type: pull_request
state: merged
author: Gankra
labels: []
assignees: []
merged: true
base: main
head: gankra/revert-pbs
created_at: 2025-05-23T14:11:17Z
updated_at: 2025-05-23T22:43:00Z
url: https://github.com/astral-sh/uv/pull/13617
synced_at: 2026-01-10T11:10:42Z
```

# blocklist the linux cpython builds from 20250517

---

_Pull request opened by @Gankra on 2025-05-23 14:11_

There is a runtime issue with some of these builds

Here is the testing I've seen (❌ has bug, ✅ works):

* uv version (pbs version)
  * ❌ uv 0.7.7 (pbs 20250521)
  * ❌ uv 0.7.6 (pbs 20250517)
  * ✅ uv 0.7.5 (pbs 20250409)
* os
  * ❌ linux
  * ✅ macos
  * ✅ windows
* arch
  * ❌ x86_64
  * ✅ aarch64
* python version
  * ❌ 3.12
  * ❌ 3.13
  * ❌ 3.14

Fixes #13610

---

_Comment by @geofft on 2025-05-23 14:16_

Did we test that 20250517 is okay? Those builds are very similar (both statically link libpython). On the other hand, if we have to block 20250517 too and go to dynamically linking libpython, we should think about that harder.

(I'm on a train and intermittently on internet)

---

_Comment by @konstin on 2025-05-23 14:22_

Can you add the test case from #13610 to the test suite `cfg`'d to the linux x86_64?

---

_Renamed from "blocklist the linux cpython builds from 20250521" to "blocklist the linux cpython builds from 20250517" by @Gankra on 2025-05-23 14:30_

---

_@zanieb reviewed on 2025-05-23 15:10_

---

_Review comment by @zanieb on `crates/uv-python/fetch-download-metadata.py`:270 on 2025-05-23 15:10_

Doesn't this need to be `>=`?

---

_@zanieb reviewed on 2025-05-23 15:10_

---

_Review comment by @zanieb on `crates/uv-python/fetch-download-metadata.py`:270 on 2025-05-23 15:10_

(you also need to run the script)

---

_Comment by @Gankra on 2025-05-23 15:22_

> Can you add the test case from https://github.com/astral-sh/uv/issues/13610 to the test suite cfg'd to the linux x86_64?

Trying to do so in https://github.com/astral-sh/uv/pull/13619 but no luck so far.

---

_@Gankra reviewed on 2025-05-23 15:24_

---

_Review comment by @Gankra on `crates/uv-python/fetch-download-metadata.py`:270 on 2025-05-23 15:24_

Ah haha, good catch. I widened the range and reran which without `>=` effectively reverted the changes to the json 😓 

---

_@Gankra reviewed on 2025-05-23 16:27_

---

_Review comment by @Gankra on `crates/uv-python/download-metadata.json`:56 on 2025-05-23 16:27_

Unfortunate that entire versions of cpython go away...

---

_Comment by @Gankra on 2025-05-23 16:31_

<img width="727" alt="Screenshot 2025-05-23 at 12 31 10 PM" src="https://github.com/user-attachments/assets/b1050aae-347c-45f6-ae57-402db780ca60" />

oh hmm that's gonna be problematic for making the revert platform-specific

---

_@konstin reviewed on 2025-05-23 20:46_

---

_Review comment by @konstin on `crates/uv-python/download-metadata.json`:56 on 2025-05-23 20:46_



We should highlight this in the release, saying that these prereleases are temporarily removed due to an unrelated, non-alpha-related miscompilation.


---

_Merged by @Gankra on 2025-05-23 22:15_

---

_Closed by @Gankra on 2025-05-23 22:15_

---

_Branch deleted on 2025-05-23 22:15_

---
