```yaml
number: 9294
title: Add aliases for build backend requests
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/init-alias
created_at: 2024-11-20T22:07:38Z
updated_at: 2024-11-21T14:51:19Z
url: https://github.com/astral-sh/uv/pull/9294
synced_at: 2026-01-10T12:00:00Z
```

# Add aliases for build backend requests

---

_Pull request opened by @zanieb on 2024-11-20 22:07_

I found it surprising we did not accept names like `hatchling` or `scikit-build`

---

_Label `cli` added by @zanieb on 2024-11-20 22:07_

---

_@zanieb reviewed on 2024-11-20 22:08_

---

_Review comment by @zanieb on `crates/uv-configuration/src/project_build_backend.rs`:9 on 2024-11-20 22:08_

Is this.. necessary?

---

_@samypr100 reviewed on 2024-11-21 01:39_

---

_Review comment by @samypr100 on `crates/uv-configuration/src/project_build_backend.rs`:26 on 2024-11-21 01:39_

```suggestion
    #[serde(alias = "scikit-build-core")]
    #[value(alias = "scikit-build-core")]
```

I would omit `scikit-build` alias as its a [different build backend ](https://scikit-build.readthedocs.io/en/latest/) than `scikit-build-core`.

---

_Review comment by @zanieb on `crates/uv-configuration/src/project_build_backend.rs`:26 on 2024-11-21 04:20_

Oh gosh those are separate? 😭 need to fix that in the documentation. Thanks!

---

_@zanieb reviewed on 2024-11-21 04:20_

---

_@konstin approved on 2024-11-21 11:24_

---

_Merged by @zanieb on 2024-11-21 14:51_

---

_Closed by @zanieb on 2024-11-21 14:51_

---

_Branch deleted on 2024-11-21 14:51_

---
