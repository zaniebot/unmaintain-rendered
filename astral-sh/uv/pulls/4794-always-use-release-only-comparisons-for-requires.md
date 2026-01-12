```yaml
number: 4794
title: "Always use release-only comparisons for `requires-python`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/requires-python-upper-bound
created_at: 2024-07-03T23:50:44Z
updated_at: 2024-07-04T20:06:53Z
url: https://github.com/astral-sh/uv/pull/4794
synced_at: 2026-01-12T16:06:28Z
```

# Always use release-only comparisons for `requires-python`

---

_@charliermarsh_

## Summary

There are a few ideas at play here:

1. pip always strips versions to the release when evaluating against a `Requires-Python`, so we now do the same. That means, e.g., using `3.13.0b0` will be accepted by a project with `Requires-Python: >= 3.13`, which does _not_ adhere to PEP 440 semantics but is somewhat intuitive.
2. Because we know we'll only be evaluating against release-only versions, we can use different semantics in PubGrub that let us collapse ranges. For example, `python_version >= '3.10' or python_version < '3.10'` can be collapsed to the truthy marker.

Closes https://github.com/astral-sh/uv/issues/4714.
Closes https://github.com/astral-sh/uv/issues/4272.
Closes https://github.com/astral-sh/uv/issues/4719.


---

_Label `bug` added by @charliermarsh on 2024-07-03 23:50_

---

_Review requested from @konstin by @charliermarsh on 2024-07-03 23:57_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-07-03 23:57_

---

_Comment by @charliermarsh on 2024-07-04 00:02_

Gotta fix the snapshots, which is just about how we display those versions.

---

_@charliermarsh reviewed on 2024-07-04 00:03_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version.rs`:544 on 2024-07-04 00:03_

Is it worth trying to avoid a copy here?

---

_Review comment by @konstin on `crates/uv-resolver/src/pubgrub/specifier.rs`:152 on 2024-07-04 08:55_

Can we merge the two `impl PubGrubSpecifier` blocks?

---

_@konstin approved on 2024-07-04 08:58_

We should add this to the documentation, otherwise the `requires-python` semantics are indecipherable after reading our prerelease version handling section.

---

_Comment by @charliermarsh on 2024-07-04 12:23_

Sounds good. I'll add some more documentation.

---

_Merged by @charliermarsh on 2024-07-04 20:06_

---

_Closed by @charliermarsh on 2024-07-04 20:06_

---

_Branch deleted on 2024-07-04 20:06_

---
