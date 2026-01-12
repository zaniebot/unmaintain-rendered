```yaml
number: 8108
title: "Add `--group` support to `uv add` and `uv remove`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: tracking/735
head: zb/735-add
created_at: 2024-10-10T20:32:51Z
updated_at: 2024-10-16T21:38:13Z
url: https://github.com/astral-sh/uv/pull/8108
synced_at: 2026-01-12T16:08:09Z
```

# Add `--group` support to `uv add` and `uv remove`

---

_@zanieb_

Part of #8090 

Adds the ability to add and remove dependencies from arbitrary groups using `uv add` and `uv remove`. Does not include resolving with the new dependencies — tackling that in #8110.

Additionally, this does not yet resolve interactions with the existing `dev` group — we'll tackle that separately as well. I probably won't merge the stack until that design is resolved.

---

_Comment by @zanieb on 2024-10-10 21:19_

Will remove from draft when I add test cases.

---

_Marked ready for review by @zanieb on 2024-10-15 00:28_

---

_@zanieb reviewed on 2024-10-15 14:44_

---

_Review comment by @zanieb on `crates/uv-distribution/src/metadata/requires_dist.rs`:152 on 2024-10-15 14:44_

This iterator is pretty unwieldy. Is there a good way to clean this up?

---

_@charliermarsh approved on 2024-10-16 21:25_

---

_@charliermarsh reviewed on 2024-10-16 21:28_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/requires_dist.rs`:220 on 2024-10-16 21:28_

I was somewhat surprised that this is only used once. I guess the other "dependency types" (like production) don't match this exact pattern?

---

_@charliermarsh reviewed on 2024-10-16 21:29_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/requires_dist.rs`:167 on 2024-10-16 21:29_

Does this correctly merge keys? Like if dev is included twice, does this concatenate the values? I sort of think it might replace them...

---

_@zanieb reviewed on 2024-10-16 21:31_

---

_Review comment by @zanieb on `crates/uv-distribution/src/metadata/requires_dist.rs`:220 on 2024-10-16 21:31_

Yeah I actually moved it out because I thought I would need to apply it twice, like.. to the `dev-dependencies` group but I ended up structuring things differently. I think we can move it back in, or make it an impl method somewhere.

---

_@zanieb reviewed on 2024-10-16 21:32_

---

_Review comment by @zanieb on `crates/uv-distribution/src/metadata/requires_dist.rs`:167 on 2024-10-16 21:32_

I think it probably replaces them, yeah. I wasn't a goal to resolve that here (per the description) since this implemented before I wrote a proposal. I figured I'd write some test cases and address it separately for review purposes.

---

_Merged by @zanieb on 2024-10-16 21:38_

---

_Closed by @zanieb on 2024-10-16 21:38_

---

_Branch deleted on 2024-10-16 21:38_

---
