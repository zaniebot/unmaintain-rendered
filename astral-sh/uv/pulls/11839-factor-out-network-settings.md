```yaml
number: 11839
title: Factor out network settings
type: pull_request
state: merged
author: jtfmumm
labels:
  - internal
assignees: []
merged: true
base: main
head: jtfm/refactor-network-settings
created_at: 2025-02-27T20:21:41Z
updated_at: 2025-02-28T09:25:33Z
url: https://github.com/astral-sh/uv/pull/11839
synced_at: 2026-01-12T16:10:01Z
```

# Factor out network settings

---

_@jtfmumm_

Three network settings are always passed together (though in random method parameter orders). I'm going to need to add another in the context of upcoming work, so I factored these out into a struct to make changes easier.

---

_@zanieb reviewed on 2025-02-27 20:39_

---

_Review comment by @zanieb on `crates/uv/src/commands/build_frontend.rs`:218 on 2025-02-27 20:39_

Can this just take `NetworkSettings` now?

---

_@jtfmumm reviewed on 2025-02-27 20:40_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/build_frontend.rs`:218 on 2025-02-27 20:40_

Yes!

---

_@zanieb reviewed on 2025-02-27 20:40_

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:102 on 2025-02-27 20:40_

Should this be in like.. `NetworkSettings::resolve(args: &GlobalArgs, workspace: Option<&FilesystemOptions>)`?

---

_@zanieb approved on 2025-02-27 20:41_

Generally into this

---

_@jtfmumm reviewed on 2025-02-27 21:58_

---

_Review comment by @jtfmumm on `crates/uv/src/settings.rs`:102 on 2025-02-27 21:58_

yeah I like that

---

_Merged by @jtfmumm on 2025-02-28 09:05_

---

_Closed by @jtfmumm on 2025-02-28 09:05_

---

_Branch deleted on 2025-02-28 09:05_

---

_Label `internal` added by @konstin on 2025-02-28 09:25_

---
