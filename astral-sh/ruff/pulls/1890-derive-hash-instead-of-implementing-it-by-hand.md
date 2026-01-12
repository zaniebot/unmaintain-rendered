```yaml
number: 1890
title: Derive Hash instead of implementing it by hand
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: derive-hash
created_at: 2023-01-15T08:35:19Z
updated_at: 2023-01-16T06:42:55Z
url: https://github.com/astral-sh/ruff/pull/1890
synced_at: 2026-01-12T05:36:32Z
```

# Derive Hash instead of implementing it by hand

---

_Pull request opened by @not-my-profile on 2023-01-15 08:35_

_No description provided._

---

_Converted to draft by @not-my-profile on 2023-01-15 08:40_

---

_Comment by @not-my-profile on 2023-01-15 10:13_

Marked this as a draft since #1891 should be merged first.

---

_@charliermarsh reviewed on 2023-01-15 18:58_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:48 on 2023-01-15 18:58_

These are all intentionally using `FxHashSet`. Last I checked it actually made a measurable difference in the benchmarks.

---

_@charliermarsh reviewed on 2023-01-15 19:17_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:48 on 2023-01-15 19:17_

To be clear: I don't want to change this right now if it has any noticeable impact on perf. We read from these _extremely_ often (but only hash the configuration once).

However, in theory, this could be even better, if that's interesting to you: https://github.com/charliermarsh/ruff/issues/703

---

_@not-my-profile reviewed on 2023-01-15 19:48_

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:48 on 2023-01-15 19:48_

Right, I agree that we shouldn't change this if it makes a measurable difference ... I'll do a benchmark.

If it makes a difference I'll just amend this PR to introduce `HashableHashSet` and `HashableHashMap` wrapper types instead.

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:48 on 2023-01-15 20:12_

Ok yes it does make a difference ... good catch! I'll change it to use newtypes instead after #1891 has been merged.

---

_@not-my-profile reviewed on 2023-01-15 20:12_

---

_@charliermarsh reviewed on 2023-01-15 20:21_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:48 on 2023-01-15 20:21_

I'm actually shocked at how big of an impact it has.

Before:

![Screen Shot 2023-01-15 at 3 20 20 PM](https://user-images.githubusercontent.com/1309177/212564931-b0c2ac73-b0e0-4c45-b659-8e3695ac0fba.png)

After:

![Screen Shot 2023-01-15 at 3 20 08 PM](https://user-images.githubusercontent.com/1309177/212564933-b2ee329b-1a0b-4d66-a299-a7b01c156be2.png)


---

_@charliermarsh reviewed on 2023-01-15 20:22_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:48 on 2023-01-15 20:22_

Maybe it shouldn't be a surprise given that we're doing these `enabled` lookups _constantly_.

---

_@charliermarsh reviewed on 2023-01-15 20:32_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:48 on 2023-01-15 20:32_

I'm going to revisit using a bitset for this... some early benchmarks suggest it could provide another 10% speedup.

---

_@not-my-profile reviewed on 2023-01-15 20:59_

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:48 on 2023-01-15 20:59_

I am actually doing that just now ... PR incoming.

---

_@charliermarsh reviewed on 2023-01-15 21:12_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:48 on 2023-01-15 21:12_

Awesome.

---

_Marked ready for review by @not-my-profile on 2023-01-16 04:43_

---

_Review requested from @charliermarsh by @not-my-profile on 2023-01-16 04:44_

---

_Merged by @charliermarsh on 2023-01-16 06:42_

---

_Closed by @charliermarsh on 2023-01-16 06:42_

---
