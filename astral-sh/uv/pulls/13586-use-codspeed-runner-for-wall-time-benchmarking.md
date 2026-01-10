```yaml
number: 13586
title: Use codspeed runner for wall time benchmarking
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/macro
created_at: 2025-05-21T21:54:30Z
updated_at: 2025-05-30T08:08:42Z
url: https://github.com/astral-sh/uv/pull/13586
synced_at: 2026-01-10T11:10:41Z
```

# Use codspeed runner for wall time benchmarking

---

_Pull request opened by @zanieb on 2025-05-21 21:54_

We couldn't use the CodSpeed "walltime" runner because it required administrative permissions on our repositories, but following some feedback they've adjusted the required permissions so we can give it a try now.

As a brief background, CodSpeed uses Valgrind for instrumented benchmarking, emulating the execution for improved stability on GitHub's runners. This is nice, but means things like allocs and io are not measured. Now, they support standard wall time benchmarking, using their own managed runners for stable measurements. Here, we add support for those while retaining the old workflow — you can toggle between views in their UI.

---

_Label `internal` added by @zanieb on 2025-05-21 21:54_

---

_Renamed from "Use codspeed runner for benchmarking" to "Use codspeed runner for wall time benchmarking" by @zanieb on 2025-05-22 19:06_

---

_Comment by @zanieb on 2025-05-22 19:07_

I think the only problem here is you only get 120 free minutes (monthly?). We'll use way more than that. 

---

_Marked ready for review by @zanieb on 2025-05-27 14:41_

---

_Assigned to @konstin by @zanieb on 2025-05-28 15:12_

---

_Comment by @zanieb on 2025-05-28 15:12_

Curious for your thoughts @konstin 

---

_Review requested from @BurntSushi by @zanieb on 2025-05-29 13:00_

---

_Review comment by @BurntSushi on `.github/workflows/ci.yml`:2250 on 2025-05-29 13:07_

Can we do `x86-64` instead? Or add `x86-64`? I feel like `x86-64` would be ideal here for Linux at least.

---

_@BurntSushi approved on 2025-05-29 13:08_

---

_@zanieb reviewed on 2025-05-29 13:16_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:2250 on 2025-05-29 13:16_

We cannot, unfortunately. That's the machine they're using because it's way cheaper. They might add x86-64 in the future.

---

_@BurntSushi reviewed on 2025-05-29 13:26_

---

_Review comment by @BurntSushi on `.github/workflows/ci.yml`:2250 on 2025-05-29 13:26_

Bummer. This will still help for I/O and allocs though. (I have this vague sense that because x86-64 is CISC, that instruction count measurements are less good than they are for something like aarch64, which is RISC.)

---

_Merged by @zanieb on 2025-05-29 17:52_

---

_Closed by @zanieb on 2025-05-29 17:52_

---

_Branch deleted on 2025-05-29 17:52_

---

_Comment by @konstin on 2025-05-30 08:08_

I'd love to try this out, something with more stable IO could make our benchmarks more sensitive and easier to interpret.

---
