```yaml
number: 342
title: " Improve error message for dependencies with no versions available"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/fix-no-version
created_at: 2023-11-06T18:15:06Z
updated_at: 2023-11-06T20:04:31Z
url: https://github.com/astral-sh/uv/pull/342
synced_at: 2026-01-10T15:50:28Z
```

#  Improve error message for dependencies with no versions available

---

_Pull request opened by @zanieb on 2023-11-06 18:15_

Partially addresses https://github.com/astral-sh/puffin/issues/310
Addresses case at https://github.com/astral-sh/puffin/issues/309#issuecomment-1793541558
Follow-up to #300 ensuring `PuffinExternal` is used consistently when formatting messages

Example at https://github.com/astral-sh/puffin/pull/342/files#diff-5c74a74ef34ef1d6e7453de8d2d19134813156e8b6a657e6b5ed71fda5a3a870

---

_Renamed from "zanie/fix no version" to " Improve error message for dependencies with no versions available" by @zanieb on 2023-11-06 18:16_

---

_@zanieb reviewed on 2023-11-06 18:20_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/snapshots/pip_compile__conflicting_transitive_url_dependency.snap`:21 on 2023-11-06 18:20_

We can still do better here... per #310

---

_@zanieb reviewed on 2023-11-06 19:17_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/snapshots/pip_compile__conflicting_transitive_url_dependency.snap`:21 on 2023-11-06 19:17_

https://github.com/astral-sh/puffin/pull/344 is another step forward

---

_@charliermarsh approved on 2023-11-06 19:47_

---

_Merged by @zanieb on 2023-11-06 20:04_

---

_Closed by @zanieb on 2023-11-06 20:04_

---

_Branch deleted on 2023-11-06 20:04_

---
