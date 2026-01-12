```yaml
number: 2645
title: "Avoid `prepare_metadata_for_build_wheel` calls for Hatch packages with dynamic dependencies"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: charlie/dynamic
created_at: 2024-03-25T03:57:01Z
updated_at: 2024-03-26T00:01:49Z
url: https://github.com/astral-sh/uv/pull/2645
synced_at: 2026-01-12T16:05:09Z
```

# Avoid `prepare_metadata_for_build_wheel` calls for Hatch packages with dynamic dependencies

---

_@charliermarsh_

## Summary

Hatch allows for highly dynamic customization of metadata via hooks. In such cases, Hatch
can't upload the PEP 517 contract, in that the metadata Hatch would return by
`prepare_metadata_for_build_wheel` isn't guaranteed to match that of the built wheel.

Hatch disables `prepare_metadata_for_build_wheel` entirely for pip. We'll instead disable
it on our end when metadata is defined as "dynamic" in the pyproject.toml, which should
allow us to leverage the hook in _most_ cases while still avoiding incorrect metadata for
the remaining cases.

Closes: https://github.com/astral-sh/uv/issues/2130.


---

_Comment by @charliermarsh on 2024-03-25 03:57_

Just the failing test to start.

---

_@ofek reviewed on 2024-03-25 03:58_

---

_Review comment by @ofek on `scripts/packages/hatch_dynamic/requirements-cli.txt`:1 on 2024-03-25 03:58_

This file can be deleted now as well as the other requirements file

---

_Marked ready for review by @charliermarsh on 2024-03-25 20:41_

---

_Review requested from @zanieb by @charliermarsh on 2024-03-25 20:42_

---

_Review requested from @konstin by @charliermarsh on 2024-03-25 20:42_

---

_@zanieb reviewed on 2024-03-25 22:07_

---

_Review comment by @zanieb on `crates/uv-build/src/lib.rs`:620 on 2024-03-25 22:07_

```suggestion
        // can't uphold the PEP 517 contract, in that the metadata Hatch would return by
```

---

_@zanieb approved on 2024-03-25 22:08_

---

_Label `bug` added by @zanieb on 2024-03-25 22:08_

---

_Label `compatibility` added by @zanieb on 2024-03-25 22:08_

---

_@charliermarsh reviewed on 2024-03-25 22:13_

---

_Review comment by @charliermarsh on `crates/uv-build/src/lib.rs`:620 on 2024-03-25 22:13_

Upload the contract

---

_Renamed from "Avoid `prepare_metadata_for_build_wheel` calls for dynamic dependencies" to "Avoid `prepare_metadata_for_build_wheel` calls for Hatch packages with dynamic dependencies" by @charliermarsh on 2024-03-25 22:23_

---

_Merged by @charliermarsh on 2024-03-25 22:26_

---

_Closed by @charliermarsh on 2024-03-25 22:26_

---

_Branch deleted on 2024-03-25 22:26_

---

_Comment by @potiuk on 2024-03-26 00:01_

Installed `uv` from main and I confirm it fixes the issue. Both - local installation from "file://" and remote via URL work. 

---
