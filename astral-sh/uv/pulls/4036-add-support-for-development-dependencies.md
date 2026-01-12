```yaml
number: 4036
title: Add support for development dependencies
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/dev
created_at: 2024-06-05T02:54:41Z
updated_at: 2024-06-06T01:40:18Z
url: https://github.com/astral-sh/uv/pull/4036
synced_at: 2026-01-12T16:06:00Z
```

# Add support for development dependencies

---

_@charliermarsh_

## Summary

Externally, development dependencies are currently structured as a flat list of PEP 580-compatible requirements:

```toml
[tool.uv]
dev-dependencies = ["werkzeug"]
```

When locking, we lock all development dependencies; when syncing, users can provide `--dev`.

Internally, though, we model them as dependency groups, similar to Poetry, PDM, and [PEP 735](https://peps.python.org/pep-0735). This enables us to change out the user-facing frontend without changing the internal implementation, once we've decided how these should be exposed to users.

A few important decisions encoded in the implementation (which we can change later):

1. Groups are enabled globally, for all dependencies. This differs from extras, which are enabled on a per-requirement basis. Note, however, that we'll only discover groups for uv-enabled packages anyway.
2. Installing a group requires installing the base package. We rely on this in PubGrub to ensure that we resolve to the same version (even though we only expect groups to come from workspace dependencies anyway, which are unique). But anyway, that's encoded in the resolver right now, just as it is for extras.


---

_Label `preview` added by @charliermarsh on 2024-06-05 02:54_

---

_Marked ready for review by @charliermarsh on 2024-06-05 03:19_

---

_Review requested from @zanieb by @charliermarsh on 2024-06-05 03:19_

---

_Review requested from @konstin by @charliermarsh on 2024-06-05 03:19_

---

_@charliermarsh reviewed on 2024-06-05 03:20_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata.rs`:39 on 2024-06-05 03:20_

I find the terminology here really confusing -- that we have both extras and dependency groups -- but, what can we do? I'm honestly considering using `dev_dependencies` everywhere internally instead of dependency groups.

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:401 on 2024-06-05 14:31_

Nit: `#[serde(rename_all = "kebab-case")]` on the struct

---

_Review comment by @konstin on `crates/uv-resolver/src/resolution/graph.rs`:39 on 2024-06-05 14:35_

Nit: Can you add a comment which one is which

---

_Review comment by @konstin on `crates/uv/tests/lock.rs`:1396 on 2024-06-05 14:41_

Nit: This is verbose

---

_@konstin approved on 2024-06-05 14:41_

---

_@charliermarsh reviewed on 2024-06-05 15:08_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:1396 on 2024-06-05 15:08_

What is verbose? Like I should use a different test case?

---

_@konstin reviewed on 2024-06-05 15:39_

---

_Review comment by @konstin on `crates/uv/tests/lock.rs`:1396 on 2024-06-05 15:39_

If you have a test on hand with less wheels generating a shorter inline snapshot, it would make the file easier to follow, but it's not blocking or anything

---

_@charliermarsh reviewed on 2024-06-05 15:59_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:1396 on 2024-06-05 15:59_

Yes totally. I just couldn't tell if you were commenting on the test itself or the lockfile format.

---

_@charliermarsh reviewed on 2024-06-05 16:30_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution/graph.rs`:39 on 2024-06-05 16:30_

I decided to introduce a separate type.

---

_Comment by @charliermarsh on 2024-06-06 01:20_

I decided to rework the terminology (internally) to center on development dependencies (even though it still supports groups internally), since that's the only thing they're actually used for right now.

---

_Merged by @charliermarsh on 2024-06-06 01:40_

---

_Closed by @charliermarsh on 2024-06-06 01:40_

---

_Branch deleted on 2024-06-06 01:40_

---
