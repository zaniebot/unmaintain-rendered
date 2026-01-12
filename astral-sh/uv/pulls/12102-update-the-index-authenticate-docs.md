```yaml
number: 12102
title: "Update the `index.authenticate` docs"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/authenticate-docs
created_at: 2025-03-10T18:00:25Z
updated_at: 2025-03-11T20:01:44Z
url: https://github.com/astral-sh/uv/pull/12102
synced_at: 2026-01-12T16:10:08Z
```

# Update the `index.authenticate` docs

---

_@zanieb_

Follow-up to #11896 

Reframes the documentation a bit.

Looking into why the `[index]` child fields aren't generate in the reference correctly too.

---

_Label `documentation` added by @zanieb on 2025-03-10 18:00_

---

_@zanieb reviewed on 2025-03-10 18:12_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:97 on 2025-03-10 18:12_

These should be generated from the enum, but it doesn't render at all right now.

---

_@zanieb reviewed on 2025-03-10 19:32_

---

_Review comment by @zanieb on `docs/configuration/indexes.md`:178 on 2025-03-10 19:32_

I'm a little hesitant about this framing, but I think it matches the other sections better and will be more discoverable.

We'll link to this from the index-specific documentation that requires this feature.

---

_Marked ready for review by @zanieb on 2025-03-11 16:36_

---

_Comment by @zanieb on 2025-03-11 16:36_

(I'll give this a self-review and try to release today)

---

_Review requested from @charliermarsh by @zanieb on 2025-03-11 17:12_

---

_Merged by @zanieb on 2025-03-11 20:01_

---

_Closed by @zanieb on 2025-03-11 20:01_

---

_Branch deleted on 2025-03-11 20:01_

---
