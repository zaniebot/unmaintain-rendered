```yaml
number: 2496
title: "Drop `macosx_10_0` from compatible wheel tags on `aarch64`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/macos-platform-tags-fix
created_at: 2024-03-17T16:12:24Z
updated_at: 2024-03-18T14:52:55Z
url: https://github.com/astral-sh/uv/pull/2496
synced_at: 2026-01-10T14:49:08Z
```

# Drop `macosx_10_0` from compatible wheel tags on `aarch64`

---

_Pull request opened by @zanieb on 2024-03-17 16:12_

Following #2489 this is the last remaining difference from Python 3.12's packaging module.

---

_Label `bug` added by @zanieb on 2024-03-17 16:12_

---

_@charliermarsh reviewed on 2024-03-17 16:16_

---

_Review comment by @charliermarsh on `crates/platform-tags/src/tags.rs`:447 on 2024-03-17 16:16_

I wonder why this was 10 to start…?

---

_@charliermarsh approved on 2024-03-17 16:16_

---

_@zanieb reviewed on 2024-03-17 16:18_

---

_Review comment by @zanieb on `crates/platform-tags/src/tags.rs`:447 on 2024-03-17 16:18_

We do include `10_4` through `10_16` so it seems easy to expect `10_0` should be included?

---

_@zanieb reviewed on 2024-03-17 16:30_

---

_Review comment by @zanieb on `crates/platform-tags/src/tags.rs`:447 on 2024-03-17 16:30_

Ah this is handled in https://github.com/astral-sh/uv/blob/9e71dd11e37fcc98b27339dfa96331a497892ca0/crates/platform-tags/src/tags.rs#L515-L543 for the x86_64 variant and it's just broken on aarch64

---

_@zanieb reviewed on 2024-03-17 16:38_

---

_Review comment by @zanieb on `crates/platform-tags/src/tags.rs`:447 on 2024-03-17 16:38_

See https://github.com/astral-sh/uv/pull/2496/commits/0038f18b77a133473ab228d10c1205a44afbac16 and https://github.com/astral-sh/uv/pull/2496/commits/73b561b19bb62987b193bb5ac7b22a893e495ed4

---

_Renamed from "Drop `macosx_10_0` from compatible wheel tags" to "Drop `macosx_10_0` from compatible wheel tags on `aarch64`" by @zanieb on 2024-03-17 16:39_

---

_@zanieb reviewed on 2024-03-17 16:45_

---

_Review comment by @zanieb on `crates/platform-tags/src/tags.rs`:761 on 2024-03-17 16:45_

I added some new coverage for x86_64 because it has a separate branch of logic but these were correct without changes.

---

_Review requested from @konstin by @zanieb on 2024-03-18 14:44_

---

_@konstin approved on 2024-03-18 14:45_

Code looks good but i can't review on mac implementation details

---

_Merged by @zanieb on 2024-03-18 14:52_

---

_Closed by @zanieb on 2024-03-18 14:52_

---

_Branch deleted on 2024-03-18 14:52_

---
