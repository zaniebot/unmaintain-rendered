```yaml
number: 2924
title: "Split configuration options out of `uv-types`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/uv-config
created_at: 2024-04-09T02:55:39Z
updated_at: 2024-04-09T16:35:54Z
url: https://github.com/astral-sh/uv/pull/2924
synced_at: 2026-01-10T14:43:31Z
```

# Split configuration options out of `uv-types`

---

_Pull request opened by @zanieb on 2024-04-09 02:55_

Needed to prevent circular dependencies in my toolchain work (#2931). I think this is probably a reasonable change as we move towards persistent configuration too?

Unfortunately `BuildIsolation` needs to be in `uv-types` to avoid circular dependencies still. We might be able to resolve that in the future.

---

_Label `internal` added by @zanieb on 2024-04-09 02:55_

---

_Comment by @zanieb on 2024-04-09 03:06_

Hm my problem is actually rooted in `uv-interpreter` being required by `uv-config` for `BuildIsolation::Shared` which is resolved by moving it over to `uv-types` (https://github.com/astral-sh/uv/pull/2924/commits/965ee75b5633cb55dbbb217da87f0cfa71521a39)

---

_Marked ready for review by @zanieb on 2024-04-09 14:24_

---

_Review requested from @charliermarsh by @zanieb on 2024-04-09 14:24_

---

_@charliermarsh reviewed on 2024-04-09 15:18_

---

_Review comment by @charliermarsh on `Cargo.toml`:47 on 2024-04-09 15:18_

I would prefer `uv-configuration` personally.

---

_@charliermarsh approved on 2024-04-09 15:18_

Some of the categorizations feel a bit off, e.g. should `requirements.rs` be in there?

---

_@zanieb reviewed on 2024-04-09 16:20_

---

_Review comment by @zanieb on `Cargo.toml`:47 on 2024-04-09 16:20_

I'm down.

---

_Comment by @zanieb on 2024-04-09 16:21_

I don't really feel strongly `RequestedRequirements` feels weird in either place really. I'm happy to have them in `uv-types` though.

---

_Comment by @charliermarsh on 2024-04-09 16:22_

Maybe leave it in types if you can. I might need to add some new types there related to requirements for hash-checking anyway.

---

_Merged by @zanieb on 2024-04-09 16:35_

---

_Closed by @zanieb on 2024-04-09 16:35_

---

_Branch deleted on 2024-04-09 16:35_

---
