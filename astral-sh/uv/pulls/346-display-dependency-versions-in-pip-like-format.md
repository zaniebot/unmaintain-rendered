```yaml
number: 346
title: Display dependency versions in pip-like format during solve failure
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/equal-disp
created_at: 2023-11-06T19:33:48Z
updated_at: 2023-11-06T19:53:16Z
url: https://github.com/astral-sh/uv/pull/346
synced_at: 2026-01-12T16:03:53Z
```

# Display dependency versions in pip-like format during solve failure

---

_@zanieb_

- Display `==` for exact version ranges
- Remove space between dependency and version range


---

_Renamed from "Display dependency versions in pi-like format during solve failure" to "Display dependency versions in pip-like format during solve failure" by @zanieb on 2023-11-06 19:35_

---

_@zanieb reviewed on 2023-11-06 19:36_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/snapshots/pip_compile__conflicting_transitive_url_dependency.snap`:20 on 2023-11-06 19:36_

These require the fixes in #342 

---

_Comment by @zanieb on 2023-11-06 19:41_

I'm not 100% sure this is better but I do find it clearer?

---

_@zanieb reviewed on 2023-11-06 19:41_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/snapshots/pip_compile__compile_unsolvable_requirements.snap`:19 on 2023-11-06 19:41_

This will not be displayed per #338 

---

_@charliermarsh reviewed on 2023-11-06 19:48_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/snapshots/pip_compile__conflicting_transitive_url_dependency.snap`:19 on 2023-11-06 19:48_

Should these not have spaces?

---

_@charliermarsh approved on 2023-11-06 19:48_

---

_@zanieb reviewed on 2023-11-06 19:52_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/snapshots/pip_compile__conflicting_transitive_url_dependency.snap`:19 on 2023-11-06 19:52_

#342 is needed to use `PuffinExternal` for those

---

_Merged by @zanieb on 2023-11-06 19:53_

---

_Closed by @zanieb on 2023-11-06 19:53_

---

_Branch deleted on 2023-11-06 19:53_

---
