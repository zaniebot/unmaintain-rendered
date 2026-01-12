```yaml
number: 8266
title: "Add support for reading and resolving `include-group` in dependency groups"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: tracking/735
head: zb/735-read-include
created_at: 2024-10-16T18:27:28Z
updated_at: 2024-10-17T00:11:07Z
url: https://github.com/astral-sh/uv/pull/8266
synced_at: 2026-01-12T16:08:14Z
```

# Add support for reading and resolving `include-group` in dependency groups

---

_@zanieb_

Part of #8090 

Adds the ability to read group inclusions (`include-group = <name>`) in the `pyproject.toml`. Resolves groups into concrete dependencies for resolution.

See https://github.com/astral-sh/uv/pull/8110 for a bit more commentary on deferred work.

---

_@zanieb reviewed on 2024-10-16 18:28_

---

_Review comment by @zanieb on `crates/uv-distribution/src/metadata/requires_dist.rs`:240 on 2024-10-16 18:28_

I don't love this implementation.

---

_@zanieb reviewed on 2024-10-16 18:29_

---

_Review comment by @zanieb on `crates/uv-distribution/src/metadata/requires_dist.rs`:258 on 2024-10-16 18:29_

It's unclear to me if we want to track the _original_ group of requirements throughout the resolver for error messages (it seems like it'd be nice, but may be a fair bit of work)

---

_Renamed from "Add support for reading `include-group` in dependency groups" to "Add support for reading and resolving `include-group` in dependency groups" by @zanieb on 2024-10-16 18:29_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject.rs`:123 on 2024-10-16 21:27_

I think it's enough for this to be included at the top-level, or is it not?

---

_@charliermarsh reviewed on 2024-10-16 21:27_

---

_@charliermarsh reviewed on 2024-10-16 21:29_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/requires_dist.rs`:334 on 2024-10-16 21:29_

I would say just raise here.

---

_@zanieb reviewed on 2024-10-16 21:29_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:123 on 2024-10-16 21:29_

This is derived from the implementation at https://github.com/PyO3/pyproject-toml-rs/pull/24 per https://github.com/astral-sh/uv/pull/8104#discussion_r1800654513 — I don't have any preference, personally.

---

_@zanieb reviewed on 2024-10-16 21:30_

---

_Review comment by @zanieb on `crates/uv-distribution/src/metadata/requires_dist.rs`:334 on 2024-10-16 21:30_

It defies the "SHOULD" recommendation in the PEP, but we will generally need to resolve all groups anyway for locking so I think that's reasonable for now.

---

_@charliermarsh reviewed on 2024-10-16 21:32_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject.rs`:126 on 2024-10-16 21:32_

Should this be required?

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject.rs`:128 on 2024-10-16 21:32_

Do we... want a catch-all variant? So we can catch other objects that are added in the future...? (We might want to error in that case anyway.)

---

_@charliermarsh reviewed on 2024-10-16 21:32_

---

_@charliermarsh reviewed on 2024-10-16 21:34_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject.rs`:126 on 2024-10-16 21:34_

If you make this `Option<GroupName>`, it will simplify some error handling. You can probably then also avoid some `GroupName` clones (I think `parents` can become `BTreeSet<&GroupName>`).

---

_@zanieb reviewed on 2024-10-16 21:45_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:126 on 2024-10-16 21:45_

I had the same thought. I think it's written this way so other fields can be added here in the future then they can be made mutually exclusive or not based on whether or not they're populated? We should probably fail with an empty table though? Or at least warn after deserialization?

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:128 on 2024-10-16 21:46_

I'm not sure. Sounds like a nice property sometimes? We don't do this elsewhere in the `pyproject.toml` do we?

---

_@zanieb reviewed on 2024-10-16 21:46_

---

_Comment by @charliermarsh on 2024-10-16 21:46_

As discussed gonna take this one over, will push to this branch.

---

_@charliermarsh reviewed on 2024-10-16 22:27_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:15549 on 2024-10-16 22:27_

@zanieb -- Good suggestion.

---

_@charliermarsh reviewed on 2024-10-16 22:28_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject.rs`:128 on 2024-10-16 22:28_

Ok the current structure is pretty strict... It rejects empty tables, extra fields, and missing `include_group`... It does match the reference implementation: https://peps.python.org/pep-0735/#reference-implementation. But I'd say what I have here is too strict though since we're doing eager validation. I think we should just warn if we see unsupported groups.

---

_@zanieb reviewed on 2024-10-16 22:35_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:128 on 2024-10-16 22:35_

Sounds good to me.

---

_@zanieb reviewed on 2024-10-16 22:35_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:15743 on 2024-10-16 22:35_

Perhaps we should say "in group `foo`"?

---

_@zanieb reviewed on 2024-10-16 22:36_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:1188 on 2024-10-16 22:36_

👍 nice

---

_@charliermarsh approved on 2024-10-16 23:36_

---

_Marked ready for review by @charliermarsh on 2024-10-16 23:36_

---

_Merged by @charliermarsh on 2024-10-17 00:11_

---

_Closed by @charliermarsh on 2024-10-17 00:11_

---

_Branch deleted on 2024-10-17 00:11_

---
